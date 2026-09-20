# Render deployment

This checkout includes a small Render-specific adaptation for the Web runner.

## Why the source was changed

Render web services require the HTTP server to listen on `0.0.0.0` and on the
port in `$PORT`. The upstream `dsh web` CLI intentionally rejects `0.0.0.0`
for local safety, so this checkout permits that bind only when Render's
`RENDER=true` runtime variable is present. Local runs remain loopback-only.

When running on Render, the Web bundle also automatically adds
`RENDER_EXTERNAL_HOSTNAME` to its trusted-host list, preserving the browser
Host/Origin trust boundary for the public Render hostname.

Render documents the `0.0.0.0` + `$PORT` requirement and provides
`RENDER_EXTERNAL_HOSTNAME` to web services.

## Render settings

**Build Command**

```text
pnpm install --frozen-lockfile --ignore-scripts && pnpm run build
```

**Start Command**

```text
pnpm dsh web --host 0.0.0.0 --port $PORT --no-open
```

**Plan**: Free

No provider/API key is hardcoded by this deployment configuration. Configure
the provider credentials supported by your Harness setup as Render environment
variables/secrets.

## Security note

The Render exception is deliberately narrow: `0.0.0.0` is accepted only when
`RENDER=true`. Do not use this change as a reason to expose a local Harness
instance directly to the LAN or Internet.
