# FeedX HR - Project Context

## Project Overview

**FeedX HR** is an AI-powered workforce management platform designed for modern businesses. It provides comprehensive tools for employee performance management, including AI-driven analytics, 360-degree feedback systems, goal tracking, and secure authentication.

### Key Features
- **AI Analytics**: Predictive performance insights powered by Google Gemini AI
- **360° Feedback**: Multi-rater performance assessments
- **Goal Tracking**: SMART objectives with progress alerts
- **Secure Auth**: JWT + OTP (One-Time Password) verification via email

## Tech Stack

### Frontend
- **React 19** with **Vite 6** as the build tool
- **Tailwind CSS 4** for styling
- **React Router 7** for routing
- **Chart.js / react-chartjs-2** for data visualization
- **Framer Motion** for animations
- **Axios** for HTTP requests
- **React Toastify** for notifications
- **Lucide React / FontAwesome / Heroicons** for icons

### Backend
- **Node.js** with **Express 4**
- **MongoDB** via **Mongoose 8**
- **JWT** (jsonwebtoken) for authentication
- **bcryptjs** for password hashing
- **Nodemailer** for email OTP delivery
- **Google Gemini AI** (`@google/genai`) for AI analytics
- **node-cron** for scheduled tasks (notifications, automated feedback)
- **Cookie Parser** for cookie-based auth

## Project Structure

```
FeedX-main/
├── client/                  # Frontend (React + Vite)
│   ├── public/              # Static assets
│   ├── src/
│   │   ├── assets/          # Images, fonts, etc.
│   │   ├── components/      # Reusable UI components
│   │   ├── context/         # React context providers (e.g., AuthContext)
│   │   ├── layouts/         # Role-based layout components (HR, Manager, Employee)
│   │   ├── pages/           # Page-level components
│   │   │   ├── employee/    # Employee-specific pages
│   │   │   ├── hr/          # HR-specific pages
│   │   │   ├── manager/     # Manager-specific pages
│   │   │   └── homecomponent/  # Landing page components
│   │   ├── App.jsx          # Main app routing and layout
│   │   ├── main.jsx         # Entry point
│   │   └── index.css        # Global styles
│   ├── .env.sample
│   ├── vite.config.js
│   └── package.json
│
└── server/                  # Backend (Express + MongoDB)
    ├── config/              # Database and configuration
    ├── controllers/         # Route controllers (request handlers)
    ├── middleware/          # Auth, validation, error middleware
    ├── models/              # Mongoose schemas
    │   ├── employee.js
    │   ├── feedback.js
    │   ├── feedbackReq.js
    │   ├── goal.js
    │   ├── hr.js
    │   ├── manager.js
    │   ├── notification.js
    │   ├── organisation.js
    │   ├── otp.js
    │   ├── SelfAssessment.js
    │   ├── SelfAssessmentSession.js
    │   └── session.js
    ├── routes/              # API route definitions
    │   ├── authRoute.js
    │   ├── employeeRoute.js
    │   ├── feedbackRoute.js
    │   ├── goalRoute.js
    │   ├── hrRoute.js
    │   ├── managerRoute.js
    │   ├── notificationRoute.js
    │   └── selfAssessmentRoutes.js
    ├── services/            # Business logic services
    ├── utils/               # Utility functions (ExpressError, NotifJob, autoFeedback)
    ├── .env.sample
    └── server.js            # Express app entry point
```

## Architecture

### Role-Based Access Control
The application supports three user roles with dedicated dashboards and layouts:
1. **HR** — Full oversight of employees, managers, and feedback systems
2. **Manager** — Team-level management: employee oversight, feedback requests, goal tracking
3. **Employee** — Personal dashboard: view feedback requests, manage goals, self-assessments

### Authentication Flow
1. User signs up or logs in
2. OTP is sent to email via Nodemailer
3. User verifies OTP to complete authentication
4. JWT tokens stored in HTTP-only cookies for session management

### API Routes
| Prefix | Purpose |
|--------|---------|
| `/api/auth` | Authentication (login, signup, OTP) |
| `/api/hr` | HR-specific operations |
| `/api/feedback` | Feedback management |
| `/api/goal` | Goal tracking |
| `/api/manager` | Manager operations |
| `/api/employee` | Employee operations |
| `/api/notif` | Notifications |
| `/api/self` | Self-assessments |

## Building and Running

### Prerequisites
- Node.js (v18+ recommended)
- MongoDB database (local or MongoDB Atlas)
- Email account for SMTP (for OTP delivery)
- Google Gemini API key

### Backend Setup
```bash
cd server
npm install

# Configure environment
cp .env.sample .env
# Fill in: PORT, MONGO_URL, JWT_SECRET, EMAIL_USER, EMAIL_PASS, GEMINI_API_KEY

# Development mode (with nodemon)
npm run dev

# Production mode
npm start
```

### Frontend Setup
```bash
cd client
npm install

# Configure environment
cp .env.sample .env
# Fill in: VITE_API_KEY

# Development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Lint
npm run lint
```

### Environment Variables

**Backend (`server/.env`)**
```
PORT=<port_number>
MONGO_URL=<mongodb_connection_string>
JWT_SECRET=<your_jwt_signing_secret>
EMAIL_USER=<your_email@gmail.com>
EMAIL_PASS=<your_app_specific_password>
GEMINI_API_KEY=<your_gemini_api_key>
```

**Frontend (`client/.env`)**
```
VITE_API_KEY=<your_vite_api_key>
```

## Development Conventions

- **Frontend**: Uses ESLint with `eslint-plugin-react-hooks` and `eslint-plugin-react-refresh` for code quality
- **Backend**: Uses `nodemon` for hot-reloading during development
- **Error Handling**: Centralized error middleware using custom `ExpressError` class
- **CORS**: Configured to allow origins from localhost (`5173`, `5174`) and deployed Netlify URL
- **Scheduled Tasks**: Background jobs for notifications (`NotifJob`) and automated feedback (`autoFeedback`) run via `node-cron`
