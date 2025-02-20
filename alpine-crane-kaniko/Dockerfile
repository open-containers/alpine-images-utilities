# syntax=docker/dockerfile:1.12

# Stage 1: Build crane
ARG BUILD_OS=alpine:3.21
ARG OPENSSL_VERSION=3.0.9

FROM ${BUILD_OS} AS builder
ARG OPENSSL_VERSION

# Install crane
RUN apk add --no-cache curl jq aws-cli docker-cli && \
    curl -L https://github.com/google/go-containerregistry/releases/latest/download/go-containerregistry_Linux_x86_64.tar.gz | tar xz && \
    mv crane /usr/local/bin/crane

# Stage 2: Create final Kaniko + crane image
FROM gcr.io/kaniko-project/executor:v1.23.2-debug AS kaniko

# Copy crane from builder stage
COPY --from=builder /usr/local/bin/crane /usr/local/bin/crane

# Create a non-root user for running the crane
RUN adduser -D -u 1000 craneuser

# Switch to non-root user
USER 1000

# Set the entry point to Kaniko
ENTRYPOINT ["/kaniko/executor"]
