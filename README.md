# OXIMAGE

**Fast batch photo processing for e-shops, websites and social media.**

Upload many photos at once, then rotate, crop, resize, convert and compress them in one pass. Download the results as a ZIP or rename them in bulk. Originals and results are deleted automatically after a retention period, so your photos are not kept longer than needed.

🌐 **Live:** https://oximage.oxlab.app

---

## Features

- **Batch upload** – drag and drop many photos; limits per plan (files per batch, batch size, file size, megapixels)
- **Automatic orientation** – EXIF orientation is applied first
- **Rotate, flip and crop** – any angle, horizontal or vertical flip, manual crop by dragging, or centred ratio crop (1:1, 4:3, 3:2, 16:9, 9:16)
- **Resize** – downscale only, to 800 / 1024 / 1080 / 1200 / 1600 / 1920 px
- **Formats and quality** – keep the original format, or convert to JPG, PNG, WebP or AVIF; quality 1–100
- **Presets** – ready-made presets for e-shops, blog, Instagram post and story, Facebook cover, YouTube thumbnail and marketplaces; custom presets on supporter plans
- **Versions** – every edit is a new version of the same photo; a version can be cropped again or processed further
- **Bulk actions** – select photos, download as ZIP, delete, or rename with a pattern such as `product-{n}`
- **Privacy by design** – originals and results are removed automatically after the retention period of your plan; photos are never stored in the database
- **Remaining quota** – you always see how many photos you can still upload in the last 24 hours
- **Bilingual** – Czech and English, with the language remembered per account
- **Email sign-in** – one-time PIN codes or password, with email verification at registration

## Plans

| Plan | Who it is for | Highlights |
|------|---------------|------------|
| **Free** | Everyone | Limited photos per day, retention of 24 hours |
| **Supporter** | People who support the project | More photos per day, larger batches, custom presets |

Supporter is a voluntary contribution, not a subscription. You can support the project on [Buy me a coffee](https://ko-fi.com/L3L71YW62P), and the supporter plan is then activated for the number of days your contribution covers.

## Tech stack

- **Backend:** PHP 8.3, plain PHP without a framework, MySQL / MariaDB
- **Images:** PHP GD (JPEG, PNG, WebP, AVIF, GIF input)
- **Frontend:** server-rendered pages, Tailwind CSS 4, a small amount of vanilla JavaScript
- **Email:** PHPMailer over SMTP
- **Background work:** a worker that processes queued photos sequentially, started after each upload and by cron every minute as a backup
- **Deployment:** Docker Compose (PHP + Apache, cron worker, MariaDB, phpMyAdmin)

## Project structure

```
public/      web root: pages, upload endpoints, assets
includes/    application logic (auth, upload, processing, mail, i18n, layout)
lang/        translations (cs, en)
bin/         worker, migrations, admin creation
database/    schema
tests/       end-to-end test suite (bash + curl)
docker/      Docker Compose setup for production
```

## Running locally

Requirements: PHP 8.3 with `gd`, `pdo_mysql`, `exif`, `zip`; MySQL or MariaDB; Composer; Node.js (for the CSS build).

```bash
composer install
npm install
npm run build:css
cp .env.example .env        # set database and SMTP values
php bin/migrate.php         # creates the database tables
php bin/create-admin.php admin@example.com "Admin name" "a-long-password"
```

Then open the project in your web server (for example `http://localhost/oximage`).

The worker processes queued photos:

```bash
php bin/worker.php
```

In production, run it from cron every minute (see `docker/oximage/crontab`).

## Running the tests

```bash
bash tests/e2e.sh
```

The script creates its own test database, starts a temporary PHP server and runs the full scenario set (sign-in, uploads, processing, plans, limits, support and admin pages).

## Privacy

- Uploaded originals and results are deleted automatically after the retention period of the plan.
- Passwords and PIN codes are stored only as hashes.
- No third-party analytics or advertising cookies are used. Details are in the [cookie policy](https://oximage.oxlab.app/cookies.php) and the [privacy policy](https://oximage.oxlab.app/privacy.php).

## Contact

OXLAB · office@oxlab.app · https://oximage.oxlab.app
