# Django on a VPS: WhiteNoise, Gunicorn and the static files surprise

Django deploys fail in predictable ways: `DisallowedHost`, missing static files in production, and workers that die during peak because `DEBUG` was left on somehow.

## Minimum production settings

- `DEBUG=False`
- `ALLOWED_HOSTS` includes the real domain and any health-check hostname
- `CSRF_TRUSTED_ORIGINS` includes `https://your.domain`
- A strong `SECRET_KEY` from the environment, never from settings committed to git

## Static and media

Run `collectstatic` during the image build or release phase. Serve static with WhiteNoise or nginx; do not rely on Django's dev static server. Media files belong on a volume or object storage, not inside the container writable layer.

## Process model

Gunicorn with 2 to 3 workers behind a reverse proxy is enough for many apps. Use a separate worker process (systemd or Compose service) for Celery or RQ if you have background jobs. Do not run `runserver` behind nginx.

## Database

Point at Postgres with `CONN_MAX_AGE` tuned carefully behind PgBouncer if you scale workers. Take logical backups before every migrate on production.

After DNS cutover, watch 400 DisallowedHost spikes in logs for an hour; forgotten www or apex hosts are the usual cause.

Walkthrough: [deploy Django on a VPS](https://peon.sh/blogs/deploy-django-vps).

Disclosure: maintained alongside work on [Peon](https://peon.sh/).
