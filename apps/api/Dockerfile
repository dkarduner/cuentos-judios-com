# Stage 1: build
FROM node:18-alpine AS build
RUN npm install -g pnpm
WORKDIR /app

COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY packages ./packages
COPY apps/api ./apps/api

ENV CI=true
RUN pnpm install --frozen-lockfile

RUN pnpm --filter api build

RUN pnpm --filter api deploy --prod --legacy /app/deploy

FROM node:18-alpine AS prod
WORKDIR /app

COPY --from=build /app/deploy .
EXPOSE 3000
CMD ["node", "dist/main"]