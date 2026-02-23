# Social Platform App - API Documentation

## Table of Contents
- [Overview](#overview)
- [Getting Started](#getting-started)
- [Authentication](#authentication)
- [API Endpoints](#api-endpoints)
- [How to Test the API](#how-to-test-the-api)
- [Testing Tools](#testing-tools)
- [Request Examples](#request-examples)

---

## Overview

This document provides comprehensive guidance on using and testing the **Social Platform App API**. The API allows users to register, log in, manage their profiles, create posts, and interact with other users.

**Base URL:** `https://social-api.hushstackcambodia.site`

---

## Getting Started

### Prerequisites
- A REST client tool (Postman, Insomnia, Thunder Client, or cURL)
- A valid email address for testing
- Access to this API documentation

### API Features
- User Authentication (Register, Login, OTP Verification)
- User Profile Management
- Post Management (Create, View, Like)
- Social Feed & Notifications
- Account Management

---

## Authentication

### 1. Register User
Create a new user account with your email.

**Endpoint:** `POST /api/auth/register`

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "YourPassword123",
  "username": "your_username",
  "first_name": "John",
  "last_name": "Doe"
}
```

**Response (Success):**
```json
{
  "status": "success",
  "message": "Registration successful",
  "user_id": "12345",
  "email": "user@example.com"
}
```

---

### 2. Login
Authenticate user with email and password.

**Endpoint:** `POST /api/auth/login`

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "YourPassword123"
}
```

**Response (Success):**
```json
{
  "status": "success",
  "message": "Login successful",
  "token": "your_jwt_token_here",
  "user_id": "12345",
  "email": "user@example.com"
}
```

---

### 3. OTP Verification
Verify One-Time Password sent to email.

**Endpoint:** `POST /api/auth/otp-verification`

**Request Body:**
```json
{
  "email": "user@example.com",
  "otp_code": "123456"
}
```

**Response (Success):**
```json
{
  "status": "success",
  "message": "OTP verified successfully",
  "verified": true
}
```

---

### 4. OTP Resend
Request a new OTP code.

**Endpoint:** `POST /api/auth/otp-resend`

**Request Body:**
```json
{
  "email": "user@example.com"
}
```

**Response (Success):**
```json
{
  "status": "success",
  "message": "OTP sent to your email"
}
```

---

## API Endpoints

### User Profile Management

#### View Own Profile
**Endpoint:** `GET /api/users/profile`

**Headers Required:**
```
Authorization: Bearer YOUR_JWT_TOKEN
```

---

#### Update Profile
**Endpoint:** `PUT /api/users/profile`

**Headers Required:**
```
Authorization: Bearer YOUR_JWT_TOKEN
```

**Request Body:**
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "bio": "I love coding",
  "profile_picture": "profile_pic_url"
}
```

---

### Post Management

#### Create Post
**Endpoint:** `POST /api/posts/create`

**Headers Required:**
```
Authorization: Bearer YOUR_JWT_TOKEN
```

**Request Body:**
```json
{
  "content": "This is my first post!",
  "image_url": "optional_image_url",
  "visibility": "public"
}
```

---

#### View Own Posts
**Endpoint:** `GET /api/posts/my-posts`

**Headers Required:**
```
Authorization: Bearer YOUR_JWT_TOKEN
```

---

#### Home Feed
**Endpoint:** `GET /api/posts/feed`

**Headers Required:**
```
Authorization: Bearer YOUR_JWT_TOKEN
```

---

#### Toggle Like on Post
**Endpoint:** `POST /api/posts/{post_id}/like`

**Headers Required:**
```
Authorization: Bearer YOUR_JWT_TOKEN
```

---

### Notifications

#### View Notifications
**Endpoint:** `GET /api/notifications`

**Headers Required:**
```
Authorization: Bearer YOUR_JWT_TOKEN
```

---

### Account Management

#### Logout
**Endpoint:** `POST /api/auth/logout`

**Headers Required:**
```
Authorization: Bearer YOUR_JWT_TOKEN
```

---

#### Delete Account
**Endpoint:** `DELETE /api/users/account`

**Headers Required:**
```
Authorization: Bearer YOUR_JWT_TOKEN
```

---

## How to Test the API

### Step 1: Register a New Account
1. Use the **Register** endpoint to create a test account
2. Use your own email address so you can receive OTP codes
3. Save the user credentials for testing

### Step 2: Login and Get Token
1. Use the **Login** endpoint with your registered email and password
2. Copy the **JWT token** from the response
3. Use this token for all authenticated requests

### Step 3: Test Authenticated Endpoints
1. Add the token to the Authorization header: `Bearer YOUR_JWT_TOKEN`
2. Test profile, post, and notification endpoints

### Step 4: Test OTP Flow (if applicable)
1. Request OTP using **OTP Resend** endpoint
2. Check your email for the OTP code
3. Verify using **OTP Verification** endpoint

---

## Testing Tools

### Option 1: Using Postman
1. Download and install **Postman** from https://www.postman.com/downloads/
2. Create a new workspace
3. Set up collection with API endpoints
4. In the **Authorization** tab, select "Bearer Token"
5. Paste your JWT token
6. Send requests

### Option 2: Using cURL (Command Line)
```bash
# Register
curl -X POST https://social-api.hushstackcambodia.site/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"YourPassword123","username":"testuser"}'

# Login
curl -X POST https://social-api.hushstackcambodia.site/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"YourPassword123"}'

# Get Profile (replace TOKEN with actual JWT)
curl -X GET https://social-api.hushstackcambodia.site/api/users/profile \
  -H "Authorization: Bearer TOKEN"
```

### Option 3: Using Insomnia
1. Download **Insomnia** from https://insomnia.rest/
2. Create new HTTP requests
3. Configure Bearer token authentication
4. Send requests and view responses

### Option 4: Using Thunder Client (VS Code Extension)
1. Install Thunder Client extension in VS Code
2. Click Thunder Client icon in sidebar
3. Create new requests
4. Add authorization headers
5. Send and test

---

## Request Examples

### Complete Testing Workflow

**1. Register:**
```
POST https://social-api.hushstackcambodia.site/api/auth/register

{
  "email": "testuser@gmail.com",
  "password": "TestPass123!",
  "username": "testuser",
  "first_name": "Test",
  "last_name": "User"
}
```

**2. Login:**
```
POST https://social-api.hushstackcambodia.site/api/auth/login

{
  "email": "testuser@gmail.com",
  "password": "TestPass123!"
}
```
*Response will contain JWT token*

**3. Get Profile:**
```
GET https://social-api.hushstackcambodia.site/api/users/profile

Headers:
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**4. Update Profile:**
```
PUT https://social-api.hushstackcambodia.site/api/users/profile

Headers:
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

{
  "first_name": "Test",
  "bio": "I am testing the API"
}
```

**5. Create Post:**
```
POST https://social-api.hushstackcambodia.site/api/posts/create

Headers:
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

{
  "content": "Hello, Social Platform!",
  "visibility": "public"
}
```

**6. Get Feed:**
```
GET https://social-api.hushstackcambodia.site/api/posts/feed

Headers:
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| 401 Unauthorized | Token expired or invalid. Re-login to get a new token |
| 400 Bad Request | Check request body format and required fields |
| 404 Not Found | Verify endpoint URL is correct |
| 500 Server Error | Server issue, try again later |
| CORS Error | Ensure proper headers are set |

---

## Notes

- All API requests must use **HTTPS**
- JWT tokens have expiration times, re-login if expired
- Always include `Content-Type: application/json` header
- OTP codes are typically valid for 10 minutes
- Passwords must meet security requirements (minimum 8 characters recommended)

---

## Support

For API issues or questions, refer to the original Coda documentation:
https://coda.io/d/API-Endpoint-of-Social-Platform-App_dAPYG9tYwDw

---

**Last Updated:** February 23, 2026
**Status:** Active & Ready for Testing
