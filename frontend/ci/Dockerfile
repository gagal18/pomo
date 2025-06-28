FROM node:20-bullseye AS build

WORKDIR /app

RUN npm install -g pnpm

COPY package.json pnpm-lock.yaml* ./

RUN pnpm install --frozen-lockfile

COPY . .

RUN pnpm run build

FROM ubuntu:22.04

RUN apt-get update && apt-get install -y nginx && rm -rf /var/lib/apt/lists/*

COPY --from=build /app/dist /var/www/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]