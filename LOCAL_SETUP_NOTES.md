# Paperclip local setup notes

## Status
- Installed at: `/root/paperclip`
- Runtime config: `/root/.paperclip/instances/default/config.json`
- Health check: `http://127.0.0.1:3100/api/health`
- UI: `http://localhost:3100`
- DB: embedded PostgreSQL on `127.0.0.1:54329`

## Fixes applied locally
Two runtime bugs were patched to allow embedded PostgreSQL to start correctly when running as root on this host:
- `packages/db/src/migration-runtime.ts`
- `server/src/index.ts`

Patch summary:
- enabled `createPostgresUser: true` when constructing `embedded-postgres`

Also set explicit auth base URL in config:
- `auth.baseUrlMode = explicit`
- `auth.publicBaseUrl = http://localhost:3100`

## Current run mode
Currently running via nohup/manual process.

## To install as a managed systemd service later
Service file already prepared at:
- `/etc/systemd/system/paperclip.service`

Run this exact command as root:
```sh
systemctl daemon-reload && systemctl enable paperclip && systemctl restart paperclip && sleep 8 && systemctl --no-pager --full status paperclip | sed -n '1,40p'
```

## Bootstrap invite
Check latest invite URL in:
- `/tmp/paperclip-start.log`

## Quick checks
```sh
curl -s http://127.0.0.1:3100/api/health
ss -ltnp | grep ':3100\|:54329'
ps -ef | grep 'cli/src/index.ts run' | grep -v grep
```
