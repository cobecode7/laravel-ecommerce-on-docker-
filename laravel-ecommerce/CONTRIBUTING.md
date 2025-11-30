# Contributing to Laravel E-commerce

Thank you for your interest in contributing to the Laravel E-commerce project! We appreciate your help in making this project better.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Environment](#development-environment)
- [How Can I Contribute?](#how-can-i-contribute)
- [Style Guides](#style-guides)
- [Pull Request Process](#pull-request-process)
- [Development Guidelines](#development-guidelines)

## 📖 Code of Conduct

Please read and follow our [Code of Conduct](CODE_OF_CONDUCT.md) to ensure a welcoming environment for everyone.

## 🚀 Getting Started

### Prerequisites

- Docker (v20.10 or higher)
- Docker Compose (v2.0 or higher)
- Git

### Setting Up Your Development Environment

1. **Fork the repository** on GitHub
2. **Clone your fork**:
   ```bash
   git clone https://github.com/YOUR_USERNAME/laravel-ecommerce.git
   cd laravel-ecommerce
   ```
3. **Set up the development environment**:
   ```bash
   # Copy the environment file
   cp .env.example .env
   
   # Build and start the Docker services
   docker-compose up -d --build
   
   # Install PHP dependencies
   docker-compose exec app composer install
   
   # Generate application key
   docker-compose exec app php artisan key:generate
   
   # Run database migrations
   docker-compose exec app php artisan migrate --seed
   ```

## 🛠️ Development Environment

### Running the Application Locally

The application will be accessible at `http://localhost:8081` and PHPMyAdmin at `http://localhost:8080`.

### Useful Development Commands

```bash
# Access the application container
docker-compose exec app bash

# Run Laravel commands
docker-compose exec app php artisan <command>

# Run tests
docker-compose exec app php artisan test

# View application logs
docker-compose logs -f app

# Install new PHP packages
docker-compose exec app composer require <package-name>

# Install new NPM packages (if applicable)
docker-compose exec app npm install <package-name>
```

### Code Formatting

This project uses Laravel Pint for code formatting. Before submitting a pull request, make sure to format your code:

```bash
docker-compose exec app ./vendor/bin/pint
```

## 🤝 How Can I Contribute?

### Reporting Bugs

- Use the issue tracker to report bugs
- Fill out the issue template with as much detail as possible
- Include steps to reproduce the issue
- Include the version of the project you're using

### Suggesting Features

- Use the issue tracker to suggest new features
- Explain the feature clearly with examples
- Describe the intended use case
- Discuss potential implementations if possible

### Submitting Pull Requests

1. Create a new branch: `git checkout -b feature/amazing-feature`
2. Make your changes
3. Add tests for your changes (if applicable)
4. Run the tests: `docker-compose exec app php artisan test`
5. Format your code: `docker-compose exec app ./vendor/bin/pint`
6. Commit your changes: `git commit -m 'Add amazing feature'`
7. Push to the branch: `git push origin feature/amazing-feature`
8. Open a pull request

## 📝 Style Guides

### Git Commit Messages

- Use the present tense ("Add feature" not "Added feature")
- Use the imperative mood ("Move cursor to..." not "Moves cursor to...")
- Limit the first line to 72 characters or less
- Reference issues and pull requests after the first line

### Code Style

- Follow PSR-12 coding standards
- Use Laravel's coding conventions
- Use descriptive variable and function names
- Write meaningful comments when necessary
- Follow the existing code style in the project

### PHP Style Guide

- Use 4 spaces for indentation (not tabs)
- Use type declarations where possible
- Use strict comparison operators (===, !==) instead of loose comparison (==, !=) when appropriate
- Use early returns to avoid deep nesting
- Use PHPDoc comments for functions and classes

### Laravel-Specific Guidelines

- Use Laravel's built-in methods and helpers when possible
- Follow Laravel's directory structure and naming conventions
- Use Eloquent models for database operations
- Use Laravel's validation features
- Use Laravel's authentication and authorization features

## 🔄 Pull Request Process

1. Ensure your code follows the style guides above
2. Update or add tests as appropriate
3. Update documentation as needed
4. Ensure all tests pass
5. Describe your changes in the pull request description
6. Link to any related issues
7. Wait for review and address any feedback

### Pull Request Requirements

- Follow the code style of the project
- Include tests for new functionality
- Update documentation if needed
- Pass all automated tests
- Have no conflicts with the base branch

## 🧪 Testing

### Running Tests

```bash
# Run all tests
docker-compose exec app php artisan test

# Run tests with coverage
docker-compose exec app php artisan test --coverage

# Run specific test file
docker-compose exec app php artisan test tests/Feature/ExampleTest.php
```

### Writing Tests

- Place tests in the appropriate directory (`tests/Feature` or `tests/Unit`)
- Use descriptive names for test methods
- Use Laravel's testing tools and helpers
- Test both positive and negative cases
- Follow the AAA pattern (Arrange, Act, Assert)

## 🔐 Security

If you discover a security vulnerability, please follow these steps:

1. **Do not open a public issue** for security vulnerabilities
2. Send an email to the maintainers with details about the vulnerability
3. Include steps to reproduce the vulnerability
4. We will address the issue as soon as possible

## 📚 Development Guidelines

### Database Migrations

- Create migrations for any database changes
- Use descriptive names for migrations
- Test migrations before submitting
- Follow Laravel's migration conventions

### Configuration

- Use environment variables for configuration
- Store sensitive data in environment files
- Use Laravel's configuration caching in production

### Error Handling

- Handle exceptions appropriately
- Log errors for debugging
- Return appropriate HTTP status codes
- Provide meaningful error messages

## 🙏 Thank You!

Thank you for contributing to the Laravel E-commerce project! Your time and effort help make this project better for everyone.