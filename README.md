# WoodCraft ERP

Next.js + MySQL system for premium wooden furniture manufacturers. It includes a public website, customer portal, admin dashboard, product catalog, custom furniture configurator, smart costing, RFQ, quotation, order, production, invoice, payment, CRM, RBAC, audit log, attachment metadata and demo seed data.

## Stack

- Next.js App Router, TypeScript strict, Tailwind CSS
- Prisma ORM with MySQL
- Zod validation for API payloads
- Vitest for core costing/search/workflow tests
- Recharts/lucide-ready UI foundation

## Quick Start

1. Install dependencies:

```bash
npm install
```

2. Create a MySQL database:

```sql
CREATE DATABASE woodcraft_erp CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

3. Copy environment values:

```bash
cp .env.example .env
```

4. Update `DATABASE_URL` in `.env`:

```env
DATABASE_URL="mysql://USER:PASSWORD@localhost:3306/woodcraft_erp"
AUTH_SECRET="replace-with-a-long-random-secret"
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

5. Generate Prisma Client:

```bash
npm run db:generate
```

6. Apply database schema:

```bash
npm run db:push
```

7. Seed demo data:

```bash
npm run db:seed
```

8. Start development:

```bash
npm run dev
```

9. Build production:

```bash
npm run build
npm run start
```

## Demo Login

- Admin: `admin@woodcraft.test`
- Password: `Admin@123456`

## Important Routes

- Public website: `/`
- Product catalog: `/products`
- Product detail: `/products/[slug]`
- Custom furniture configurator: `/custom-furniture`
- RFQ form: `/request-quote`
- Customer portal: `/portal`
- Admin dashboard: `/admin`
- Cost simulator: `/admin/costing`
- Login screen: `/login`

## Core Workflow

The code models the main operating flow:

`Browse Product -> Customize Product -> Estimated Cost -> Submit RFQ -> Sales Costing -> Quotation -> Customer Approval -> Sales Order -> Production -> Invoice -> Payment -> Completed`

API routes that persist to MySQL:

- `POST /api/custom-requests`
- `POST /api/rfqs`
- `POST /api/quotations/[id]/accept`

## Database

The Prisma schema contains normalized tables for:

- users, roles, permissions, role permissions, user roles
- customers, addresses, leads, lead activities
- categories, collections, rooms, products, variants, images, files, attributes
- materials, material categories, material price history, suppliers, supplier materials
- BOM and BOM items
- custom requests and files
- RFQs and files
- quotations, quotation versions, quotation items
- sales orders, order items, order status history
- production orders, production steps, step files
- invoices, invoice items, payments
- cost models, rules, estimations, estimation items
- notifications, activities, attachments, audit logs

SQL migration output is in `prisma/migrations/000001_init/migration.sql`.

## Verification

Run:

```bash
npm test
npm run typecheck
npm run lint
npm run build
npx prisma validate
```

Database-dependent checks require a running MySQL server:

```bash
npm run db:push
npm run db:seed
```

## Notes

- Prices use Prisma `Decimal` fields in the database schema.
- File binaries are not stored in MySQL; `Attachment` and module-specific file tables store metadata and storage keys for local/S3-compatible storage.
- Cost rules are represented in database tables and the costing module is written so admin-managed values can be passed into the estimator.
- Historical cost prediction starts with rule-based similarity and can be replaced by AI/ML behind the same output shape later.
