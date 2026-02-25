# Scalable Web Application with Authentication & Dashboard

## Overview

This project is a full-stack web application built using React.js and Node.js that demonstrates secure authentication, protected routes, and dashboard-based CRUD functionality.

The application is structured for scalability, modularity, and production readiness.

---

## Live Deployment

Frontend:
https://siddharth-image-gram.vercel.app/signin

Backend:
https://imagegram-0psf.onrender.com

Test Credentials:
Email: test@example.com  
Password: 123456


## Tech Stack

### Frontend
- React.js (Vite)
- TailwindCSS
- Context API for state management
- Protected Route handling

### Backend
- Node.js
- Express.js
- MongoDB
- JWT Authentication
- bcrypt password hashing

---

## Core Features Implemented

### Authentication (JWT-Based)

- User Registration
- User Login
- Password hashing using bcrypt
- JWT token generation upon login
- JWT verification middleware
- Protected routes (dashboard access restricted)
- Logout functionality (token removal)

---

### Dashboard

- Fetch authenticated user profile
- Create, Read, Update, Delete operations
- Search and filter functionality
- Profile display
- Secure API communication

---

### CRUD Entity

The sample entity used in this project is **Posts** (can function as tasks/notes entity).

CRUD Operations:
- Create Post
- View Posts
- Update Post
- Delete Post

---

## Security Practices

- Password hashing using bcrypt
- JWT authentication middleware
- Token verification on protected endpoints
- Server-side validation
- Client-side form validation
- Structured error handling using middleware
- Environment variable usage for sensitive data

## Project Structure

