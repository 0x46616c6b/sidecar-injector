FROM golang:1.19.5-alpine3.17 as build
ARG TARGETPLATFORM
ARG BUILDPLATFORM
RUN echo "Building on $BUILDPLATFORM, for $TARGETPLATFORM" > /log
RUN apk update && apk add git make
WORKDIR /webhook
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN export CGO_ENABLED=0 && make build-webhook

FROM alpine:3.17.1
RUN apk update && apk add --no-cache ca-certificates
RUN mkdir /webhook
COPY --from=build /webhook/bin/webhook /webhook
WORKDIR /webhook
USER nobody
ENTRYPOINT  [ "/webhook/webhook" ]
