<a target="_blank" href="https://ninjaportal.net" target="_blank">
 <img src="https://i.ibb.co/Rpmq93Lr/Copy-of-Ninja-Tech-logo-template-1.png" alt="Ninja Portal Logo" height="180px" />   
</a>

# NinjaPortal Kickstart

Starter Laravel application template prepared for NinjaPortal.

Use this template to bootstrap a production-ready NinjaPortal application with the
Portal package, Filament admin, and the Shadow theme already wired in.

## Requirements

- PHP `^8.2`
- Composer 2
- A database (MySQL recommended for real projects)
- Node.js + npm (only if you want to build frontend assets)

## 1. Create The Project

Install the template through Composer:

```bash
composer create-project ninjaportal/kickstart your-project-name
cd your-project-name
```

## 2. Install Dependencies

If you are working from the repository directly instead of `create-project`, install the locked dependencies:

```bash
composer install
```

Then install JS dependencies if needed:

```bash
npm install
```

## 3. Configure Environment

Copy the environment file and generate the application key:

```bash
cp .env.example .env
php artisan key:generate
```

Update database credentials in `.env`, then configure NinjaPortal / Apigee values (already included in `.env.example`):

```dotenv
APIGEE_PLATFORM=edge
APIGEE_ENDPOINT=https://api.enterprise.apigee.com/v1
APIGEE_ORGANIZATION=your-org
APIGEE_USERNAME=your-username
APIGEE_PASSWORD=your-password

APIGEE_MONETIZATION_ENABLED=false
APIGEE_MONETIZATION_PLATFORM=edge
APIGEE_MONETIZATION_ENDPOINT=https://api.enterprise.apigee.com/v1/mint
```

if using Apigee X:
```dotenv
APIGEE_DRIVER=apigeex
APIGEE_ENDPOINT=https://apigee.googleapis.com/v1
APIGEE_ORGANIZATION=your-org
```

## 4. Installations

### 4.1 Install NinjaPortal
Run the package installer:

```bash
php artisan portal:install
```

What this does:

- publishes NinjaPortal config
- publishes Spatie permission migrations
- runs migrations
- seeds baseline settings + RBAC permissions
- adds `App\Providers\NinjaPortalServiceProvider` to your application bootstrap providers

Optional flags:

```bash
php artisan portal:install --force-provider-overwrite
php artisan portal:install --delete-default-users-migration
```

We recommend using the `--delete-default-users-migration` flag if you are installing NinjaPortal on a fresh laravel project, 
as it will delete the default `users` table migration.

### 4.2 Install Shadow theme

```bash
php artisan shadow:install
```
Shadow theme is shipped with pre-built assets.

### 4.3 Install Filament Admin 
```bash
php artisan filament:install 
```

This will install Filament Admin and its dependencies.


## 5. (Optional) Seed Demo Data

```bash
php artisan portal:seed --demo
```

Useful seed options:

```bash
php artisan portal:seed --all
php artisan portal:seed --settings --rbac
```

### Seeded Demo Accounts

After running `php artisan portal:seed --demo`, the following demo accounts are available:

| Type | Name | Email | Password | Notes |
|---|---|---|---|---|
| Admin | Portal Owner | `admin@ninjaportal.test` | `password` | Assigned the `super_admin` role when RBAC is seeded |
| Admin | Support Admin | `support.admin@ninjaportal.test` | `password` | Assigned the `super_admin` role when RBAC is seeded |
| User | Jade Summers | `jade.summers@ninjaportal.test` | `password` | Active user |
| User | Marco Diaz | `marco.diaz@ninjaportal.test` | `password` | Active user |
| User | Priya Nair | `priya.nair@ninjaportal.test` | `password` | Pending user |

## 6. Apigee Authentication Notes

### Apigee Edge

Use the username/password env variables shown above.

### Apigee X

Set:

```dotenv
APIGEE_PLATFORM=apigeex
APIGEE_ENDPOINT=https://apigee.googleapis.com/v1
APIGEE_MONETIZATION_PLATFORM=apigee_x
APIGEE_MONETIZATION_ENDPOINT=https://apigee.googleapis.com/v1
```

Then place a service account key file at:

```text
storage/app/service_account_key.json
```

LaraApigee uses that file path for Apigee X authentication by default.

## 7. Run The Application

Backend:

```bash
php artisan serve
```

Assets (optional during local development):

```bash
npm run dev
```

## Optional Next Step: API Package

If you want the prebuilt NinjaPortal REST API package as well:

```bash
composer require ninjaportal/portal-api
```

Then publish/configure its settings and generate API docs (Scribe) based on your project needs.