```
auth-dashboard-app/
│
├── backend/
│   ├── package.json
│   ├── package-lock.json
│   ├── .gitignore
│   └── src/
│       ├── Config/
│       ├── Controllers/
│       ├── Middlewares/
│       ├── repositories/
│       ├── Routers/
│       ├── schema/
│       ├── services/
│       ├── Storage/
│       ├── utils/
│       ├── Validators/
│       └── index.js
│
├── frontend/
│   ├── package.json
│   ├── package-lock.json
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── index.html
│   ├── public/
│   └── src/
│       ├── Authentication/
│       ├── ProtectedRoute/
│       ├── Profile/
│       ├── Posts/
│       ├── Context/
│       ├── Services/
│       ├── Routes.jsx
│       ├── App.jsx
│       └── main.jsx
│
└── README.md


## API Documentation

Base URL (Local):
```
http://localhost:3005/api/v1
```

Base URL (Production - Render):
```
https://imagegram03.onrender.com/api/v1
```

---

## 1️⃣ Authentication APIs

### Register User
**POST** `/user/signup`

Body (x-www-form-urlencoded):
- username
- email
- password

---

### Login User
**POST** `/user/signin`

Body (x-www-form-urlencoded):
- email
- password

Response:
- JWT Token

---

### Send OTP
**POST** `/user/sendotp?email={email}`

---

### Verify OTP
**POST** `/user/verifyotp?email={email}&otp={otp}`

---

## 2️⃣ Posts (CRUD)

All protected routes require header:

```
x-access-token: <JWT_TOKEN>
```

---

### Create Post
**POST** `/posts`

Form-Data:
- image (file)
- caption (text)

---

### Get All Posts
**GET** `/posts`

---

### Update Post
**PUT** `/posts?id={postId}`

Form-Data:
- caption (text)

---

### Delete Post
**DELETE** `/posts?id={postId}`

---

## 3️⃣ Comments

### Create Comment
**POST** `/comments`

Body:
- text
- type (post/comment)
- content_id

Header:
```
x-access-token
```

---

### Show Comments
**GET** `/comments`

---

## 4️⃣ Likes

### Like Post
**POST** `/like/post`

Body:
- post_id

Header:
```
x-access-token
```

---

### Like Comment
**POST** `/like/comment`

Body:
- comment_id

Header:
```
x-access-token
```

---

## 5️⃣ User & Profile

### Get User Feed
**GET** `/user/feedall?page={page}&limit={limit}`

Header:
```
x-access-token
```

---

### Follow User
**PUT** `/community/follow`

Body:
- follow (userId)

Header:
```
x-access-token
```

---

### Get Profile
**GET** `/user/profile`

Header:
```
x-access-token
```

---

## Authentication Flow

1. User signs up or logs in.
2. Backend validates credentials.
3. JWT token is generated.
4. Token is sent in `x-access-token` header.
5. Middleware verifies token before protected routes.
6. Request proceeds if valid.

---

## Security Implementation

- Password hashing using bcrypt
- JWT-based stateless authentication
- Middleware-based route protection
- Input validation
- Error handling middleware



## How to Run Locally

### 1. Clone the Repository

```
git clone <your-repo-link>
cd auth-dashboard-app
```

---

### 2. Setup Backend

```
cd backend
npm install
```

Create a `.env` file in backend folder:

```
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_secret_key
```

Run backend:

```
npm run dev
```

---

### 3. Setup Frontend

```
cd frontend
npm install
npm run dev
```

Frontend will run on:

```
http://localhost:5173
```

Backend runs on:

```
http://localhost:5000
```

---

## Postman Collection

A Postman collection is included in this repository to test:

- Register
- Login
- Authenticated requests
- CRUD operations

---

## Frontend-Backend Integration Flow

1. User registers/logs in.
2. Backend hashes password and generates JWT.
3. Frontend stores JWT securely.
4. Protected routes verify token before rendering dashboard.
5. CRUD operations include JWT in request headers.
6. Backend validates token and processes request.
7. MongoDB handles persistence.
8. Response returned and UI updates.

---

## Security & Architecture Decisions

- Separation of concerns (Controllers, Services, Routes)
- Middleware-based authentication
- Stateless authentication using JWT
- Modular folder structure for scalability
- Environment-based configuration

---

## Conclusion


## Production Scaling Strategy

To scale this application for production-level usage, the following architectural improvements would be implemented:

### 1. Authentication & Token Strategy
- Implement refresh tokens with short-lived access tokens.
- Store refresh tokens securely (HTTP-only cookies).
- Add token rotation and blacklisting for enhanced security.
- Introduce role-based access control (RBAC).

---

### 2. API Scalability
- Separate services into modular microservices if traffic increases.
- Introduce API versioning to maintain backward compatibility.
- Add rate limiting (e.g., express-rate-limit) to prevent abuse.
- Implement request validation using centralized validation middleware.

---

### 3. Caching Layer
- Integrate Redis for:
  - Caching frequently accessed posts/feed
  - Storing session metadata
  - Reducing database load

---

### 4. Database Optimization
- Add proper indexing on frequently queried fields.
- Implement pagination for feed and posts.
- Use aggregation pipelines efficiently.
- Introduce read replicas for scaling read-heavy workloads.

---

### 5. Load Handling & Infrastructure
- Containerize using Docker.
- Deploy using scalable infrastructure (AWS EC2 / ECS / Kubernetes).
- Use NGINX as reverse proxy.
- Enable horizontal scaling via load balancer.

---

### 6. Frontend Optimization
- Code splitting and lazy loading for performance.
- API abstraction layer for centralized request handling.
- Environment-based configuration (dev/staging/prod).
- Implement error boundaries and global error handling.

---

### 7. CI/CD & DevOps
- Setup CI/CD pipeline for automated testing and deployment.
- Add automated linting and security checks.
- Use environment secrets management.

---

### 8. Monitoring & Logging
- Integrate centralized logging (Winston + external log service).
- Add application monitoring (e.g., Prometheus, Grafana).
- Implement structured error reporting.

---

### 9. Security Enhancements
- Enable HTTPS (SSL/TLS).
- Use helmet.js for HTTP security headers.
- Add CORS policy restrictions.
- Protect against common attacks (XSS, CSRF, rate abuse).

---

Overall, the current modular architecture allows easy extension into a scalable, production-ready system by introducing caching, infrastructure scaling, improved authentication flows, and observability mechanisms.
