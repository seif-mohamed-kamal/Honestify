# 🎯 Honestify - Anonymous Messaging Platform Backend API

> **A secure, scalable backend RESTful API for an anonymous messaging platform that enables users to receive honest feedback and messages while maintaining privacy and security.**

![Language](https://img.shields.io/badge/Language-JavaScript%2099.2%25-yellow)
![HTML](https://img.shields.io/badge/HTML-0.8%25-red)
![Node.js](https://img.shields.io/badge/Node.js-Backend-green)
![License](https://img.shields.io/badge/License-ISC-blue)

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [Technology Stack](#-technology-stack)
- [System Architecture](#-system-architecture)
- [Folder Structure](#-folder-structure)
- [Installation & Setup](#-installation--setup)
- [Environment Variables](#-environment-variables)
- [API Documentation](#-api-documentation)
- [How It Works](#-how-it-works)
- [How to Use](#-how-to-use)
- [Database Models](#-database-models)
- [Security Features](#-security-features)
- [Error Handling](#-error-handling)
- [Contributing](#-contributing)
- [Author](#-author)

---

## 🌟 Overview

**Honestify** is a robust backend API built with Node.js and Express.js that powers an anonymous messaging platform. The platform allows users to share their profiles with others who can send them anonymous messages, providing a safe space for honest feedback and transparent communication.

### Core Concept:
- Users create accounts and generate unique profile links
- Others can send anonymous or authenticated messages to users
- Messages can include attachments (images, files)
- Users receive notifications via email
- Advanced security with JWT tokens, encryption, and rate limiting
- Redis for session management and token handling

---

## ✨ Key Features

### 🔐 **Authentication & Authorization**
- JWT-based authentication with Access & Refresh tokens
- Email verification with OTP confirmation
- Google OAuth 2.0 integration for social login
- Two-factor verification support
- Secure password reset via OTP and email links
- Rate limiting on login attempts (max 5 attempts in 10 minutes)

### 💬 **Messaging System**
- Send anonymous or authenticated messages
- File attachments support (images, documents)
- Message retrieval and management
- Message deletion
- Message timestamps and metadata

### 👤 **User Management**
- Complete user profiles with customizable information
- Profile picture and cover images
- Public profile sharing via unique links
- User visit tracking
- Account activation/deactivation
- Automatic cleanup of unconfirmed accounts

### 📧 **Email Services**
- Automated OTP delivery via email
- Beautiful HTML email templates
- Password reset notifications
- Email verification workflows
- Event-driven email processing with NodeMailer

### 📁 **File Management**
- Multer integration for file uploads
- Profile picture uploads
- Cover image galleries
- Message attachments
- File validation and size restrictions
- Local storage management

### 🚀 **Performance & Security**
- Redis caching for tokens and sessions
- CORS protection with origin validation
- Helmet.js for HTTP headers security
- Request validation using Joi
- Rate limiting on all endpoints
- SQL injection prevention
- XSS protection
- Encrypted sensitive data (phone numbers)

### 🔄 **Token Management**
- Access token rotation system
- Refresh token functionality
- Token expiration handling
- Secure token storage in Redis
- Admin-specific tokens support

---

## 🛠️ Technology Stack

### Backend Framework & Runtime
| Technology | Version | Purpose |
|------------|---------|---------|
| **Node.js** | Latest | JavaScript runtime |
| **Express.js** | ^5.2.1 | Web framework & routing |
| **JavaScript (ES Modules)** | ES6+ | Language |

### Database & Data Management
| Technology | Version | Purpose |
|------------|---------|---------|
| **MongoDB** | Latest | NoSQL database |
| **Mongoose** | ^9.2.0 | ODM for MongoDB |
| **Redis** | ^5.11.0 | In-memory caching & session storage |

### Security & Authentication
| Technology | Version | Purpose |
|------------|---------|---------|
| **JWT (jsonwebtoken)** | ^9.0.3 | Token-based authentication |
| **Bcrypt** | ^6.0.0 | Password hashing |
| **Argon2** | ^0.44.0 | Advanced password hashing algorithm |
| **Helmet** | ^8.1.0 | HTTP headers security |
| **google-auth-library** | ^10.6.1 | Google OAuth 2.0 verification |

### File Management & Upload
| Technology | Version | Purpose |
|------------|---------|---------|
| **Multer** | ^2.1.0 | File upload middleware |

### Email & Communication
| Technology | Version | Purpose |
|------------|---------|---------|
| **NodeMailer** | ^8.0.1 | Email sending service |

### Validation & Data Processing
| Technology | Version | Purpose |
|------------|---------|---------|
| **Joi** | ^18.0.2 | Schema validation |
| **Dotenv** | ^17.2.3 | Environment variable management |
| **CORS** | ^2.8.6 | Cross-Origin Resource Sharing |
| **express-rate-limit** | ^8.3.1 | Rate limiting middleware |

### Development Tools
| Technology | Version | Purpose |
|------------|---------|---------|
| **cross-env** | ^10.1.0 | Cross-platform env variables |

---

## 🏗️ System Architecture

### Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                             │
│              (Web Browser / Mobile App)                          │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ↓
┌─────────────────────────────────────────────────────────────────┐
│                    MIDDLEWARE LAYER                             │
├─────────────────────────────────────────────────────────────────┤
│  • CORS & Helmet Security                                       │
│  • Rate Limiting (express-rate-limit)                          │
│  • Authentication Middleware (JWT verification)                 │
│  • Authorization Middleware (Role-based access control)        │
│  • Validation Middleware (Joi schema validation)                │
│  • Error Handling Middleware                                    │
└────────────────┬───────────────────────────────┬────────────────┘
                 │                               │
                 ↓                               ↓
    ┌────────────────────────┐    ┌─────────────────────────┐
    │   ROUTING LAYER        │    │   SECURITY LAYER        │
    ├────────────────────────┤    ├─────────────────────────┤
    │  • /auth routes        │    │  • Token Generation     │
    │  • /user routes        │    │  • Password Hashing     │
    │  • /message routes     │    │  • Encryption/Decryption│
    └────────┬───────────────┘    │  • OTP Management       │
             │                    └────────┬────────────────┘
             ↓                             │
┌─────────────────────────────────────────────────────────────────┐
│                  CONTROLLER LAYER                               │
├─────────────────────────────────────────────────────────────────┤
│  • AuthController    • UserController    • MessageController    │
└──────────┬─────────────────┬──────────────────┬────────────────┘
           │                 │                  │
           ↓                 ↓                  ↓
┌─────────────────────────────────────────────────────────────────┐
│                   SERVICE LAYER                                 │
├─────────────────────────────────────────────────────────────────┤
│  Business logic for:                                            │
│  • Auth Services (signup, login, password reset)               │
│  • User Services (profile management, updates)                 │
│  • Message Services (send, retrieve, delete)                   │
│  • Email Services (OTP, notifications)                         │
│  • File Services (upload, validation)                          │
└────────┬──────────────────┬───────────────────┬────────────────┘
         │                  │                   │
         ↓                  ↓                   ↓
┌───────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│  DATABASE LAYER   │  │  CACHE LAYER     │  │  EMAIL LAYER     │
├───────────────────┤  ├──────────────────┤  ├──────────────────┤
│  • MongoDB        │  │  • Redis Cache   │  │  • NodeMailer    │
│  • Mongoose ODM   │  │  • Token Storage │  │  • Email Events  │
│  • Models         │  │  • Session Mgmt  │  │  • Templates     │
│  • Repositories   │  │  • Rate Limiting │  │                  │
└─────────┬─────────┘  └────────┬─────────┘  └────────┬─────────┘
          │                     │                     │
          ↓                     ↓                     ↓
    ┌──────────────────────────────────────────────────────┐
    │         EXTERNAL SERVICES                            │
    ├──────────────────────────────────────────────────────┤
    │  • MongoDB Atlas (Cloud Database)                    │
    │  • Redis Server (Caching & Sessions)                │
    │  • Gmail/SMTP Server (Email Delivery)               │
    │  • Google OAuth (Social Authentication)             │
    └──────────────────────────────────────────────────────┘
```

### Request Flow Diagram

```
USER REQUEST
    │
    ├─→ CORS Check
    │   └─→ Helmet Headers
    │       └─→ Rate Limiter
    │           └─→ Body Parser (JSON)
    │               └─→ Route Matching
    │                   │
    │                   ├─→ Authentication Middleware (if protected route)
    │                   │   └─→ JWT Verification
    │                   │       └─→ Redis Token Check
    │                   │
    │                   ├─→ Validation Middleware
    │                   │   └─→ Joi Schema Validation
    │                   │
    │                   └─→ Controller
    │                       └─→ Service Layer
    │                           ├─→ Database Operation (MongoDB)
    │                           ├─→ Cache Operation (Redis)
    │                           └─→ Email Operation (NodeMailer)
    │
    └─→ Response Handler
        └─→ Error Handler (if any error)
            └─→ JSON Response to Client
```

---

## 📁 Folder Structure

```
Honestify/
│
├── 📄 package.json                 # Project dependencies & scripts
├── 📄 package-lock.json            # Locked dependency versions
├── 📄 README2.md                   # Original documentation
├── 📄 index.html                   # File upload test page
├── 📄 search.txt                   # Search utilities
├── 📄 .gitignore                   # Git ignore rules
│
├── 📁 config/                      # Configuration files
│   ├── .env.development            # Development environment variables
│   ├── .env.production             # Production environment variables
│   └── config.service.js           # Config loader service
│
└── 📁 src/                         # Source code
    │
    ├── 📄 main.js                  # Application entry point
    ├── 📄 app.bootstrap.js         # Express app initialization
    │
    ├── 📁 common/                  # Shared utilities & constants
    │   │
    │   ├── 📁 enums/               # Enumeration constants
    │   │   ├── user.enum.js        # User role & provider enums
    │   │   ├── email.enum.js       # Email subject enums
    │   │   └── security.enum.js    # Token type enums
    │   │
    │   ├── 📁 service/             # Shared services
    │   │   └── (Redis utilities)   # Cache operations
    │   │
    │   └── 📁 utils/               # Utility functions
    │       │
    │       ├── 📁 security/        # Security utilities
    │       │   ├── token.security.js      # JWT operations
    │       │   ├── hash.security.js       # Password hashing
    │       │   ├── encryption.security.js # Data encryption
    │       │   └── index.js               # Export security utils
    │       │
    │       ├── 📁 mailer/          # Email handling
    │       │   ├── mailer.js       # Email sending logic
    │       │   ├── template.js     # Email HTML templates
    │       │   ├── event.mailer.js # Event-driven email
    │       │   └── index.js        # Export mailer
    │       │
    │       ├── 📁 multer/          # File upload handling
    │       │   ├── local.multer.js # Multer configuration
    │       │   ├── validatio.multer.js  # File validation
    │       │   └── index.js        # Export multer
    │       │
    │       ├── 📁 response/        # Response formatting
    │       │   └── (Response helpers)
    │       │
    │       ├── validation.js       # Input validation schemas
    │       └── index.js            # Export utilities
    │
    ├── 📁 middleware/              # Express middleware
    │   ├── auth.middleware.js      # Authentication & authorization
    │   ├── validatio.middleware.js # Request validation
    │   └── index.js                # Export middleware
    │
    ├── 📁 modules/                 # Feature modules
    │   │
    │   ├── 📁 auth/                # Authentication module
    │   │   ├── auth.controller.js  # Auth request handlers
    │   │   ├── auth.service.js     # Auth business logic
    │   │   ├── auth.validation.js  # Auth input schemas
    │   │   └── index.js            # Export auth router
    │   │
    │   ├── 📁 user/                # User management module
    │   │   ├── user.controller.js  # User request handlers
    │   │   ├── user.service.js     # User business logic
    │   │   ├── user.validation.js  # User input schemas
    │   │   ├── user.authrize.js    # User authorization
    │   │   └── index.js            # Export user router
    │   │
    │   ├── 📁 message/             # Messaging module
    │   │   ├── message.controller.js  # Message request handlers
    │   │   ├── messaga.services.js    # Message business logic
    │   │   ├── message.validation.js  # Message input schemas
    │   │   └── index.js               # Export message router
    │   │
    │   └── index.js                # Export all routers
    │
    └── 📁 DB/                      # Database layer
        │
        ├── connection.db.js        # MongoDB connection
        ├── radis.connection.db.js  # Redis connection
        ├── DB.repositry.js         # Database operations
        │
        ├── 📁 model/               # Data models
        │   ├── user.model.js       # User schema & model
        │   ├── message.model.js    # Message schema & model
        │   ├── token.model.js      # Token schema & model
        │   └── index.js            # Export models
        │
        └── 📁 repository/          # Repository pattern (optional)

```

### Directory Descriptions:

| Directory | Purpose |
|-----------|---------|
| `/config` | Environment configuration and settings management |
| `/src/common` | Shared utilities, enums, and constants used across modules |
| `/src/middleware` | Express middleware for authentication, validation, error handling |
| `/src/modules` | Feature modules organized by domain (auth, user, message) |
| `/src/DB` | Database connections, models, and data access layer |

---

## 🚀 Installation & Setup

### Prerequisites
- **Node.js** (v16 or higher)
- **npm** or **yarn**
- **MongoDB** (local or Atlas)
- **Redis** (local or cloud)
- **Gmail Account** (for email functionality)

### Step 1: Clone the Repository

```bash
git clone https://github.com/seif-mohamed-kamal/Honestify.git
cd Honestify
```

### Step 2: Install Dependencies

```bash
npm install
# or
yarn install
```

### Step 3: Setup Environment Variables

Create a `.env.development` file in the `config/` directory:

```env
# Server
PORT=3000
NODE_ENV=development

# Database
DB_URI=mongodb://localhost:27017/honestify
# OR for MongoDB Atlas:
# DB_URI=mongodb+srv://username:password@cluster.mongodb.net/honestify

# JWT Secrets (generate secure random strings)
JWT_SECRET=your_super_secret_jwt_key_here_min_32_chars
JWT_SECRET_refresh=your_refresh_token_secret_here
JWT_SECRET_ADMIN=your_admin_token_secret_here
JWT_SECRET_ADMIN_refresh=your_admin_refresh_secret_here
JWT_SECRET_RESET=your_password_reset_secret_here
JWT_EXPIRES_IN=7d

# Encryption
ENCRYPT_KEY=your_32_character_encryption_key

# Redis
RADIS_URI=redis://localhost:6379
# OR for Redis Cloud:
# RADIS_URI=redis://:password@host:port

# Email (Gmail)
APP_GMAIL=your-email@gmail.com
APP_PASSWORD=your-gmail-app-password

# Security
SALT_ROUND=10

# Google OAuth
GOOGLE_CLIENT_ID=your-google-client-id.apps.googleusercontent.com
```

### Step 4: Start the Server

#### Development Mode (with auto-reload)
```bash
npm run start:dev
```

#### Production Mode
```bash
npm run start:prod
```

### Step 5: Verify Installation

```bash
# Server should be running on http://localhost:3000
curl http://localhost:3000
# Response: "Hello World!"
```

---

## 🔐 Environment Variables

### Complete Environment Variables Reference

```env
# ============ SERVER CONFIG ============
PORT=3000                                    # Server port
NODE_ENV=development                         # Environment: development/production

# ============ DATABASE CONFIG ============
DB_URI=mongodb://localhost:27017/honestify   # MongoDB URI

# ============ JWT SECRETS ============
JWT_SECRET=your_access_token_secret         # User access token secret
JWT_SECRET_refresh=your_refresh_secret      # User refresh token secret
JWT_SECRET_ADMIN=your_admin_access_secret   # Admin access token secret
JWT_SECRET_ADMIN_refresh=your_admin_refresh # Admin refresh token secret
JWT_SECRET_RESET=your_reset_secret          # Password reset token secret
JWT_EXPIRES_IN=7d                           # Token expiration time

# ============ ENCRYPTION ============
ENCRYPT_KEY=32-character-encryption-key    # Data encryption key

# ============ REDIS CONFIG ============
RADIS_URI=redis://localhost:6379           # Redis connection URI

# ============ EMAIL CONFIG ============
APP_GMAIL=your-email@gmail.com              # Gmail address
APP_PASSWORD=your-app-specific-password    # Gmail app password (NOT regular password)

# ============ SECURITY ============
SALT_ROUND=10                               # Bcrypt salt rounds
```

### How to Generate Secrets:

```javascript
// Generate random string for JWT secrets
const crypto = require('crypto');
const secret = crypto.randomBytes(32).toString('hex');
console.log(secret);

// Or use online generator: https://randomkeygen.com/
```

---

## 📚 API Documentation

### Base URL
```
http://localhost:3000
```

### Response Format (Success)
```json
{
  "status": 200,
  "message": "Success message",
  "data": { /* response data */ }
}
```

### Response Format (Error)
```json
{
  "error_message": "Error description",
  "extra": { /* additional error info */ },
  "stack": "stack trace (development only)"
}
```

---

## 🔐 Authentication Module (`/auth`)

### 1. **Sign Up**
```http
POST /auth/signup
Content-Type: application/json

{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "password": "SecurePass123!",
  "phone": "+1234567890",
  "role": 1,
  "two_step_verefication": true
}
```

**Response (201 Created):**
```json
{
  "status": 201,
  "message": "User registered successfully",
  "data": {
    "_id": "user_id",
    "email": "john@example.com",
    "firstName": "John",
    "lastName": "Doe"
  }
}
```

### 2. **Sign Up with Google**
```http
POST /auth/signup/gmail
Content-Type: application/json

{
  "idToken": "google_id_token"
}
```

**Response (201 Created):**
```json
{
  "status": 201,
  "data": {
    "accessToken": "jwt_access_token",
    "refreshToken": "jwt_refresh_token"
  }
}
```

### 3. **Login**
```http
POST /auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "SecurePass123!"
}
```

**Response (200 OK):**
```json
{
  "status": 200,
  "message": "Login successful",
  "data": {
    "accessToken": "jwt_access_token",
    "refreshToken": "jwt_refresh_token",
    "user": {
      "_id": "user_id",
      "email": "john@example.com",
      "firstName": "John"
    }
  }
}
```

### 4. **Confirm Email (OTP)**
```http
PATCH /auth/confirmEmail
Content-Type: application/json

{
  "email": "john@example.com",
  "otp": "123456"
}
```

### 5. **Resend OTP**
```http
PATCH /auth/resendOtp
Content-Type: application/json

{
  "email": "john@example.com"
}
```

### 6. **Forget Password**
```http
POST /auth/forgetpassword
Content-Type: application/json

{
  "email": "john@example.com"
}
```

### 7. **Verify Forget Password OTP**
```http
PATCH /auth/verify-forget-password-otp
Content-Type: application/json

{
  "email": "john@example.com",
  "otp": "123456"
}
```

### 8. **Reset Password**
```http
PATCH /auth/reset-password
Content-Type: application/json

{
  "email": "john@example.com",
  "otp": "123456",
  "password": "NewSecurePass123!"
}
```

---

## 👤 User Module (`/user`)

### 1. **Get User Profile (Protected)**
```http
GET /user/
Authorization: Bearer <access_token>
```

**Response (200 OK):**
```json
{
  "status": 200,
  "data": {
    "_id": "user_id",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "profilePicture": "url",
    "coverProfilePicture": ["url"],
    "visited": 150
  }
}
```

### 2. **Get Public Profile**
```http
GET /user/share-profile/:userId
```

**Response (200 OK):**
```json
{
  "status": 200,
  "data": {
    "_id": "user_id",
    "firstName": "John",
    "lastName": "Doe",
    "profilePicture": "url",
    "visited": 150
  }
}
```

### 3. **Rotate Token (Get New Access Token)**
```http
GET /user/rotate-token
Authorization: Bearer <refresh_token>
```

**Response (200 OK):**
```json
{
  "status": 200,
  "data": {
    "accessToken": "new_jwt_access_token"
  }
}
```

### 4. **Upload Profile Picture**
```http
PATCH /user/upload/profile-picture
Authorization: Bearer <access_token>
Content-Type: multipart/form-data

file: <image_file>
```

### 5. **Upload Cover Pictures**
```http
PATCH /user/upload/cover-picture
Authorization: Bearer <access_token>
Content-Type: multipart/form-data

file: <image_file>
```

### 6. **Update Password**
```http
PATCH /user/update-password
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "oldPassword": "CurrentPass123!",
  "newPassword": "NewPass123!"
}
```

### 7. **Logout (Protected)**
```http
POST /user/logout
Authorization: Bearer <access_token>
```

### 8. **Send Password Reset Link**
```http
POST /user/foreget-password-by-link
Content-Type: application/json

{
  "email": "john@example.com"
}
```

### 9. **Reset Password by Link**
```http
POST /user/reset-password-by-link
Content-Type: application/json

{
  "email": "john@example.com",
  "token": "reset_token",
  "password": "NewPass123!"
}
```

### 10. **Delete Unconfirmed Users (Admin)**
```http
DELETE /user/delete-unconfirmed-users
Authorization: Bearer <admin_access_token>
```

---

## 💬 Message Module (`/message`)

### 1. **Send Message**
```http
POST /message/:receiverId
Content-Type: multipart/form-data

messageText: "Your feedback here"
file: <optional_attachment>
isAnonymous: true/false
Authorization: Bearer <access_token> (optional)
```

**Response (201 Created):**
```json
{
  "status": 201,
  "message": "Message sent successfully",
  "data": {
    "_id": "message_id",
    "receiverId": "user_id",
    "senderId": "sender_id_or_null",
    "messageText": "Your feedback",
    "attachment": "file_url",
    "isAnonymous": true,
    "createdAt": "2024-01-01T10:30:00Z"
  }
}
```

### 2. **Get All Messages**
```http
GET /message/list
Authorization: Bearer <access_token>
```

**Response (200 OK):**
```json
{
  "status": 200,
  "data": [
    {
      "_id": "message_id",
      "senderId": "sender_id_or_null",
      "messageText": "Feedback content",
      "isAnonymous": true,
      "createdAt": "2024-01-01T10:30:00Z"
    }
  ]
}
```

### 3. **Get Single Message**
```http
GET /message/:messageId
Authorization: Bearer <access_token>
```

### 4. **Delete Message**
```http
DELETE /message/:messageId
Authorization: Bearer <access_token>
```

**Response (200 OK):**
```json
{
  "status": 200,
  "message": "Message deleted successfully"
}
```

---

## 🔄 How It Works

### **User Registration & Email Verification Flow**

```
1. User Registers
   ├─→ Validate Input (Joi schema)
   ├─→ Check Email Uniqueness
   ├─→ Hash Password (Bcrypt/Argon2)
   ├─→ Create User Document in MongoDB
   └─→ Send OTP Email (NodeMailer)

2. OTP Sent to Email
   ├─→ Generate 6-digit OTP
   ├─→ Hash OTP with Argon2
   ├─→ Store in Redis (2 min TTL)
   ├─→ Send HTML Email Template
   └─→ Increment attempt counter

3. Email Verification
   ├─→ Receive OTP from email
   ├─→ Submit OTP
   ├─→ Retrieve hashed OTP from Redis
   ├─→ Compare with submitted OTP
   └─→ Mark email as confirmed

4. Account Activation
   └─→ User can now login
```

### **Authentication & Token Flow**

```
1. User Login
   ├─→ Validate Credentials
   ├─→ Check Email Confirmation
   ├─→ Verify Password Hash
   ├─→ Rate Limit Check (Redis)
   └─→ Generate Tokens

2. Token Generation
   ├─→ Create Access Token (JWT)
   │   └─→ Expires in 7 days
   ├─→ Create Refresh Token (JWT)
   │   └─→ Expires in 30 days
   ├─→ Store Tokens in Redis
   └─→ Return to Client

3. Protected Endpoint Access
   ├─→ Client sends Access Token in header
   ├─→ Middleware verifies JWT
   ├─→ Check token in Redis cache
   ├─→ Extract user data
   └─→ Allow/Deny access

4. Token Rotation (Refresh)
   ├─→ Client sends Refresh Token
   ├─→ Verify Refresh Token
   ├─→ Generate new Access Token
   └─→ Update Redis cache
```

### **Message Sending Flow**

```
1. Send Message
   ├─→ Validate receiver exists
   ├─→ Validate message content
   ├─→ Handle file upload (if any)
   │   ├─→ Validate file type/size
   │   ├─→ Save to local storage
   │   └─→ Generate file URL
   ├─→ Determine if anonymous
   ├─→ Store in MongoDB
   └─→ Send notification email

2. Message Storage
   ├─→ receiverId (who gets the message)
   ├─→ senderId (null if anonymous)
   ├─→ messageText (encrypted)
   ├─→ attachment URL
   ├─→ isAnonymous flag
   └─→ timestamps

3. Notification
   ├─→ Trigger email event
   ├─→ Load email template
   ├─→ Send via NodeMailer
   └─→ Log delivery status
```

### **Password Reset Flow (OTP)**

```
1. Request Password Reset
   ├─→ Check email exists
   ├─→ Rate limit check (360 seconds)
   └─→ Send OTP email

2. Verify OTP
   ├─→ Retrieve hashed OTP from Redis
   ├─→ Compare with submitted OTP
   └─→ Confirm OTP valid

3. Reset Password
   ├─→ Hash new password
   ├─→ Update user document
   ├─→ Set changeCredentialsTime
   ├─→ Clear OTP from Redis
   └─→ Invalidate old tokens
```

---

## 📖 How to Use

### **For Frontend Developers**

#### 1. **User Registration**
```javascript
// 1. Sign up user
const response = await fetch('http://localhost:3000/auth/signup', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    firstName: 'John',
    lastName: 'Doe',
    email: 'john@example.com',
    password: 'SecurePass123!',
    phone: '+1234567890',
    two_step_verefication: true
  })
});

// 2. User receives OTP in email
// 3. Submit OTP for verification
const verifyResponse = await fetch('http://localhost:3000/auth/confirmEmail', {
  method: 'PATCH',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'john@example.com',
    otp: '123456'
  })
});
```

#### 2. **User Login**
```javascript
const loginResponse = await fetch('http://localhost:3000/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'john@example.com',
    password: 'SecurePass123!'
  })
});

const { data } = await loginResponse.json();
localStorage.setItem('accessToken', data.accessToken);
localStorage.setItem('refreshToken', data.refreshToken);
```

#### 3. **Get User Profile**
```javascript
const profileResponse = await fetch('http://localhost:3000/user/', {
  method: 'GET',
  headers: {
    'Authorization': `Bearer ${localStorage.getItem('accessToken')}`
  }
});

const { data } = await profileResponse.json();
console.log(data);
```

#### 4. **Send Anonymous Message**
```javascript
const formData = new FormData();
formData.append('messageText', 'Great feedback!');
formData.append('file', fileInput.files[0]);
formData.append('isAnonymous', 'true');

const messageResponse = await fetch(
  'http://localhost:3000/message/:receiverId',
  {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${localStorage.getItem('accessToken')}`
    },
    body: formData
  }
);
```

#### 5. **Share Profile & Receive Messages**
```javascript
// Get public profile link
const profileUrl = `http://your-frontend.com/profile/user-id`;

// Share with others who can send messages without logging in
// Or they can login to send authenticated messages
```

#### 6. **Retrieve Messages**
```javascript
const messagesResponse = await fetch('http://localhost:3000/message/list', {
  method: 'GET',
  headers: {
    'Authorization': `Bearer ${localStorage.getItem('accessToken')}`
  }
});

const { data } = await messagesResponse.json();
data.forEach(msg => {
  console.log(msg.messageText);
  console.log(msg.isAnonymous ? 'Anonymous' : msg.senderId);
});
```

---

## 💾 Database Models

### **User Model**

```javascript
{
  _id: ObjectId,
  firstName: String (2-25 chars),
  lastName: String (2-25 chars),
  email: String (unique, lowercase),
  password: String (hashed, required for system provider),
  age: Number,
  phone: String (encrypted),
  gender: Number (enum),
  role: Number (enum: user=0, admin=1),
  visited: Number (default: 0),
  provider: Number (enum: system=0, google=1),
  profilePicture: String (URL),
  coverProfilePicture: [String] (URL array),
  gallery: [String] (URL array),
  confirmEmail: Date (null if not confirmed),
  changeCredentialsTime: Date,
  otp: String,
  otpExpiresAt: Date,
  activated: Boolean (default: false),
  createdAt: Date,
  updatedAt: Date,
  
  // Virtual field
  username: String (firstName + lastName)
}
```

### **Message Model**

```javascript
{
  _id: ObjectId,
  receiverId: ObjectId (ref: User),
  senderId: ObjectId (ref: User, nullable for anonymous),
  messageText: String (encrypted),
  attachment: String (file URL),
  isAnonymous: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

### **Token Model**

```javascript
{
  _id: ObjectId,
  userId: ObjectId (ref: User),
  accessToken: String (JWT),
  refreshToken: String (JWT),
  expiresAt: Date,
  createdAt: Date
}
```

---

## 🔒 Security Features

### **1. Password Security**
- ✅ Dual hashing with Bcrypt (default) and Argon2 (backup)
- ✅ Salting (10 rounds by default)
- ✅ Never stored in plain text
- ✅ Password change invalidates old tokens

### **2. JWT Token Security**
- ✅ Separate secrets for user and admin tokens
- ✅ Refresh token rotation
- ✅ Token expiration (7 days for access)
- ✅ Token storage in Redis for validation
- ✅ Token blacklisting on logout

### **3. Data Encryption**
- ✅ Sensitive data encryption (phone numbers)
- ✅ Message content can be encrypted
- ✅ AES encryption for data at rest

### **4. API Security**
- ✅ CORS protection with whitelist
- ✅ Helmet.js for secure HTTP headers
- ✅ Rate limiting on authentication endpoints
- ✅ Request validation with Joi
- ✅ XSS protection
- ✅ CSRF protection ready

### **5. Database Security**
- ✅ Mongoose schema validation
- ✅ Input sanitization
- ✅ SQL injection prevention (NoSQL)
- ✅ Unique email constraint

### **6. Email Security**
- ✅ OTP-based verification
- ✅ OTP expiration (2 minutes)
- ✅ Max 3 OTP attempts before blocking
- ✅ Rate limiting on OTP requests

### **7. File Upload Security**
- ✅ File type validation
- ✅ File size limits
- ✅ Sanitized file names
- ✅ Stored outside web root

### **8. Rate Limiting**
- ✅ 3 requests per 2 minutes per endpoint
- ✅ Login attempts: max 5 in 10 minutes
- ✅ OTP attempts: max 3 in 6 minutes
- ✅ Automatic IP-based blocking

---

## ⚠️ Error Handling

### **Error Response Format**
```json
{
  "error_message": "Descriptive error message",
  "extra": {
    "status": 400,
    "details": "Additional error context"
  },
  "stack": "Full stack trace (development only)"
}
```

### **Common Error Codes**

| Code | Status | Meaning |
|------|--------|---------|
| 400 | Bad Request | Invalid input/validation error |
| 401 | Unauthorized | Missing/invalid authentication |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate entry/conflict |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Server Error | Unexpected server error |

### **Error Handling Middleware**
```javascript
app.use((error, req, res, next) => {
  const status = error.cause?.status ?? 500;
  return res.status(status).json({
    error_message: status == 500 ? "Something went wrong" : error.message,
    extra: error?.cause?.extra,
    stack: NODE_ENV == "development" ? error.stack : undefined
  });
});
```

---

## 🔧 Development Guidelines

### **Project Structure Principles**

1. **Modular Architecture**: Each feature is isolated in its own module
2. **Single Responsibility**: Each file has one clear purpose
3. **Separation of Concerns**: Controller → Service → Repository pattern
4. **Reusable Utilities**: Common logic in shared utilities
5. **Environment-based Config**: Different settings for dev/prod

### **Coding Standards**

- Use ES6+ syntax (async/await, destructuring)
- Follow naming conventions (camelCase for vars, PascalCase for classes)
- Add JSDoc comments for complex functions
- Handle errors with try-catch
- Use meaningful variable names
- Keep functions small and focused

### **Testing Endpoints**

Use the included `index.html` or tools like:
- **Postman**: Import collection from repo
- **Insomnia**: REST client
- **cURL**: Command line
- **Thunder Client**: VS Code extension

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the **ISC License** - see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**ENG/Seif Mohamed**

- GitHub: [@seif-mohamed-kamal](https://github.com/seif-mohamed-kamal)
- Email: seifm9067@gmail.com

---

## 🆘 Support & Issues

For issues, questions, or suggestions:

1. Check [existing issues](https://github.com/seif-mohamed-kamal/Honestify/issues)
2. Create a [new issue](https://github.com/seif-mohamed-kamal/Honestify/issues/new)
3. Include detailed description and reproduction steps
4. Attach relevant logs or screenshots

---

## 📚 Additional Resources

- [Express.js Documentation](https://expressjs.com/)
- [MongoDB Documentation](https://docs.mongodb.com/)
- [Mongoose ODM](https://mongoosejs.com/)
- [JWT.io](https://jwt.io/)
- [Redis Documentation](https://redis.io/docs/)
- [Nodemailer Guide](https://nodemailer.com/about/)

---

## 🎯 Roadmap

- [ ] Add WebSocket support for real-time notifications
- [ ] Implement message scheduling
- [ ] Add message reaction system (emoji reactions)
- [ ] Implement user analytics dashboard
- [ ] Add multi-language support
- [ ] Create mobile app companion
- [ ] Implement blockchain-based message verification
- [ ] Add AI-powered content moderation

---

**Last Updated**: September 2026  
**Version**: 1.0.0  
**Status**: Active Development ✅
