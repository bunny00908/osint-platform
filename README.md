# OSINT Platform

A complete Open Source Intelligence platform for Indian mobile number and vehicle registration lookup with authentication, analytics, and admin management.

## Features

- 🔐 JWT-based authentication with refresh tokens
- 📱 Mobile number lookup (10-digit Indian format)
- 🚗 Vehicle registration lookup (Indian format: KA19HV4003)
- 📊 Analytics dashboard with charts
- 🔍 Search history with pagination
- 👤 User profile management
- 👨‍💼 Admin panel for user management
- ⚡ Rate limiting (10 requests/minute per user)
- 📝 Audit trails and request logging
- 🌓 Dark/Light theme support
- 📱 Fully responsive design

## Tech Stack

### Frontend
- React 18 with TypeScript
- TailwindCSS for styling
- React Query for data fetching
- Recharts for analytics
- React Router for navigation

### Backend
- Node.js + Express with TypeScript
- PostgreSQL with Prisma ORM
- JWT authentication
- Helmet for security
- Zod for validation

## API Integrations

### Mobile Lookup
- Primary: Akash Num2Info API
- Fallback: Exploits India with CORS proxy

### Vehicle Lookup
- Primary: Turtle Mint Insurance API
- Fallback: VehInfo API with CORS proxy

## Installation

### Prerequisites
- Node.js 18+
- PostgreSQL 14+
- npm or yarn

### Backend Setup

```bash
cd backend
npm install
cp .env.example .env
# Configure your database URL and JWT secret
npx prisma migrate dev
npm run dev
```

### Frontend Setup

```bash
cd frontend
npm install
cp .env.example .env
# Configure API URL
npm start
```

## Environment Variables

See `.env.example` files in both frontend and backend directories.

## Security Features

- ✅ Bcrypt password hashing
- ✅ JWT with 15min access tokens, 7-day refresh tokens
- ✅ HTTP-only cookies for refresh tokens
- ✅ Input validation with Zod
- ✅ SQL injection prevention (Prisma)
- ✅ XSS protection (Helmet)
- ✅ Rate limiting per user/IP
- ✅ CORS properly configured

## Database Schema

- **User**: id, email, password, name, role, createdAt
- **Search**: id, userId, type, query, result, source, createdAt
- **ApiKey**: id, key, name, rateLimit, isActive

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `POST /api/auth/refresh` - Refresh access token
- `POST /api/auth/logout` - Logout user

### Search
- `POST /api/search/mobile` - Lookup mobile number
- `POST /api/search/vehicle` - Lookup vehicle registration
- `GET /api/search/history` - Get user search history

### Admin
- `GET /api/admin/users` - List all users
- `GET /api/admin/searches` - List all searches
- `DELETE /api/admin/users/:id` - Delete user

## License

MIT
