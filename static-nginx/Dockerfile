FROM node:18-alpine as builder

RUN yarn global add html-minifier@4.0.0 clean-css-cli@5.6.1

WORKDIR /app

ADD public/ .

# minify html css
RUN echo $(html-minifier \
        --collapse-whitespace \
        --remove-comments \
        --use-short-doctype \
        index.html) > index.html && \
    cleancss -o style.css style.css

FROM nginx:1.23.1-alpine as prod

ARG TZ='Europe/Paris'
ENV DEFAULT_TZ ${TZ}

RUN apk --update --no-cache --no-progress add tzdata && \
    cp /usr/share/zoneinfo/${DEFAULT_TZ} /etc/localtime

RUN rm -f /var/log/nginx/*

RUN mkdir /app
COPY --from=builder /app /app
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80