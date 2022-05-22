FROM golang:alpine
ARG GITHUB_LOGIN
ARG GITHUB_TOKEN
RUN apk add -U --no-cache git ca-certificates
RUN git config --global url."https://${GITHUB_LOGIN}:${GITHUB_TOKEN}@github.com".insteadOf "https://github.com"
WORKDIR /sysa
COPY go.mod go.sum /sysa/
RUN go mod download && go mod verify
COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -ldflags="-w -s" .
RUN echo "nobody:x:65534:65534:Nobody:/:" > /etc_passwd


FROM scratch

COPY --from=0 /etc_passwd /etc/passwd
COPY --from=0 /sysa/sysa-go /sysa
COPY --from=0 /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

USER nobody

EXPOSE 8070
ENV GIN_MODE=release
ENTRYPOINT ["/sysa"]