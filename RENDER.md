# DeepSeek Harness on Render

This repository includes a Render Blueprint and a deployment overlay for the
DeepSeek Harness Web UI.

## Deploy

1. Push this repository to GitHub.
2. In Render, choose **New -> Blueprint** and select the repository.
3. Render reads `render.yaml` and creates the `deepseek-harness` Web Service.
4. When prompted for `DEEPSEEK_API_KEY`, enter your DeepSeek API key.
5. Deploy.

The service uses the Render Free plan. Render supplies `PORT` at runtime; the
`render.patch.yml` overlay binds the Harness web server to `0.0.0.0` and uses
that port. The normal DSH CLI safety check for `--host 0.0.0.0` remains intact
because the deployment does not pass that CLI flag.

## Manual Render settings

If creating the service manually instead of using the Blueprint:

- Runtime: Node
- Plan: Free
- Build command: `corepack enable && pnpm install --frozen-lockfile && pnpm run build`
- Start command: `pnpm dsh --profile web --no-open --patch ./render.patch.yml`
- Environment: `NODE_ENV=production`
- Secret: `DEEPSEEK_API_KEY=<your key>`

Do not commit API keys to Git.

## Free-tier limitations

Render Free web services have limited CPU/RAM and can spin down after idle
periods. The filesystem is ephemeral, so local sessions/files are not durable
across restarts or redeploys.
