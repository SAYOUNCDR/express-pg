# PgNode

PgNode is a lightweight REST API built with Express and PostgreSQL for managing user records. It demonstrates a modular Node.js service with basic CRUD operations, environment-driven configuration, and centralized error handling.

## Features

- CRUD endpoints for users backed by PostgreSQL.
- Automatic table provisioning on startup.
- Centralized error handler with JSON responses.
- Environment-based configuration with dotenv and connection pooling.
- Ready-to-run Nodemon development workflow.

## Project Structure

```
src/
  index.js             # Express app setup and server bootstrap
  config/db.js         # PostgreSQL connection pool
  controllers/         # Request handlers
  data/                # Table bootstrap and SQL assets
  middlewares/         # Shared middleware (error handling)
  models/              # Database queries
  routes/              # Route definitions
```

## Prerequisites

- Node.js 18+
- PostgreSQL 14+

## Environment Variables

Create a `.env` file in the project root and provide the following keys:

```
PORT=3001            # Optional; defaults to 3001
USER=postgres        # Database user
PASSWORD=secret      # Database password
HOST=localhost       # Database host
DATABASE=pgnode      # Database name
DBPORT=5432          # Database port
```

Ensure the specified database already exists; the app will create the `users` table if needed.

## Installation

```bash
npm install
```

## Running the Server

```bash
npm run dev
```

The command starts Nodemon with `src/index.js`. On launch the service will provision the `users` table if it does not exist and log the connection status. Visit `http://localhost:3001/` to verify connectivity.

## API Endpoints

Base URL: `/api`

| Method | Endpoint    | Description            |
| ------ | ----------- | ---------------------- |
| POST   | `/user`     | Create a new user      |
| GET    | `/user`     | List all users         |
| GET    | `/user/:id` | Fetch a user by ID     |
| PUT    | `/user/:id` | Update user name/email |
| DELETE | `/user/:id` | Remove a user          |

### Request Examples

Create user

```http
POST /api/user
Content-Type: application/json

{
  "name": "Ada Lovelace",
  "email": "ada@example.com"
}
```

Update user

```http
PUT /api/user/1
Content-Type: application/json

{
  "name": "Ada L.",
  "email": "ada.l@example.com"
}
```

## Error Handling

All unhandled errors are routed through a centralized middleware that returns a JSON payload:

```json
{
  "status": 500,
  "message": "Something went wrong",
  "error": "Error message"
}
```

## Database Notes

- Table schema: `id SERIAL PRIMARY KEY`, `name` and `email` fields, and `created_at` timestamp.
- The bootstrap script in `src/data/createUserTable.js` runs automatically on startup; you can also inspect `src/data/data.sql` for the schema definition.


