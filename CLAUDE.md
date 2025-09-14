# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

FOSSBilling is a free open-source billing and client management system built with PHP 8.2+ and using modern web technologies. It follows a modular architecture with themes, modules, and a dependency injection container.

## Development Commands

### PHP Commands
- **Install dependencies**: `composer install`
- **Run tests**: `composer test` or `vendor/bin/phpunit`
- **Static analysis**: `vendor/bin/phpstan analyse`
- **Code style check**: `vendor/bin/php-cs-fixer fix --dry-run`
- **Code style fix**: `vendor/bin/php-cs-fixer fix`
- **Refactoring**: `vendor/bin/rector process --dry-run` (preview) or `vendor/bin/rector process` (apply)

### Node.js/Frontend Commands
- **Install frontend dependencies**: `npm install`
- **Build all assets**: `npm run build`
- **Build themes only**: `npm run build-themes`
- **Build modules only**: `npm run build-modules`
- **Build specific theme**: `npm run build-admin_default` or `npm run build-huraga`
- **Build WYSIWYG module**: `npm run build-wysiwyg`

### Testing
- **Run Cypress E2E tests**: `npm run cypress:open`
- **Run single PHPUnit test**: `vendor/bin/phpunit tests/path/to/TestClass.php`
- **Run tests with specific PHP version**: The CI supports PHP 8.2, 8.3, and 8.4

## Architecture

### Core Structure
- **`src/`**: Main application code
  - **`config-sample.php`**: Configuration template
  - **`di.php`**: Dependency injection container setup
  - **`load.php`**: Application bootstrap and initialization
  - **`index.php`**: Main entry point
  - **`console.php`**: CLI commands entry point
  - **`cron.php`**: Scheduled tasks entry point

### Key Directories
- **`src/library/`**: Core framework classes
  - **`FOSSBilling/`**: Main application classes (Config, Environment, etc.)
  - **`Box/`**: Legacy BoxBilling compatibility layer
  - **`Model/`**: Data models
  - **`Payment/`**: Payment gateway integrations
  - **`Registrar/`**: Domain registrar integrations
  - **`Server/`**: Server management integrations

- **`src/modules/`**: Feature modules (Activity, Client, Email, Invoice, etc.)
  - Each module contains: Api/, Service/, Controller/, and html_* templates
  - Modules follow a consistent structure with dependency injection

- **`src/themes/`**: User interface themes
  - **`admin_default/`**: Default admin interface theme (uses npm workspaces)
  - **`huraga/`**: Default client interface theme (uses npm workspaces)

### Technology Stack
- **Backend**: PHP 8.2+ with Pimple DI container
- **Database**: RedBean PHP ORM
- **Templates**: Twig templating engine
- **Frontend Build**: Webpack Encore for asset compilation
- **CSS Framework**: Bootstrap 5.3
- **JavaScript Libraries**: Tom Select, Autosize
- **Testing**: PHPUnit 11.4+, PHPStan 2.0+, Cypress
- **Code Quality**: PHP-CS-Fixer, Rector

### Configuration
- Copy `src/config-sample.php` to `src/config.php` for local development
- Environment detection via `FOSSBilling\Environment` class
- Configuration managed through `FOSSBilling\Config` class
- Debug mode and monitoring settings available

### Module Architecture
Each module follows this pattern:
- **Api/**: Public API endpoints and admin API controllers
- **Service/**: Business logic and core functionality
- **Controller/**: Web controllers for handling HTTP requests
- **html_admin/**: Admin interface templates
- **html_client/**: Client interface templates

### Database
- Uses RedBean PHP ORM for database abstraction
- Database migrations and setup handled during installation
- Models located in `src/library/Model/`

### Development Environment
- Supports DDEV for local development (`.ddev/` configuration present)
- Docker support available via `Dockerfile`
- Requires PHP 8.2+ with extensions: curl, intl, mbstring, pdo, zlib

### Build Process
The application uses npm workspaces for theme building:
- Themes are built independently using Webpack Encore
- Assets are compiled and optimized for production
- Source maps and development builds available

### Testing Strategy
- Unit tests with PHPUnit in `tests/` directory
- Legacy tests in `tests-legacy/` directory
- E2E tests with Cypress
- Static analysis with PHPStan (baseline: `phpstan-baseline.neon`)
- Code style enforcement with PHP-CS-Fixer

## Important Notes

- **Vendor directory**: Located at `src/vendor/` (not root level)
- **Composer platform**: Configured for PHP 8.2
- **Node version**: Requires npm 10+
- **Security**: Remove `install/` directory after installation in production
- **Environment**: Uses APP_ENV environment variable for environment detection
- **Logging**: Monolog for structured logging
- **Error handling**: Whoops for development error pages