# Laravel10-Livewire

A collection of small Livewire 2 components on Laravel 10, each on its own page, built to practise reactive server-side UI.

## Context

A learning project built by Oleksandr Karabudka in June 2023 (about three days of work, 24 to 26 June). The goal was to try out typical Livewire use cases one by one: state, events, validation, pagination, file uploads and dependent inputs, without writing custom JavaScript.

## Features

Each component is a full-page Livewire route:

- **Counter** (`/`): increment and decrement.
- **Calculator** (`/calculator`): basic arithmetic with recalculation on input change.
- **Todo list** (`/todo-list`): add, toggle, delete and select todo items stored in the database.
- **Cascading dropdown** (`/cascading-dropdown`): choosing a continent loads its countries.
- **Product search** (`/products`): live search by title with pagination (20 per page).
- **Image upload** (`/image-upload`): validated upload stored on the `public` disk.
- **Register form** (`/register`): real-time per-field validation with `validateOnly`.

## Tech stack

- PHP 8.1+, Laravel 10
- Livewire 2
- Tailwind CSS, Vite
- MariaDB 10.5, Redis, MailHog
- Docker Compose (nginx, PHP-FPM 8.2)

## Architecture

- Components live in `app/Http/Livewire`, with views in `resources/views/livewire`, and share one layout.
- Livewire traits are used where they fit: `WithPagination` for product search, `WithFileUploads` for image upload.
- Models: `TodoItem`, `Continent` and `Country` (one-to-many), `Product`.
- The `DatabaseSeeder` fills five continents with ten countries each and creates 100 products with factories.

## Getting started

Requires Docker and Docker Compose.

```bash
sh docker/conf/ssl/generate.sh        # create the local self-signed TLS certificate
cp .env.example .env                  # then adjust the DB_* values
docker compose up -d web db           # web starts php and mailhog as dependencies
docker compose exec php composer install
docker compose exec php php artisan key:generate --ansi
docker compose exec php php artisan migrate --seed
docker compose exec php php artisan storage:link   # needed to display uploaded images
npm install && npm run build
```

When using the `db` container, set `DB_HOST=db` in `.env`. Open http://localhost.

## Testing

Only the default Laravel example tests are included; the components have no tests of their own.

```bash
docker compose exec php php artisan test
```

## Status

Finished learning project (2023), not maintained.

## License

MIT, see [LICENSE](LICENSE).
