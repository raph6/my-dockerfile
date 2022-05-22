FROM node:18.1.0-alpine3.15 as builder

WORKDIR /app
ADD package.json yarn.lock ./
RUN yarn

ADD . .
RUN yarn run gulp && rm -rf node_modules && yarn install --prod && \
    rm -rf \ 
      .git \
      .github \
      src \
      .gitignore \
      Dockerfile \
      dump.sql \
      gulpfile.js \
      nodemon.json \
      package.json \
      run-dev.sh \
      tsconfig.json \
      yarn.lock

FROM astefanutti/scratch-node:17

COPY --from=builder /app /

EXPOSE 3000

ENTRYPOINT ["node", "dist/index.js"]