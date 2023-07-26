FROM node:16-alpine AS build
WORKDIR /app
COPY ./package*.json ./
RUN npm ci
COPY . ./
RUN npm run build \
    && rm -rf ./src \
    && rm -rf node ./node_modules

FROM node:16-alpine AS production
WORKDIR /app
COPY ./package*.json ./
RUN npm ci --omit=dev --no-audit && npm i -g -pm2
COPY --from=build /app/dist ./dist
COPY ./ecosystem.config.js .
EXPOSE 4000
CMD [ "pm2-runtime", "start", "ecosystem.config.js"]