FROM node:18.1.0-alpine3.15 as builder

WORKDIR /app
ADD package.json yarn.lock ./
RUN yarn

ADD . .
RUN yarn run build

# minify public html
RUN yarn global add html-minifier
RUN echo $(html-minifier \
        --collapse-whitespace \
        --remove-comments \
        --use-short-doctype \
        dist/index.html) > dist/index.html

FROM nginx:1.21.6-alpine as prod

ARG TZ='Europe/Paris'
ENV DEFAULT_TZ ${TZ}

RUN apk --update --no-cache --no-progress add tzdata && \
    cp /usr/share/zoneinfo/${DEFAULT_TZ} /etc/localtime

RUN rm -f /var/log/nginx/*

RUN mkdir /app
COPY --from=builder /app/dist /app
COPY --from=builder /app/404.html /usr/share/nginx/html/404.html
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
