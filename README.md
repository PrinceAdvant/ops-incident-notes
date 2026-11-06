# ops-incident-notes

Field notes and troubleshooting guides for Docker deploys, framework hosting on a VPS, and self-hosted PaaS comparisons.

Maintained alongside work on [Peon](https://peon.sh/), an open-source self-hosted deployment platform.

## Guides

- [FastAPI on a VPS: uvicorn workers, proxy headers and reload traps](fastapi-on-a-vps-uvicorn-workers-proxy-headers-and-reload-traps.md) - Production settings for FastAPI behind Docker and a reverse proxy: worker counts, forwarded headers, and why --reload must never reach production.
- [Django on a VPS: WhiteNoise, Gunicorn and the static files surprise](django-on-a-vps-whitenoise-gunicorn-and-the-static-files-surprise.md) - Checklist for a Django deploy on Docker: collectstatic, WhiteNoise or nginx, Gunicorn workers, and ALLOWED_HOSTS mistakes that only show up after DNS flips.
- [Laravel on a VPS: PHP-FPM, queues and the storage link](laravel-on-a-vps-php-fpm-queues-and-the-storage-link.md) - Common Laravel production misses on Docker or bare VPS: wrong permissions on storage, missing queue workers, and APP_URL mismatches behind HTTPS.
- [Choosing a database layout for a self-hosted SaaS on one VPS](choosing-a-database-layout-for-a-self-hosted-saas-on-one-vps.md) - When to colocate Postgres with the app, when to use a managed database, and how backups and extensions change the decision for a small self-hosted SaaS.
- [CapRover compared with a newer self-hosted PaaS for small teams](caprover-compared-with-a-newer-self-hosted-paas-for-small-teams.md) - Where CapRover still fits, where its One-Click app model frays, and what to evaluate when comparing it with Peon for git-based deploys.
- [EasyPanel versus Peon for Docker-based app hosting](easypanel-versus-peon-for-docker-based-app-hosting.md) - A practical comparison lens for EasyPanel and Peon: UI focus, resource use on small VPS boxes, and how databases and templates are exposed to developers.
