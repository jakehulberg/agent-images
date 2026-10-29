# agent-images

Container images for coding agents, one folder per agent. Built for
[Airlock](https://github.com/jakehulberg/airlock) sandboxes, but nothing here depends on Airlock.

| Folder | Image | Agent |
|---|---|---|
| `claude/` | `ghcr.io/jakehulberg/agent-claude` | Claude Code |

## Rules for every image

- **No credentials.** Agents reach their APIs through Infisical Agent Vault. A session sidecar
  writes the proxy settings (`HTTPS_PROXY`, the proxy's CA) to `/session/env`; the proxy adds the
  real key on the way out. If the agent refuses to start without a key, set a placeholder.
- **Runs as a non-root user** and works with a read-only root filesystem, given writable `$HOME`
  and `/tmp`.
- **Idles by default** (`sleep infinity`). Tasks are exec'd in with the prompt on stdin.
- **Quiet:** no auto-update or telemetry calls, which the proxy would block anyway.

## Adding an agent

1. Add `<agent>/Dockerfile`. CI builds every folder with a Dockerfile.
2. In the cluster, add a `SandboxTemplate` using `ghcr.io/jakehulberg/agent-<agent>`, a
   `SandboxWarmPool` with the Airlock annotations for its container and task command, and the
   agent's API host to the Agent Vault access bundle.

## Images

Every push to `main` builds each folder for `linux/arm64` and tags it `YYYYMMDDTHHMMSS-<sha7>` (UTC
build time). All images from one commit share the tag.
