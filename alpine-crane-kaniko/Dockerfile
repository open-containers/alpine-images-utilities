# syntax=docker/dockerfile:1.12

# Stage 1: Build crane
ARG BUILD_OS=alpine:3.21
ARG OPENSSL_VERSION=3.0.9

FROM ${BUILD_OS} AS builder
ARG OPENSSL_VERSION

# Install dependencies and crane
RUN apk add --no-cache curl jq && \
    curl -L https://github.com/google/go-containerregistry/releases/latest/download/go-containerregistry_Linux_x86_64.tar.gz | tar xz && \
    mv crane /usr/local/bin/crane

# Stage 2: Create final Kaniko + crane image
FROM gcr.io/kaniko-project/executor:v1.23.2-debug AS kaniko

# Copy crane from builder stage
COPY --from=builder /usr/local/bin/crane /usr/local/bin/crane

# Manually create non-root user and set up /etc/passwd if missing
# RUN [ ! -f /etc/passwd ] && echo "Creating /etc/passwd file..." && echo "root:x:0:0:root:/root:/bin/sh" > /etc/passwd || true

# RUN echo "craneuser:x:1000:1000:Crane User:/home/craneuser:/bin/sh" >> /etc/passwd && \
#     mkdir -p /home/craneuser && \
#     chown -R 1000:1000 /home/craneuser

# Switch to non-root user
# USER 1000

# Set the entry point to Kaniko
ENTRYPOINT ["/kaniko/executor"]
