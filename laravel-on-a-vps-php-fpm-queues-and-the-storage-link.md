# Laravel on a VPS: PHP-FPM, queues and the storage link

Laravel's happy path on Forge hides several moving parts. On a plain VPS or Compose stack you must wire them yourself.

## Web and workers

PHP-FPM serves HTTP. Queue workers and the scheduler are separate processes. A Compose file that only runs `php-fpm` will accept jobs into the database driver and never execute them. Add a `queue` service with `php artisan queue:work` and a `scheduler` that runs `php artisan schedule:work` (or a host cron calling `schedule:run`).

## Filesystem

```
php artisan storage:link
chown -R www-data:www-data storage bootstrap/cache
```

Uploads written to `storage/app` must live on a volume. Without the link, `/storage/...` URLs 404.

## Environment

Set `APP_ENV=production`, `APP_DEBUG=false`, and `APP_URL` to the public HTTPS URL so generated links and asset URLs are correct behind the proxy. Run `config:cache` and `route:cache` in the release phase; clear them when config changes.

If you use Horizon, give it its own process and confirm Redis persistence settings match how much job loss you can tolerate on reboot.

Guide: [deploy Laravel on a VPS](https://peon.sh/blogs/deploy-laravel-vps).

Disclosure: maintained alongside work on [Peon](https://peon.sh/).
