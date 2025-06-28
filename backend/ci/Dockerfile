FROM node:20-alpine AS dev

WORKDIR /app

RUN apk add --no-cache libc6-compat

RUN npm install -g pnpm

COPY package.json pnpm-lock.yaml* ./

RUN pnpm install --frozen-lockfile

COPY . .

FROM node:20-alpine AS build

WORKDIR /app

RUN apk add --no-cache libc6-compat
RUN npm install -g pnpm

ENV NODE_ENV=production

COPY --chown=node:node --from=dev /app/node_modules ./node_modules
COPY --chown=node:node . .

RUN pnpm run build


USER node

FROM node:20-alpine AS prod

WORKDIR /app

RUN apk add --no-cache libc6-compat

ENV NODE_ENV=production

COPY --chown=node:node --from=build /app/dist ./dist
COPY --chown=node:node --from=build /app/node_modules ./node_modules

USER node

CMD ["node", "dist/main.js"]
