### Development

Locally:

pnpm run dev

Access: `http://localhost:8888`

### Build for Production

`dist`.

## Deployment

### Netlify

1. Link your repo to Netlify.
2. Auto-detect settings; confirm to deploy.

Netlify Docs: https://docs.netlify.com/

### Docker

Build and run container:

pnpm run build

Find output in `dist`.

## Deployment

### Netlify

1. Link your repo to Netlify.
2. Auto-detect settings; confirm to deploy.

Netlify Docs: https://docs.netlify.com/

### Docker

Build and run container:

docker buildx build . -t simplyvue:latest
docker run --rm -it -p 8080:80 simplyvue:latest

Visit: `http://localhost:8080`

## Continuous Development

1. Implement.
2. Test.

Further references:

- Vue 3: https://v3.vuejs.org/guide/introduction.html
- Vite: https://vitejs.dev/guide/
