# ![RealWorld Example App](logo.png)

> ### Django Ninja + Postgres backend codebase containing real world examples (CRUD, auth, advanced patterns, etc).

This repository provides a production-style API backend built with [Django Ninja](https://django-ninja.dev).

## What This Codebase Does

It implements a Medium-like API backend with these capabilities:
- Authentication and accounts (register, login, JWT auth, current-user update)
- Profiles and follow system
- Articles CRUD with tags and favorites
- Personalized feed and article filtering
- Comments create/list/delete
- Consistent API error payloads
- Auto-generated docs at `/docs`

## How To Set Up The Repo

### Prerequisites

- Python 3.12+
- `make`
- Optional: PostgreSQL for non-SQLite local runs
- Optional: Docker and Docker Compose

### 1. Clone the repository

```shell
git clone <your-fork-or-mirror-url>
cd realworld-django-ninja
```

### 2. Install project dependencies

```shell
make sync
```

If you want PostgreSQL support on Debian/Ubuntu:

```shell
sudo apt install -y postgresql-server-dev-all
```

### 3. Configure database (optional)

By default, local development works with SQLite.

To use PostgreSQL, set `DATABASE_URL`:

```shell
DATABASE_URL=postgresql://user:password@host:port/dbname
```

### 4. Apply migrations

SQLite:

```shell
DEBUG=True make migrate
```

PostgreSQL:

```shell
DATABASE_URL=postgresql://user:password@host:port/dbname make migrate
```

## How To Use This Project

### Run the API server

Debug mode (SQLite):

```shell
make run-debug
```

Non-debug mode (example with PostgreSQL):

```shell
make run DATABASE_URL=postgresql://user:password@host:port/dbname ALLOWED_HOSTS=*
```

Then open the API docs at [http://localhost:8000/docs](http://localhost:8000/docs).

### Run checks and tests

Recommended full verification (lint + type-check + fast tests):

```shell
make verify
```

Other useful targets:
- `make test-django`
- `make test-django-fast`
- `make test-postman`
- `make test-postman-with-managed-server`

### Run with Docker

```shell
docker compose up
```

Rebuild image when needed:

```shell
docker compose up --build
```

### Deploying

This project can be deployed like any standard Django application.

The official Django deployment guide is the recommended reference:
[https://docs.djangoproject.com/en/5.0/howto/deployment/](https://docs.djangoproject.com/en/5.0/howto/deployment/)

### Connect a frontend

Choose any frontend that follows the same API contract, or use one of the included git submodules in `fronts`:
- [khaledosman/react-redux-realworld-example-app](https://github.com/khaledosman/react-redux-realworld-example-app)
- [mutoe/vue3-realworld-example-app](https://github.com/mutoe/vue3-realworld-example-app)

Fetch submodules:

```shell
make submodule
```

Frontend helper commands:

| Description | [react-redux-realworld-example-app](https://github.com/khaledosman/react-redux-realworld-example-app) | [vue3-realworld-example-app](https://github.com/mutoe/vue3-realworld-example-app) |
| -------------------- | ------------------------ | ---------------------- |
| Install dependencies | `make front-setup-react` | `make front-setup-vue` |
| Run frontend         | `make front-run-react`   | `make front-run-vue`   |
| Clean dependencies   | `make front-clean-react` | `make front-clean-vue` |

By default, frontends call `http://localhost:8000`. Override with `API_URL` as needed.

#### About [Django Ninja](https://django-ninja.dev/)
[Django Ninja](https://django-ninja.dev/) is an overlay to [Django](https://www.djangoproject.com) which lets you create APIs while being heavily inspired by [FastAPI](https://fastapi.tiangolo.com). It means it tries to stay as simple as possible for the API creation, while letting you benefit from the whole [Django](https://www.djangoproject.com) ecosystem, including [its ORM](https://docs.djangoproject.com/en/5.0/topics/db/), [its auth system](https://docs.djangoproject.com/en/5.0/topics/auth/), even [its HTML templates](https://docs.djangoproject.com/en/5.0/topics/templates/) if you still need those...

[Django Ninja](https://django-ninja.dev/) is a very good alternative to [Django REST framework](https://www.django-rest-framework.org), as it tries to be less unnecessarily complex and more performant.

As [Django Ninja](https://django-ninja.dev/) (and by extension this repository) only covers the API part, you may then [connect a frontend to it](#connect-a-frontend) after [deploying](#deploying).

#### Code style
In the long term, this repository aims to target as many good practices expected in a Django Ninja project as possible.

#### Quick-and-dirty to production-ready
One of Django Ninja's strengths is enabling rapid prototyping with untyped dictionaries, then iterating toward proper schemas. A previous iteration of this project contained a quick-and-dirty `comments` app implementation using raw dicts, broad exception handling, and minimal typing - the kind of code you might write for a fast PoC. The current version shows the refactored, production-ready approach with proper `ModelSchema` definitions and typed responses.

