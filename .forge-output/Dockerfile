

FROM debian:bookworm-slim

LABEL org.opencontainers.image.title="demo-weather" \
      org.opencontainers.image.version="0.1.0"
LABEL ai.initializ.forge.egress-profile="strict" \
      ai.initializ.forge.egress-mode="allowlist"
LABEL ai.initializ.forge.tool-interface-version="1.0"
LABEL ai.initializ.forge.skills-count="2"
# Required binaries (from skill requirements):
# - curl
# - jq

ENV FORGE_MODEL_NAME="gpt-5.4"
ENV FORGE_MODEL_PROVIDER="openai"

ARG FORGE_DEV=false

WORKDIR /app

COPY . .
RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates curl jq && rm -rf /var/lib/apt/lists/*
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates && \
    ARCH=$(uname -m | sed 's/aarch64/arm64/;s/x86_64/x86_64/') && \
    curl -fsSL -o /tmp/forge.tar.gz "https://github.com/initializ/forge/releases/download/vnext/forge-Linux-${ARCH}.tar.gz" && \
    tar xz -C /usr/local/bin forge -f /tmp/forge.tar.gz && rm /tmp/forge.tar.gz && \
    chmod +x /usr/local/bin/forge && \
    rm -rf /var/lib/apt/lists/*
EXPOSE 8080

ENTRYPOINT ["forge","run","--host","0.0.0.0"]
