# Lab 6 - Task API

An API to create, read, update and delete tasks and it also has login and uses Redis to cache data.

## Setup

Install the packages:

```
npm install
```

Create a `.env` file. You can copy `.env.example`:

```
PORT=3000
SESSION_SECRET=your-session-secret-here
REDIS_URL=redis://localhost:6379
```

## Start Redis

Open Docker Desktop, then run:

```
docker run -d --name week7-redis -p 6379:6379 redis:7-alpine
```

Check that it is running:

```
docker ps
```
## Redis running (docker ps)

```
CONTAINER ID   IMAGE            COMMAND                  STATUS         PORTS                    NAMES
430dceff1272   redis:7-alpine   "docker-entrypoint.s…"   Up 19 seconds  0.0.0.0:6379->6379/tcp   week7-redis
```

## Run the app

```
npm run dev
```

The app runs on http://localhost:3000

## Tests

| Test | Expected | Result |
|---|---|---|
| GET /api/tasks/health | 200 and request is logged | Passed |
| POST invalid task | 400 with errors | Passed |
| POST valid task | 201 with an id | Passed |
| GET missing task | 404 Task not found | Passed |
| Helmet header | Header is present | Passed (X-Content-Type-Options: nosniff) |
| GET /api/auth/me before login | 401 | Passed |
| Register, login, GET /me | 201, 200, 200 | Passed |
| GET /api/tasks two times | database, then cache | Passed |
| Create task, then GET /api/tasks | database with new task | Passed |

## npm audit

npm audit found 5 problems (2 moderate, 3 high). They come from nodemon and sequelize. I did not use `npm audit fix --force` because it would install old versions and break the app.

## Why these tools

- Sequelize: works with the database without writing SQL and helps stop SQL injection.
- express-validator: checks the data before saving it.
- Helmet: adds security headers.
- Passport and bcrypt: handle login and keep passwords hashed.
- compression: makes responses smaller.
- Redis: saves the task list in memory so it loads faster. The cache is cleared when a task changes.