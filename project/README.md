# Taxi Booking System

A comprehensive taxi booking application with React frontend and Node.js backend, featuring user authentication, booking management, and a user dashboard.

## Features

### Frontend (React + TypeScript)
- 🚗 Vehicle selection and booking
- 👤 User authentication (login/register)
- 📊 User dashboard with booking history
- 🔍 Trip lookup functionality
- 📱 Responsive design with Tailwind CSS
- 🎨 Modern UI with Lucide React icons

### Backend (Node.js + Express)
- 🔐 JWT-based authentication
- 🗄️ MySQL database integration
- 📝 RESTful API endpoints
- 🛡️ Security middleware (helmet, rate limiting)
- ✅ Input validation and error handling
- 🔄 CORS support for frontend integration

### Database (MySQL)
- 👥 Users table with authentication
- 🚙 Vehicles table with pricing
- 📋 Bookings table with trip management
- 🔗 Proper foreign key relationships
- 📊 Optimized indexes for performance

## Tech Stack

### Frontend
- React 18 with TypeScript
- Vite for build tooling
- Tailwind CSS for styling
- Lucide React for icons
- Date-fns for date handling

### Backend
- Node.js with Express
- MySQL2 for database connectivity
- JWT for authentication
- Bcrypt for password hashing
- Express Validator for input validation
- Helmet for security headers
- CORS for cross-origin requests

## Prerequisites

Before running this application, make sure you have:

- Node.js (v16 or higher)
- MySQL (v8.0 or higher)
- npm or yarn package manager

## Installation & Setup

### 1. Clone the Repository

```bash
git clone <repository-url>
cd TaxiBooking/project
```

### 2. Backend Setup

#### Install Dependencies
```bash
cd backend
npm install
```

#### Environment Configuration
1. Copy the environment example file:
```bash
cp .env.example .env
```

2. Update the `.env` file with your configuration:
```env
# Database Configuration
DB_HOST=localhost
DB_PORT=3306
DB_NAME=taxi_booking
DB_USER=root
DB_PASSWORD=your_mysql_password

# JWT Configuration
JWT_SECRET=your_super_secret_jwt_key_here
JWT_EXPIRES_IN=7d

# Server Configuration
PORT=5000
NODE_ENV=development

# CORS Configuration
FRONTEND_URL=http://localhost:5173

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100
```

#### Database Setup
1. Create MySQL database and initialize tables:
```bash
npm run init-db
```

This will:
- Create the `taxi_booking` database
- Set up all required tables (users, vehicles, bookings)
- Insert sample vehicle data
- Create a demo user account

#### Start Backend Server
```bash
npm run dev
```

The backend server will start on `http://localhost:5000`

### 3. Frontend Setup

#### Install Dependencies
```bash
cd ..
npm install
```

#### Start Frontend Development Server
```bash
npm run dev
```

The frontend will start on `http://localhost:5173`

## Demo Credentials

After running the database initialization, you can use these demo credentials:

- **Email:** demo@taxibooking.com
- **Password:** demo123

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - User login
- `GET /api/auth/profile` - Get user profile
- `PUT /api/auth/profile` - Update user profile
- `GET /api/auth/verify` - Verify JWT token

### Users
- `GET /api/users/:userId/dashboard` - Get user dashboard data
- `GET /api/users/:userId/bookings` - Get user bookings
- `PUT /api/users/:userId/bookings/:bookingId/cancel` - Cancel booking

### Bookings
- `POST /api/bookings` - Create new booking
- `GET /api/bookings/trip/:tripId` - Get booking by trip ID
- `GET /api/bookings` - Get user bookings (authenticated)
- `PUT /api/bookings/:bookingId/status` - Update booking status

### Vehicles
- `GET /api/vehicles` - Get all vehicles
- `GET /api/vehicles/:vehicleId` - Get vehicle by ID
- `POST /api/vehicles/:vehicleId/availability` - Check availability

## Database Schema

### Users Table
```sql
CREATE TABLE users (
  id VARCHAR(36) PRIMARY KEY DEFAULT (UUID()),
  email VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  name VARCHAR(255) NOT NULL,
  phone VARCHAR(20) NOT NULL,
  is_verified BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### Vehicles Table
```sql
CREATE TABLE vehicles (
  id VARCHAR(36) PRIMARY KEY,
  type ENUM('bike', 'auto', 'car', 'van', 'lorry') NOT NULL,
  name VARCHAR(255) NOT NULL,
  description TEXT,
  image VARCHAR(500),
  base_price DECIMAL(10, 2) NOT NULL,
  hourly_rate DECIMAL(10, 2) NOT NULL,
  daily_rate DECIMAL(10, 2) NOT NULL,
  features JSON,
  available BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### Bookings Table
```sql
CREATE TABLE bookings (
  id VARCHAR(36) PRIMARY KEY DEFAULT (UUID()),
  trip_id VARCHAR(20) UNIQUE NOT NULL,
  user_id VARCHAR(36),
  vehicle_id VARCHAR(36) NOT NULL,
  rental_type ENUM('location', 'hourly', 'daily') NOT NULL,
  from_date DATETIME NOT NULL,
  to_date DATETIME NOT NULL,
  pickup_location VARCHAR(500),
  dropoff_location VARCHAR(500),
  hours INT,
  days INT,
  total_fare DECIMAL(10, 2) NOT NULL,
  payment_method ENUM('cash', 'card') NOT NULL,
  customer_name VARCHAR(255) NOT NULL,
  customer_email VARCHAR(255) NOT NULL,
  customer_phone VARCHAR(20) NOT NULL,
  is_guest BOOLEAN DEFAULT TRUE,
  status ENUM('confirmed', 'completed', 'cancelled') DEFAULT 'confirmed',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL,
  FOREIGN KEY (vehicle_id) REFERENCES vehicles(id) ON DELETE CASCADE
);
```

## Features Overview

### User Authentication
- Secure registration and login
- JWT token-based authentication
- Password hashing with bcrypt
- Token verification middleware

### Booking System
- Multiple rental types (location-based, hourly, daily)
- Real-time fare calculation
- Vehicle availability checking
- Guest and authenticated user bookings

### User Dashboard
- Booking statistics and analytics
- Recent and upcoming bookings
- Profile management
- Booking history with filtering
- Cancel bookings functionality

### Trip Management
- Unique trip ID generation
- Trip lookup functionality
- Booking status tracking
- Payment method selection

## Development

### Running in Development Mode

1. Start the backend server:
```bash
cd backend
npm run dev
```

2. Start the frontend development server:
```bash
cd ..
npm run dev
```

### Building for Production

1. Build the frontend:
```bash
npm run build
```

2. Preview the production build:
```bash
npm run preview
```

## Security Features

- JWT token authentication
- Password hashing with bcrypt
- Rate limiting to prevent abuse
- CORS configuration
- Input validation and sanitization
- SQL injection prevention
- Security headers with Helmet

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License.

## Support

For support or questions, please open an issue in the repository.

---

**Note:** This is a demo application. For production use, ensure proper security measures, environment configuration, and database optimization.