# OSINT Platform - Development Guide

## Backend Setup

### Prerequisites
- Node.js 18+
- PostgreSQL 14+

### Installation

1. Navigate to backend directory:
```bash
cd backend
```

2. Install dependencies:
```bash
npm install
```

3. Create `.env` file from `.env.example`:
```bash
cp .env.example .env
```

4. Update `.env` with your database credentials:
```
DATABASE_URL="postgresql://user:password@localhost:5432/osint_platform"
JWT_SECRET="your-super-secret-jwt-key"
JWT_REFRESH_SECRET="your-super-secret-refresh-key"
NODE_ENV="development"
PORT=5000
```

5. Run Prisma migrations:
```bash
npm run prisma:migrate
```

6. Start the development server:
```bash
npm run dev
```

Server will run on `http://localhost:5000`

## Frontend Setup

### Prerequisites
- Node.js 18+
- npm or yarn

### Installation

1. Navigate to frontend directory:
```bash
cd frontend
```

2. Install dependencies:
```bash
npm install
```

3. Create `.env` file from `.env.example`:
```bash
cp .env.example .env
```

4. Update `.env` if needed (default is `http://localhost:5000/api`)

5. Start the development server:
```bash
npm start
```

App will run on `http://localhost:3000`

## Features

### Authentication
- User registration and login
- JWT-based authentication (15-minute access token, 7-day refresh token)
- HTTP-only cookies for refresh tokens
- Automatic token refresh

### Search Features
- Mobile number lookup (10-digit Indian format)
- Vehicle registration lookup (KA19HV4003 format)
- Search history with pagination
- Copy results to clipboard

### Admin Features
- View all users
- View all searches across platform
- Delete users
- User and search analytics

### Security
- Bcrypt password hashing
- Input validation with Zod
- SQL injection prevention (Prisma ORM)
- XSS protection (Helmet)
- CORS properly configured
- Rate limiting (10 requests/minute per user)

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `POST /api/auth/refresh` - Refresh access token
- `POST /api/auth/logout` - Logout user
- `GET /api/auth/me` - Get current user

### Search
- `POST /api/search/mobile` - Lookup mobile number
- `POST /api/search/vehicle` - Lookup vehicle registration
- `GET /api/search/history` - Get user search history

### Admin
- `GET /api/admin/users` - List all users (with pagination)
- `GET /api/admin/users/:id` - Get user with their searches
- `DELETE /api/admin/users/:id` - Delete user
- `GET /api/admin/searches` - List all searches (with pagination)

## Database Schema

### User Table
- `id` - Integer (Primary Key)
- `email` - String (Unique)
- `password` - String (Bcrypt hashed)
- `name` - String
- `role` - String (user/admin)
- `createdAt` - DateTime
- `updatedAt` - DateTime

### Search Table
- `id` - Integer (Primary Key)
- `userId` - Integer (Foreign Key)
- `type` - String (mobile/vehicle)
- `query` - String
- `result` - JSON
- `source` - String (API name)
- `createdAt` - DateTime

### ApiKey Table
- `id` - Integer (Primary Key)
- `key` - String (Unique)
- `name` - String
- `rateLimit` - Integer
- `isActive` - Boolean
- `createdAt` - DateTime

## External API Integrations

### Mobile Number Lookup
- **Primary API**: https://akash-num2info-api.vercel.app/?num={number}&key=10
- **Fallback**: https://exploitsindia.site/track/live.php?term={number} (via CORS proxy)

### Vehicle Registration Lookup
- **Primary API**: https://app.turtlemintinsurance.com/api/findregistrationresult?registration={number}&vertical=FW
- **Fallback**: https://vehinfo.ek4nsh.in/api/vehicle?rc={number}

## Troubleshooting

### Database Connection Error
- Ensure PostgreSQL is running
- Check DATABASE_URL in .env file
- Verify database credentials

### Port Already in Use
- Backend: Change PORT in .env (default 5000)
- Frontend: Set PORT environment variable (default 3000)

### CORS Errors
- Check CORS_ORIGIN in backend .env matches frontend URL
- Ensure credentials: true in frontend API client

### Token Expiration
- Access tokens expire in 15 minutes
- Refresh tokens expire in 7 days
- Frontend automatically handles token refresh

## Testing

### Test User Creation
```bash
# Register at http://localhost:3000/register
# Or use API directly
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123","name":"Test User"}'
```

### Test Mobile Lookup
```bash
curl -X POST http://localhost:5000/api/search/mobile \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"number":"9876543210"}'
```

### Test Vehicle Lookup
```bash
curl -X POST http://localhost:5000/api/search/vehicle \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"registration":"KA19HV4003"}'
```

## Production Deployment

### Environment Variables (Production)
```bash
NODE_ENV="production"
JWT_SECRET="your-production-secret-key"
JWT_REFRESH_SECRET="your-production-refresh-key"
DATABASE_URL="postgresql://user:password@prod-db:5432/osint_platform"
CORS_ORIGIN="https://your-domain.com"
```

### Build Commands
```bash
# Backend
cd backend && npm run build

# Frontend
cd frontend && npm run build
```

## License

MIT
