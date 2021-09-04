FROM node:16-alpine3.11 AS deps

WORKDIR /app/
COPY package.json /app/
COPY yarn.lock /app/
RUN yarn install --frozen-lockfile

FROM node:16-alpine3.11 AS build
WORKDIR /app/
COPY --from=deps /app/node_modules /app/node_modules
COPY package.json /app/
COPY tsconfig.json /app/
COPY ./src/*.ts /app/src/
RUN yarn compile
RUN yarn build

FROM alpine
RUN apk update && \
    apk add --no-cache libstdc++ libgcc ca-certificates && \
    rm -rf /var/cache/apk/*
WORKDIR /app/
COPY --from=build /app/dist/ .
CMD ./myhello
