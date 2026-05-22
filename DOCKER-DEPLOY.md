# Pharmacy Management System — Docker Deployment Guide

This project ships with a Docker Compose setup that runs the Angular
frontend, the Node/Express backend, and a MongoDB database in three
containers wired together on a private Docker network.

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (or
  Docker Engine + Docker Compose v2) installed and running
- Ports `4200`, `3001`, and `27017` free on your host

---

## 1. Clone the repository

Pick any folder on your machine and clone the repo:

```bash
git clone https://github.com/LalanaChami/Pharmacy-Mangment-System.git
cd Pharmacy-Mangment-System
```

## 2. Where the Docker Compose files live

All Docker files live at the repository root (same directory as
`package.json` and `angular.json`):

```
Pharmacy-Mangment-System/
├── docker-compose.yml          ← orchestration
├── .env.example                ← copy to .env to customise
├── docker/
│   ├── Dockerfile.backend      ← Node/Express image
│   └── Dockerfile.frontend     ← Angular dev-server image
└── ...
```

You do not need to move anything — Compose finds `docker-compose.yml`
automatically when you run it from the project root.

## 3. Environment configuration

Copy the sample env file:

```bash
cp .env.example .env
```

Open `.env` and adjust if you need to. The only setting is:

| Variable    | Purpose                                                                                                        | Default                              |
| ----------- | -------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `MONGO_URI` | MongoDB connection string used by the backend. Leave default to use the local Mongo container, or paste your own Atlas URI. | `mongodb://mongo:27017/pharmacy`     |

> The repo's original `backend/app.js` hard-coded a MongoDB Atlas URI
> with leaked credentials. That URI is no longer functional. The file
> has been patched in this clone to read `MONGO_URI` from the
> environment, falling back to the local Mongo container.
>
> Additional changes made to this clone to get a clean `docker compose up`:
> - Removed `brain.js` from `package.json` (unused; pulled in a native
>   X11/OpenGL dep that won't build on arm64).
> - Added `bcryptjs`, `debug`, and `lodash.isequal` to `package.json`
>   (used by the backend / a frontend lib but missing from the original
>   manifest).
> - Rewrote `http://localhost:3000` → `http://localhost:3001` across
>   `src/` so the frontend hits the remapped backend port.

## 4. Build and start the containers

From the project root:

```bash
docker compose up --build
```

This will:

1. Pull the `mongo:5.0` image
2. Build the **backend** image (installs Node deps, ~3–5 min the first time)
3. Build the **frontend** image (same — Angular has a lot of deps)
4. Start all three services in the foreground so you can watch the logs

To run detached (in the background) instead:

```bash
docker compose up --build -d
```

The first build can take a while because the project installs a large
Angular 8 toolchain. Subsequent `docker compose up` calls reuse the
cached layers and start in seconds.

Tail logs when running detached:

```bash
docker compose logs -f             # all services
docker compose logs -f frontend    # one service
```

## 5. Accessing the application

Once Compose reports that the Angular dev server is ready
(`Compiled successfully` in the frontend logs), open:

| URL                                            | What it is                       |
| ---------------------------------------------- | -------------------------------- |
| http://localhost:4200                          | Pharmacy app (Angular frontend)  |
| http://localhost:3001/api/...                  | Backend REST API                 |
| mongodb://localhost:27017                      | MongoDB (e.g. for Compass/mongosh) |

> The backend container listens on port 3000 internally but is published
> on host port **3001** to avoid clashing with a common conflict
> (Grafana also defaults to 3000). The Angular frontend's hardcoded
> `http://localhost:3000` API URLs have been rewritten to `:3001` in
> this clone. If you change the published port in `docker-compose.yml`,
> run a find/replace across `src/` for the new value.

The frontend calls the backend at `http://localhost:3000` from your
browser, which Docker forwards into the backend container — no extra
configuration needed.

## 6. Stopping, restarting, and cleaning up

| Action                                              | Command                                |
| --------------------------------------------------- | -------------------------------------- |
| Stop containers (preserves data + images)           | `docker compose stop`                  |
| Start them again                                    | `docker compose start`                 |
| Restart a single service                            | `docker compose restart backend`       |
| Stop **and remove** containers (keeps DB volume)    | `docker compose down`                  |
| Same, plus wipe the MongoDB volume (full reset)     | `docker compose down -v`               |
| Rebuild after changing a Dockerfile or `package.json` | `docker compose up --build`          |
| Open a shell inside a running container             | `docker compose exec backend sh`       |

To uninstall completely, run `docker compose down -v` and then
`docker image rm` the two locally-built images
(`pharmacy-mangment-system-backend`, `pharmacy-mangment-system-frontend`).

---

## Troubleshooting

- **`Cannot find module` errors on first start.** The build step failed
  to install Node deps. Re-run `docker compose build --no-cache`.
- **Frontend stuck at "Compiled successfully" but page won't load.**
  Make sure port 4200 isn't already in use on your host
  (`lsof -iTCP:4200`).
- **Backend logs say `connection failed!`** Mongo container hasn't
  finished starting yet. The backend will keep running; refresh in a
  few seconds, or `docker compose restart backend`.
- **`bcrypt` build error during `npm install`.** The Dockerfiles use
  `node:12.18` (Debian) specifically to avoid Alpine's missing native
  build tools. If you switched the base image and hit this, switch
  back or add `python3 make g++` to the image.
