ARG GO_VERSION=1.26.1
ARG NODE_VERSION=24.14.1

FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

ARG GO_VERSION
ARG NODE_VERSION
ARG HUGO_VERSION=0.160.0
ARG DART_SASS_VERSION=1.99.0

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN arch="$(dpkg --print-architecture)" \
    && case "$arch" in \
        amd64) go_arch="amd64"; hugo_arch="amd64"; sass_arch="x64"; node_arch="x64" ;; \
        arm64) go_arch="arm64"; hugo_arch="arm64"; sass_arch="arm64"; node_arch="arm64" ;; \
        *) echo "Unsupported architecture: $arch" >&2; exit 1 ;; \
      esac \
    && apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates curl tar xz-utils \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p /usr/local/lib/nodejs \
    && curl -fsSL "https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-${node_arch}.tar.xz" -o /tmp/node.tar.xz \
    && tar -C /usr/local/lib/nodejs -xJf /tmp/node.tar.xz \
    && ln -sfn "/usr/local/lib/nodejs/node-v${NODE_VERSION}-linux-${node_arch}" /usr/local/lib/nodejs/current \
    && ln -sf /usr/local/lib/nodejs/current/bin/node /usr/local/bin/node \
    && ln -sf /usr/local/lib/nodejs/current/bin/npm /usr/local/bin/npm \
    && ln -sf /usr/local/lib/nodejs/current/bin/npx /usr/local/bin/npx \
    && ln -sf /usr/local/lib/nodejs/current/bin/corepack /usr/local/bin/corepack \
    && rm /tmp/node.tar.xz \
    && rm -rf /usr/local/go \
    && curl -fsSL "https://go.dev/dl/go${GO_VERSION}.linux-${go_arch}.tar.gz" -o /tmp/go.tar.gz \
    && tar -C /usr/local -xzf /tmp/go.tar.gz \
    && rm /tmp/go.tar.gz \
    && curl -fsSL "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-${hugo_arch}.tar.gz" -o /tmp/hugo.tar.gz \
    && tar -C /usr/local/bin -xzf /tmp/hugo.tar.gz hugo \
    && rm /tmp/hugo.tar.gz \
    && curl -fsSL "https://github.com/sass/dart-sass/releases/download/${DART_SASS_VERSION}/dart-sass-${DART_SASS_VERSION}-linux-${sass_arch}.tar.gz" -o /tmp/dart-sass.tar.gz \
    && tar -C /usr/local/lib -xzf /tmp/dart-sass.tar.gz \
    && ln -sf /usr/local/lib/dart-sass/sass /usr/local/bin/sass \
    && rm /tmp/dart-sass.tar.gz

ENV PATH="/usr/local/go/bin:/usr/local/lib/nodejs/current/bin:${PATH}"

WORKDIR /workspaces/quikckstart