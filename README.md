# Sales Insights MIS — TypeORM + PostgreSQL

This version has been reorganized to follow the working layout and startup pattern of the supplied **EduCore_TypeORM_PostgreSQL_Migration_Fixed** reference.

## Stack

- React + Vite
- Node.js + Express
- TypeORM
- PostgreSQL
- JWT authentication

## One-command development startup

Requirements:

- Node.js 18+ (Node.js 20+ recommended)
- npm
- **Either** Docker Desktop **or** a local PostgreSQL server

### Recommended: Docker Desktop

1. Extract the ZIP and open the folder in VS Code.
2. Make sure Docker Desktop is running.
3. Install packages:

```bash
npm install
```

4. Start everything:

```bash
npm run dev
```

`npm run dev` now checks PostgreSQL first. If PostgreSQL is not already running and Docker Desktop is available, it automatically starts the included PostgreSQL container. The backend then runs TypeORM migrations, seeds the demo accounts/data, and starts together with the React frontend.

Open:

```text
http://localhost:5173
```

Backend/API:

```text
http://localhost:4000
```

Health endpoint:

```text
http://localhost:4000/api/health
```

## Demo logins

| Role | Email | Password |
|---|---|---|
| Administrator | `admin@company.com` | `admin123` |
| Executive | `executive@company.com` | `executive123` |
| Regional Manager | `regional@company.com` | `regional123` |
| Marketing | `marketing@company.com` | `marketing123` |
| Data / BI Analyst | `analyst@company.com` | `analyst123` |

The development seed is idempotent and keeps these packaged demo accounts active with the documented passwords, preventing stale seed data from causing login failures.

## Database commands

```bash
npm run db:up
npm run db:migrate
npm run db:seed
npm run db:down
```

A destructive reset is protected. In Windows PowerShell:

```powershell
$env:ALLOW_DB_RESET="true"
npm run db:reset
```

## Using an existing local PostgreSQL installation

Copy `.env.example` to `.env` and set your real PostgreSQL values:

```env
DB_HOST=127.0.0.1
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=YOUR_POSTGRES_PASSWORD
DB_NAME=sales_insights_mis
DB_SSL=false
DB_LOGGING=false
```

Create the database if needed, then run:

```bash
npm run db:migrate
npm run db:seed
npm run dev
```

## Project layout

```text
sales-insights-mis/
├── server/
│   ├── database/
│   │   ├── data-source.js
│   │   ├── entities/
│   │   ├── migrations/
│   │   ├── scripts/
│   │   ├── services/
│   │   ├── schema.sql
│   │   └── reporting-queries.sql
│   ├── middleware/
│   ├── routes/
│   ├── services/
│   └── server.js
├── src/
├── docker-compose.yml
├── .env.example
├── DATABASE.md
├── package.json
└── vite.config.js
```

The previous Windows `.bat` setup/start files are intentionally removed. Frontend and backend are started together with only `npm run dev`.


## UI stack
The interface now uses **Tailwind CSS** for the visual system and responsive polish, with **Font Awesome** icons across navigation, KPI cards, authentication and utility controls.

After extracting the project run:
```bash
npm install
npm run dev
```
The frontend and backend start together through the existing full-stack development command.


## TypeORM + SQL integration

The backend now uses a hybrid database approach:

- **TypeORM entities/repositories** for authentication, CRUD operations, relationships, transactions, migrations and seeding.
- **PostgreSQL SQL** for reporting and analytical queries where aggregate SQL is clearer and more efficient.
- **Migration 2** adds customer segmentation, promotion-to-product relationships, promotion sales attribution, profit targets, customer codes and richer import metadata.

New SQL-backed API endpoints include:

```text
GET /api/analytics/overview
GET /api/analytics/regions
GET /api/analytics/products
GET /api/analytics/monthly
GET /api/analytics/customers
GET /api/marketing/segments
GET /api/marketing/promotions
```

All protected endpoints use the same JWT login already used by the dashboards. Regional managers remain limited to their assigned region in the analytics endpoints.
