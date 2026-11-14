# FeedBox volumes and networking starter

FeedBox is a minimal feedback collector with PostgreSQL (`db`), Express (`api`), and an Nginx-served frontend (`web`).

## Project structure

```text
├── server.js
├── Dockerfile
├── package.json
├── package-lock.json
├── docker-compose.yml
├── docker-compose.dev.yml
├── .env.example
└── client/
    ├── Dockerfile
    ├── nginx.conf
    └── index.html
```

## Run the starter

```bash
cp .env.example .env
docker compose up --build
```

- API: http://localhost:5000/api/messages
- Frontend: http://localhost:3000

For API hot reload during development, start with the development override:

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml up --build
```

PostgreSQL data is stored in the `postgres_data` named volume. The frontend and
API communicate over the private `frontend` network, with Nginx proxying API
requests. The API and database use the private `backend` network, so PostgreSQL
is reachable only by the API. Only the API and frontend ports are published to
the host. The development Compose file bind-mounts the source and preserves
container-installed dependencies in an anonymous volume.

## Before changing anything

1. Open `docker-compose.yml` and trace how each service currently starts.
2. Identify where PostgreSQL writes data inside its container.
3. Check which services genuinely need to communicate with each other.
4. Notice which development behaviour should not belong in the main Compose file.

Use the assignment brief's expected behaviours as your completion checklist. Choose the required Compose configuration yourself.

Validate the final configuration with:

```bash
docker compose config -q
docker compose up --build
```

Do not commit `.env` or `node_modules`.
