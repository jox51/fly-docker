# Redis on Fly.io with Laravel

This repository contains the configuration files needed to deploy a Redis instance on Fly.io, specifically designed to work with Laravel applications.

## Overview

This project provides a minimal setup for running Redis on Fly.io using the official Redis Docker image. The setup includes proper configuration for authentication, networking, and persistence.

## Files

The deployment consists of three main configuration files:

- `Dockerfile`: Uses the official Redis image and copies custom configuration
- `redis.conf`: Redis server configuration with security and networking settings
- `fly.toml`: Fly.io deployment configuration

## Key Configuration Details

### Redis Configuration

The `redis.conf` includes these important settings:

- Password authentication enabled
- Binding to all interfaces (`0.0.0.0` and `::`)
- Protected mode enabled
- Data persistence configured
- Default port: 6379

### Connection String Format

When connecting from a Laravel application on Fly.io, use the following connection string format:

```
redis://default:your_password@your-redis-app.internal:6379
```

## Laravel Configuration

In your Laravel application, make these configuration adjustments:

1. Set the Redis connection in your `.env`:

```
REDIS_HOST=your-redis-app.internal
REDIS_PASSWORD=your_password
REDIS_PORT=6379
```

2. Important: In your cache and database configurations, set the default database to 1:

```php
// config/database.php
'redis' => [
    'client' => env('REDIS_CLIENT', 'predis'),
    'default' => [
        'host' => env('REDIS_HOST', '127.0.0.1'),
        'password' => env('REDIS_PASSWORD', null),
        'port' => env('REDIS_PORT', 6379),
        'database' => 1, // This is important
    ],
],
```

## Troubleshooting

Common issues and solutions:

1. Connection Refused: Make sure you're using the full connection string format with authentication.
2. Authentication Failed: Verify the password in your connection string matches the one in `redis.conf`.
3. Internal Address: The `.internal` address only works between Fly.io applications in the same organization.

## Important Notes

- The Redis instance is only accessible within your Fly.io organization's private network
- Always use strong passwords in production
- The configuration includes basic persistence settings
- Protected mode is enabled for security

## Contributing

Feel free to submit issues and enhancement requests!

## Related Documentation

- [Fly.io Laravel Redis Documentation](https://fly.io/docs/laravel/database-guides/laravel-redis/)
- [Redis Configuration Documentation](https://redis.io/topics/config)
- [Laravel Redis Documentation](https://laravel.com/docs/redis)

## License

This project is open-sourced software licensed under the MIT license.
