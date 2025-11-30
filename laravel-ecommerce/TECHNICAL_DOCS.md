# Laravel E-commerce: Technical Documentation

## Project Overview

This document provides detailed technical information about the Laravel e-commerce application, including architecture, components, and development guidelines.

## 🏗️ Architecture

### Tech Stack
- **Backend Framework**: Laravel 12.x
- **Programming Language**: PHP 8.4.11
- **Web Server**: Nginx
- **Database**: MariaDB 11.8
- **Cache/Session/Queue**: Redis
- **Containerization**: Docker & Docker Compose
- **Package Manager**: Composer (PHP)

### Project Structure
```
laravel-ecommerce/
├── app/                    # Laravel application code
│   ├── Console/           # Artisan commands
│   ├── Exceptions/        # Application exceptions
│   ├── Http/              # Controllers, Middleware, Requests
│   ├── Models/            # Eloquent models
│   └── ...
├── bootstrap/             # Framework bootstrap files
├── config/                # Laravel configuration files
├── database/              # Migrations, seeds, factories
├── docker/                # Docker configurations
│   ├── app/               # PHP-FPM Dockerfile
│   └── nginx/             # Nginx configuration
├── public/                # Public web assets
├── resources/             # Views, assets, language files
├── routes/                # Application routes
├── tests/                 # Test files
├── .env                   # Environment variables
├── .env.example           # Environment variables template
├── composer.json          # PHP dependencies
├── docker-compose.yml     # Docker services definition
└── README.md              # Project documentation
```

## 🐳 Docker Environment

### Services Architecture
```
┌─────────────────┐    ┌─────────────────┐
│     Client      │    │  PHPMyAdmin     │
│   (Browser)     │    │    (Port 8080)  │
└─────────┬───────┘    └─────────────────┘
          │                       │
          │               ┌───────▼────────┐
          │               │   Nginx        │
          │               │   (Port 8081)  │
          │               └───────┬────────┘
          │                       │
          └───────────────────────┼─────────────────────┐
                                  │                     │
                    ┌─────────────▼────────┐     ┌─────▼──────────┐
                    │   PHP-FPM Container  │     │  Redis         │
                    │   (app service)      │     │  (Port 6380)   │
                    │   (Port 9000)        │     └────────────────┘
                    └─────────────┬────────┘
                                  │
                    ┌─────────────▼────────┐
                    │    MariaDB           │
                    │    (Port 3307)       │
                    └──────────────────────┘
```

### Docker Configuration Details

#### App Service (PHP-FPM)
- **Base Image**: `php:8.4.11-fpm`
- **Extensions**: `pdo_mysql`, `mbstring`, `exif`, `pcntl`, `bcmath`, `gd`
- **User**: Non-root user with UID 1337
- **Working Directory**: `/var/www`
- **Volume**: Project directory mapped to container
- **Composer**: Installed globally with Laravel installer

#### Web Service (Nginx)
- **Base Image**: `nginx:alpine`
- **Configuration**: Custom Nginx config for Laravel routing
- **Static Files**: Optimized caching for images, CSS, JS
- **Port**: 8081 → 80 (container)

#### Database Service (MariaDB)
- **Base Image**: `mariadb:11.8`
- **Database**: `ecommerce_db`
- **User**: `ecommerce_user`
- **Port**: 3307 → 3306 (container)
- **Persistent Storage**: `db_data` volume

#### Redis Service
- **Base Image**: `redis:alpine`
- **Port**: 6380 → 6379 (container)
- **Persistent Storage**: `redis_data` volume
- **Uses**: Cache, sessions, queue

## 🔩 Configuration Files

### .env Configuration
Key environment variables:
```env
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:LHGLy/jFzJZjGZrO6g5T9Bk/K9kKt7zZp8K9kKt7zZp8=
APP_DEBUG=true
APP_URL=http://localhost:8081

DB_CONNECTION=mysql
DB_HOST=db          # Reference to db service in docker-compose
DB_PORT=3306        # Internal port in container
DB_DATABASE=ecommerce_db
DB_USERNAME=ecommerce_user
DB_PASSWORD=password

CACHE_DRIVER=redis
QUEUE_CONNECTION=redis
SESSION_DRIVER=redis
SESSION_LIFETIME=120

REDIS_HOST=redis    # Reference to redis service in docker-compose
REDIS_PORT=6379     # Internal port in container
```

