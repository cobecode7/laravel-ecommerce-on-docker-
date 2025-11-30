# Laravel E-commerce Application

A Dockerized Laravel application for e-commerce functionality. This project follows modern development practices with containerized services for easy deployment and scaling.

## 🚀 Features

- **Laravel Framework**: Built on the latest Laravel (v12) with PHP 8.4.11
- **Dockerized Environment**: Complete containerized setup with services
- **MariaDB Database**: Using MariaDB 11.8 for data persistence
- **Redis Integration**: For caching, sessions, and queue management
- **Nginx Web Server**: Production-ready web server configuration
- **PHP-FPM**: Optimized PHP processing
- **PHPMyAdmin**: Database management interface
- **Queue Management**: Background job processing
- **Session Management**: Redis-backed sessions for scalability

## 📋 Prerequisites

- Docker (v20.10 or higher)
- Docker Compose (v2.0 or higher)
- Git

## 🛠️ Installation

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd laravel-ecommerce
   ```

2. **Set up environment variables:**
   ```bash
   cp .env.example .env
   ```

3. **Build and start the Docker services:**
   ```bash
   docker-compose up -d --build
   ```

4. **Install Laravel dependencies:**
   ```bash
   docker-compose exec app composer install
   ```

5. **Generate application key:**
   ```bash
   docker-compose exec app php artisan key:generate
   ```

6. **Run database migrations:**
   ```bash
   docker-compose exec app php artisan migrate --seed
   ```

## 🏗️ Services

The application consists of the following services:

- **app**: PHP-FPM container running Laravel application
- **web**: Nginx container serving the application on port 8081
- **db**: MariaDB database on port 3307
- **redis**: Redis server for caching and queues on port 6380
- **phpmyadmin**: Database management interface on port 8080

## 🌐 Access Points

- **Application**: http://localhost:8081
- **PHPMyAdmin**: http://localhost:8080
- **Database**: localhost:3307 (external) / `db:3306` (internal)

## 🔧 Development

For development, you can access the containers directly:

```bash
# Access the application container
docker-compose exec app bash

# Run Laravel commands
docker-compose exec app php artisan <command>

# View application logs
docker-compose logs -f app

# Run tests
docker-compose exec app php artisan test
```

## 🛡️ Security

- Application key is automatically generated
- Password hashing with Laravel's default hasher
- CSRF protection enabled by default
- Session data stored in Redis with 120-minute lifetime
- Environment variables for sensitive data

## 📦 Environment Variables

Key environment variables:

- `DB_HOST`: Database host (defaults to `db`)
- `DB_PORT`: Database port (defaults to `3306`)
- `DB_DATABASE`: Database name (defaults to `ecommerce_db`)
- `DB_USERNAME`: Database username (defaults to `ecommerce_user`)
- `REDIS_HOST`: Redis host (defaults to `redis`)
- `APP_ENV`: Application environment (defaults to `local`)

## 🔁 Deployment

For production deployment, consider:

1. Changing environment variables for production
2. Using HTTPS with proper certificates
3. Configuring persistent volumes for data
4. Setting up a reverse proxy with SSL termination
5. Implementing backup strategies for database and Redis

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a pull request

## 🐳 Docker Compose Services

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: docker/app/Dockerfile
    image: laravel-ecommerce-app
    container_name: laravel-ecommerce-app
    restart: unless-stopped
    working_dir: /var/www
    volumes:
      - ./:/var/www
    networks:
      - ecommerce-network

  web:
    image: nginx:alpine
    container_name: laravel-ecommerce-web
    restart: unless-stopped
    ports:
      - "8081:80"
    volumes:
      - ./:/var/www
      - ./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf
    networks:
      - ecommerce-network
    depends_on:
      - app

  db:
    image: mariadb:11.8
    container_name: laravel-ecommerce-db
    restart: unless-stopped
    ports:
      - "3307:3306"
    environment:
      MYSQL_DATABASE: ecommerce_db
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_USER: ecommerce_user
      MYSQL_PASSWORD: password
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - ecommerce-network

  redis:
    image: redis:alpine
    container_name: laravel-ecommerce-redis
    restart: unless-stopped
    ports:
      - "6380:6379"
    volumes:
      - redis_data:/data
    networks:
      - ecommerce-network

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: laravel-ecommerce-phpmyadmin
    restart: unless-stopped
    ports:
      - "8080:80"
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: root_password
    networks:
      - ecommerce-network
    depends_on:
      - db

networks:
  ecommerce-network:
    driver: bridge

volumes:
  db_data:
  redis_data:
```

## 🔧 Customization

- Modify `docker/nginx/default.conf` to customize Nginx configuration
- Update `docker/app/Dockerfile` to add additional PHP extensions or libraries
- Adjust environment variables in `.env` for your specific requirements

## 📞 Support

If you encounter any issues or have questions, please file an issue on the GitHub repository.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
