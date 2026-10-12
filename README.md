# Plugin Registry Server - Laravel API

**English** | [繁體中文](README.zh-TW.md)

## 📋 Overview

A Plugin Registry Server API built with Laravel. It provides plugin management, versioning, and user authentication.

## 🚀 Quick start

### 1. Install dependencies

```bash
composer install
```

### 2. Set environment variables

Copy `.env.example` to `.env` and configure it:

```bash
cp .env.example .env
php artisan key:generate
```

### 3. Configure the database

Set the database connection in `.env`:

```env
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=plugin_registry
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

### 4. Configure S3 (optional)

To use S3 storage:

```env
FILESYSTEM_DISK=s3
AWS_ACCESS_KEY_ID=your_key
AWS_SECRET_ACCESS_KEY=your_secret
AWS_DEFAULT_REGION=ap-northeast-1
AWS_BUCKET=plugin-registry-storage
```

### 5. Run migrations

```bash
php artisan migrate
```

### 6. Start the development server

```bash
php artisan serve
```

The API runs at `http://localhost:8000`.

## 📁 Project structure

```
api/
├── app/
│   ├── Http/
│   │   └── Controllers/
│   │       └── Api/
│   │           ├── AuthController.php      # Authentication
│   │           ├── PluginController.php    # Plugins
│   │           ├── PluginVersionController.php  # Versions
│   │           ├── SearchController.php    # Search
│   │           └── StatsController.php    # Statistics
│   ├── Models/
│   │   ├── User.php
│   │   ├── Plugin.php
│   │   ├── PluginVersion.php
│   │   ├── PluginReview.php
│   │   ├── PluginDependency.php
│   │   └── ApiKey.php
│   └── Services/
│       └── StorageService.php              # S3 storage service
├── database/
│   └── migrations/                          # Database migrations
├── routes/
│   └── api.php                              # API routes
└── config/
    ├── jwt.php                              # JWT config
    └── filesystems.php                      # Filesystem config
```

## 🔌 API endpoints

### Auth API

- `POST /api/v1/auth/register` - Register
- `POST /api/v1/auth/login` - Log in
- `POST /api/v1/auth/refresh` - Refresh token
- `POST /api/v1/auth/logout` - Log out (auth required)
- `GET /api/v1/auth/me` - Get current user (auth required)

### Plugin API

- `GET /api/v1/plugins` - List all plugins
- `GET /api/v1/plugins/{plugin}` - Get plugin details
- `POST /api/v1/plugins` - Publish a new plugin (auth required)
- `PUT /api/v1/plugins/{plugin}` - Update a plugin (auth required)
- `DELETE /api/v1/plugins/{plugin}` - Delete a plugin (auth required)

### Version API

- `GET /api/v1/plugins/{plugin}/versions` - List all versions
- `GET /api/v1/plugins/{plugin}/versions/{version}` - Get a specific version
- `POST /api/v1/plugins/{plugin}/versions` - Publish a new version (auth required)
- `GET /api/v1/plugins/{plugin}/versions/{version}/download` - Download a plugin (auth required)

### Search API

- `GET /api/v1/search` - Advanced search

### Stats API

- `GET /api/v1/stats` - Platform statistics

## 🔐 Authentication

The API uses JWT (JSON Web Token) authentication.

### Usage

Add this request header:

```
Authorization: Bearer {token}
```

### Getting a token

```bash
curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "password"
  }'
```

Response:

```json
{
  "success": true,
  "data": {
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
    "token_type": "bearer",
    "expires_in": 3600,
    "user": {...}
  }
}
```

## 📦 Database schema

- **users** - users
- **plugins** - plugins
- **plugin_versions** - plugin versions
- **plugin_reviews** - plugin reviews
- **plugin_dependencies** - plugin dependencies
- **api_keys** - API keys

## 🛠️ Development

### Run tests

```bash
php artisan test
```

### Migrations

```bash
# Create a migration
php artisan make:migration create_table_name

# Run migrations
php artisan migrate

# Roll back
php artisan migrate:rollback
```

### Create a model

```bash
php artisan make:model ModelName
```

### Create a controller

```bash
php artisan make:controller Api/ControllerName
```

## 📚 References

- [Laravel docs](https://laravel.com/docs)
- [JWT Auth docs](https://jwt-auth.readthedocs.io/)
- [AWS S3 SDK](https://docs.aws.amazon.com/sdk-for-php/)

## 🔗 Agent-Foundry integration

The Agent-Foundry CLI uses this API like this:

```python
# Set the registry URL
REMOTE_REGISTRY_URL = "https://registry.agent-foundry.org/api/v1"

# Download a plugin
response = requests.get(
    f"{REMOTE_REGISTRY_URL}/plugins/{name}/versions/{version}/download",
    headers={"Authorization": f"Bearer {token}"}
)
```

---

**Work in progress** — more features coming soon.