### Nginx Configuration
The nginx configuration includes:
- Laravel-specific routing with `try_files`
- PHP-FPM handling via FastCGI
- Static file caching with 1-year expiration
- Proper SCRIPT_FILENAME and PATH_INFO parameters
- Error page handling

## 🚀 Deployment Considerations

### Production Configuration
For production deployment, consider these modifications:

1. **Environment Variables**:
   - Set `APP_ENV=production`
   - Set `APP_DEBUG=false`
   - Use strong, unique values for all passwords and keys

2. **Security**:
   - Implement SSL/TLS certificates
   - Configure proper firewall rules
   - Use non-default ports
   - Implement rate limiting

3. **Performance**:
   - Use dedicated volumes for database and Redis
   - Configure multiple PHP-FPM workers
   - Implement application-level caching
   - Optimize database queries

4. **Monitoring**:
   - Add health checks to services
   - Implement log aggregation
   - Set up monitoring for containers

### Scaling Considerations
- Load balancing for web service
- Database read replicas
- Horizontal scaling of app service
- Redis clustering for high availability

## 🧪 Testing Strategy

### Test Structure
Laravel tests are organized in the `tests/` directory with:
- Feature tests (`tests/Feature/`)
- Unit tests (`tests/Unit/`)
- Test cases extending Laravel's testing classes

### Running Tests
Tests can be run inside the app container:
```bash
docker-compose exec app php artisan test
```

## 🔄 Development Workflow

### Local Development
1. Clone the repository
2. `docker-compose up -d --build`
3. `docker-compose exec app composer install`
4. `docker-compose exec app php artisan key:generate`
5. `docker-compose exec app php artisan migrate --seed`

### Code Standards
- Follow PSR-12 coding standards
- Use Laravel's built-in tools and conventions
- Implement proper error handling
- Follow security best practices

## 🔒 Security Measures

### Built-in Security Features
- Laravel's CSRF protection
- SQL injection prevention via Eloquent/QueryBuilder
- Session management via Redis
- Environment-based configuration

### Recommended Security Practices
- Regular dependency updates
- Input validation and sanitization
- Proper error handling without information disclosure
- Secure communication (HTTPS in production)

## 📦 Dependency Management

### PHP Dependencies
Managed through Composer with:
- Laravel framework and core components
- Development tools (PHPUnit, Faker, etc.)
- Laravel-specific packages

### Frontend Dependencies
Currently using Laravel Mix for asset compilation, though the project appears to have empty js/css directories.

## 🔄 Update and Maintenance

### Updating Laravel
```bash
docker-compose exec app composer update laravel/framework
```

### Updating Dependencies
```bash
docker-compose exec app composer update
```

### Database Migrations
```bash
docker-compose exec app php artisan migrate
```

### Seeding Data
```bash
docker-compose exec app php artisan db:seed
```

## 🤝 Contributing Guidelines

### Development Environment
1. Fork and clone the repository
2. Set up Docker environment as described
3. Create feature branch
4. Make changes following Laravel conventions
5. Test functionality
6. Submit pull request

### Code Review Process
- Ensure all tests pass
- Follow Laravel coding standards
- Include appropriate documentation
- Update tests if needed

## 📞 Support and Resources

### Getting Help
- File an issue on the GitHub repository
- Check the Laravel documentation
- Review Docker documentation for container-specific issues

### Useful Commands
- `docker-compose ps` - Check running services
- `docker-compose logs -f app` - View app logs
- `docker-compose exec app bash` - Access app container
- `docker-compose down -v` - Stop and remove containers with volumes

This technical documentation provides comprehensive information about the Laravel E-commerce application's architecture, configuration, and development practices.