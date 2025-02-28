<script setup lang="ts">
import { useI18n } from "vue-i18n";
import { themeList } from "./data";
import type { CustomTheme } from "~/layouts/Navbar/components/ThemeChange/types";

defineOptions({
  name: "ThemeChange",
});
const { t } = useI18n();
const showDropdown = ref(false);
const mode = useColorMode<CustomTheme>({
  attribute: "data-theme",
  modes: {
    cupcake: "cupcake",
    bumblebee: "bumblebee",
    emerald: "emerald",
    corporate: "corporate",
    synthwave: "synthwave",
    retro: "retro",
    cyberpunk: "cyberpunk",
    valentine: "valentine",
    halloween: "halloween",
    garden: "garden",
    forest: "forest",
    aqua: "aqua",
    lofi: "lofi",
    pastel: "pastel",
    fantasy: "fantasy",
    wireframe: "wireframe",
    black: "black",
    luxury: "luxury",
    dracula: "dracula",
    cmyk: "cmyk",
    autumn: "autumn",
    business: "business",
    acid: "acid",
    lemonade: "lemonade",
    night: "night",
    coffee: "coffee",
    winter: "winter",
  },
});

function changeTheme(event: MouseEvent, theme: CustomTheme) {
  const isSameTheme = mode.value === theme;
  try {
    // eslint-disable-next-line ts/ban-ts-comment
    // @ts-expect-error
    if (document.startViewTransition === undefined) {
      throw new Error("document.startViewTransition is undefined, please update your browser to the latest version or use a modern browser.");
    }
    const x = event.clientX;
    const y = event.clientY;
    const endRadius = Math.hypot(
      Math.max(x, innerWidth - x),
      Math.max(y, innerHeight - y),
    );
    // eslint-disable-next-line ts/ban-ts-comment
    // @ts-expect-error
    const transition = document.startViewTransition();
    transition.ready.then(() => {
      const clipPath = [
        `circle(0px at ${x}px ${y}px)`,
        `circle(${endRadius}px at ${x}px ${y}px)`,
      ];
      document.documentElement.animate(
        {
          clipPath: isSameTheme ? [...clipPath].reverse() : clipPath,
        },
        {
          duration: 500,
          easing: "ease-in",
          pseudoElement: isSameTheme
            ? "::view-transition-old(root)"
            : "::view-transition-new(root)",
        },
      );
    });
  } catch (error: unknown) {
    console.warn(`Failed to change theme : ${error instanceof Error ? error.message : ""}`);
  } finally {
    mode.value = theme;
  }
}
</script>

<template>
  <nav title="Change Theme" class="relative dropdown dropdown-end" @mouseenter="showDropdown = true" @mouseleave="showDropdown = false">
    <div tabindex="0" class="btn btn-square btn-ghost">
      <UnoCSSIconButton icon="i-tabler-color-swatch mb-n2 mt-2" :class="{ wakawaka: showDropdown }" class="transition-transform" />
      <span class="hidden md:inline">{{ t("") }}</span>
      <UnoCSSIconButton icon="i-tabler-chevron-down" :class="{ 'rotate-180': showDropdown }" class="transition-transform" />
    </div>
    <div
      class="dropdown-content scrollbar absolute right-0 top-full mt-2 h-[70vh] max-h-96 w-52 overflow-y-auto bg-base-200 text-base-content shadow-2xl rounded-box"
    >
      <div class="grid grid-cols-1 gap-3 p-3" tabindex="0">
        <div
          v-for="theme in themeList"
          :key="theme.id"
          class="overflow-hidden rounded-lg outline-2 outline-base-content outline-offset-2 hover:outline"
          :class="mode === theme.id ? 'outline' : ''"
          @click="changeTheme($event, theme.id)"
        >
          <div
            :data-theme="theme.id"
            class="w-full cursor-pointer bg-base-100 text-base-content font-sans"
          >
            <div class="grid grid-cols-5 grid-rows-3">
              <div class="col-span-5 row-span-3 row-start-1 flex gap-1 px-4 py-3">
                <div class="grow text-sm font-bold">
                  {{ theme.id }}
                </div>
                <div class="flex shrink-0 flex-wrap gap-1">
                  <div class="w-2 rounded bg-primary" />
                  <div class="w-2 rounded bg-secondary" />
                  <div class="w-2 rounded bg-accent" />
                  <div class="w-2 rounded bg-neutral" />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<style scoped>
nav {
  z-index: 999;
}
.mb-n2,
.mt-2 {
  margin-bottom: -0.5rem;
  margin-top: 0.5rem;
}
.transition-transform {
  transition: transform 0.3s ease;
}
.rotate-180 {
  transform: rotate(180deg);
}
.wakawaka {
  animation: wakawaka 2s ease-in-out infinite;
}
@keyframes wakawaka {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-0.25rem);
  }
}
.scrollbar::-webkit-scrollbar {
  width: 20px;
  height: 20px;
}

.scrollbar::-webkit-scrollbar-track {
  border-radius: 100vh;
  background: #f7f4ed;
}

.scrollbar::-webkit-scrollbar-thumb {
  background: #e0cbcb;
  border-radius: 100vh;
  border: 3px solid #f6f7ed;
}

.scrollbar::-webkit-scrollbar-thumb:hover {
  background: #c0a0b9;
}

::view-transition-old(root),
::view-transition-new(root) {
  animation: none;
  mix-blend-mode: normal;
}

::view-transition-old(root) {
  z-index: 999;
}
::view-transition-new(root) {
  z-index: 1;
}
</style>
