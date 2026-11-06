# FastAPI on a VPS: uvicorn workers, proxy headers and reload traps

FastAPI demos often start with `uvicorn main:app --reload`. That flag is for local development. In production it watches files with extra processes and is a footgun on a server image that also contains build tools.

## A boring production command

```
uvicorn app.main:app --host 0.0.0.0 --port 8000 --workers 2 --proxy-headers --forwarded-allow-ips='*'
```

Tune workers to CPU count and memory. Two workers on a 2 GB VPS is a common start; raise only after measuring. If you use Gunicorn as process manager, `gunicorn -k uvicorn.workers.UvicornWorker` is the usual pattern.

## Proxy headers

Without `--proxy-headers` (and a trusted proxy), `request.url` and HTTPS redirects see `http` and the internal hostname. Configure the reverse proxy to send `X-Forwarded-For` and `X-Forwarded-Proto`, then limit which hops are trusted.

## Docker specifics

- Run migrations in an entrypoint or release job before workers accept traffic.
- Put Postgres on the Compose network and point `DATABASE_URL` at the service name.
- Health checks should hit a lightweight `/health` that verifies DB connectivity.

Pin dependency versions in the image and prefer multi-stage builds so build toolchains never ship in the runtime layer.

A full path from zero to TLS is in [deploy FastAPI on a VPS with Docker](https://peon.sh/blogs/deploy-fastapi-vps-docker).

Disclosure: maintained alongside work on [Peon](https://peon.sh/).
