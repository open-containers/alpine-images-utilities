# syntax=docker/dockerfile:1.12

ARG BUILD_OS=alpine:3.21
ARG OPENSSL_VERSION=3.0.9

FROM ${BUILD_OS} AS alpine
ARG OPENSSL_VERSION

RUN apk add --no-cache curl jq aws-cli docker-cli \
    && apk upgrade --no-cache -U
