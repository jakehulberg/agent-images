# Claude Code, for Airlock sandboxes. No credentials in here: the session sidecar
# writes the Agent Vault proxy settings to /session/env, and the proxy adds the
# real API key on the way out.
FROM node:22-bookworm-slim

RUN apt-get update \
 && apt-get install -y --no-install-recommends git ca-certificates curl \
 && rm -rf /var/lib/apt/lists/*

RUN npm install -g @anthropic-ai/claude-code@2.1.310 \
 && npm cache clean --force

# Claude Code won't start without a key; the proxy replaces this one. The proxy also
# only allows the bundle's hosts, so skip update checks and telemetry.
# NODE_USE_ENV_PROXY: Node fetch ignores HTTPS_PROXY without it.
ENV ANTHROPIC_API_KEY=placeholder \
    NODE_USE_ENV_PROXY=1 \
    DISABLE_AUTOUPDATER=1 \
    CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1

USER node
WORKDIR /home/node
CMD ["sleep", "infinity"]
