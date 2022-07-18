
VERSION 0.6
FROM node:lts-slim
WORKDIR /app

deps:
    COPY package.json yarn.lock ./
    RUN yarn install
    SAVE ARTIFACT package.json AS LOCAL ./package.json
    SAVE ARTIFACT yarn.lock AS LOCAL ./yarn.lock

source:
    FROM +deps
    COPY src src
    COPY nest-cli.json tsconfig.json tsconfig.build.json .

build:
    FROM +source
    RUN yarn build
    SAVE ARTIFACT dist /dist AS LOCAL dist

test:
    FROM +source
    COPY test test
    RUN yarn test
    RUN yarn test:e2e

lint:
    FROM +source
    COPY test test
    COPY .eslintrc.js .prettierrc .
    RUN yarn format:check
    RUN yarn lint:check


docker:
    FROM +deps
    COPY +build/dist ./dist
    EXPOSE 3000
    ENTRYPOINT ["yarn", "start:prod"]
    SAVE IMAGE --push earthly-poc