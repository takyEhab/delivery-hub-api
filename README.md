# delivery-hub-api

Backend REST API for managing delivery orders, drivers, and customers for a small food business. Built with Node.js, Express, PostgreSQL, JWT auth, and bcrypt.

## Setup

1. **Install dependencies**

   ```bash
   npm install
   ```

2. **Configure environment**

   ```bash
   cp .env.example .env
   ```

   Edit `.env` with your PostgreSQL connection string and a strong `JWT_SECRET`.

3. **Create the database and apply schema**

   ```bash
   createdb delivery_hub
   psql delivery_hub -f schema.sql
   ```

   Or set `DATABASE_URL` and run:

   ```bash
   npm run db:schema
   ```

4. **Seed the initial owner account**

   ```bash
   npm run db:seed
   ```

   Defaults: phone `0500000000`, password `changeme123`. Override with `OWNER_NAME`, `OWNER_PHONE`, `OWNER_EMAIL`, `OWNER_PASSWORD`.

5. **Start the server**

   ```bash
   npm run dev
   ```

## API Overview

All protected routes require `Authorization: Bearer <token>`.

| Method | Path | Access | Description |
|--------|------|--------|-------------|
| POST | `/auth/login` | Public | Login (phone + password) |
| POST | `/auth/register` | Owner | Create a driver account |
| GET | `/drivers` | Owner | List drivers |
| POST | `/drivers` | Owner | Create driver |
| PATCH | `/drivers/:id` | Owner | Update / deactivate driver |
| DELETE | `/drivers/:id` | Owner | Remove driver |
| GET | `/customers` | Owner, Driver | List customers |
| GET | `/customers/search?q=` | Owner, Driver | Search by name or phone |
| POST | `/customers` | Owner | Create customer |
| PATCH | `/customers/:id` | Owner | Update customer |
| POST | `/orders` | Owner | Create order (`customer_id` or inline `customer`) |
| GET | `/orders` | Owner (all), Driver (assigned only) | List orders |
| GET | `/orders/:id` | Owner (any), Driver (own) | Order detail |
| PATCH | `/orders/:id/assign` | Owner | Assign driver |
| PATCH | `/orders/:id/status` | Driver | Update delivery status |

### Error responses

```json
{ "error": "Human-readable message" }
```

## Project structure

```
src/
  config/       Database and env configuration
  db/queries/   Parameterized SQL queries
  controllers/  Request handlers
  routes/       Express routers
  middleware/   Auth, validation, error handling
  validators/   Joi schemas
  utils/        JWT, passwords, errors
schema.sql      PostgreSQL tables and relationships
scripts/        Owner seed script
```
