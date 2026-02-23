# Social Platform App - API Documentation

## Table of Contents
- [Overview](#overview)
- [Quickstart](#quickstart)
- [Getting Started](#getting-started)
- [Authentication](#authentication)
- [API Endpoints](#api-endpoints)
- [How to Test the API](#how-to-test-the-api)
- [Testing Tools](#testing-tools)
- [Testing Checklist](#testing-checklist)
- [Request Examples](#request-examples)
- [Common Error Codes](#common-error-codes)
- [Coda Reference](#coda-reference)
- [Support](#support)

---

## Overview

This document provides comprehensive guidance on using and testing the **Social Platform App API**. The API allows users to register, log in, manage their profiles, create posts, and interact with other users.

**Base URL:** `https://social-api.hushstackcambodia.site`

---

## Quickstart

1. Register a user with your email.
2. Log in and copy the returned Sanctum personal access `token`.
3. Add header `Authorization: Bearer <token>` for every protected request.
4. Create a post, view feed, like/unlike a post.
5. Log out when done testing.

Use any HTTP client (Postman, curl, Insomnia, Thunder Client). Ready-made curls are in [Request Examples](#request-examples).

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

**Request Body (form-data):**
| Key                    | Type | Value              |
|------------------------|------|--------------------|
| first_name             | Text | John               |
| last_name              | Text | Doe                |
| email                  | Text | user@example.com   |
| password               | Text | YourPassword123    |
| password_confirmation  | Text | YourPassword123    |

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
Authenticate user with email and password. Returns a Sanctum personal access token.

**Endpoint:** `POST /api/auth/login`

**Request Body (form-data):**
| Key       | Type | Value               |
|-----------|------|---------------------|
| email     | Text | user@example.com    |
| password  | Text | YourPassword123     |

**Response (Success):**
```json
{
  "status": "success",
  "message": "Login successful",
  "token": "your_sanctum_token_here",
  "user_id": "12345",
  "email": "user@example.com"
}
```

---

### 3. OTP Verification
Verify One-Time Password sent to email.

**Endpoint:** `POST /api/auth/otp-verification`

**Request Body (form-data):**
| Key      | Type | Value            |
|----------|------|------------------|
| email    | Text | user@example.com |
| otp_code | Text | 123456           |

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

**Request Body (form-data):**
| Key   | Type | Value            |
|-------|------|------------------|
| email | Text | user@example.com |

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
Authorization: Bearer YOUR_SANCTUM_TOKEN
```

---

#### Update Profile
**Endpoint:** `PUT /api/users/profile`

**Headers Required:**
```
Authorization: Bearer YOUR_SANCTUM_TOKEN
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
**Endpoint:** `POST /api/posts`

**Headers Required:**
```
Authorization: Bearer YOUR_SANCTUM_TOKEN
```

**Request Body (form-data or JSON):**
```bash
# form-data fields
content=This is my first post!
picture=@/path/to/image.png   # optional file
visibility=public             # optional, defaults to public
```

---

#### View Own Posts
**Endpoint:** `GET /api/posts/my-posts`

**Headers Required:**
```
Authorization: Bearer YOUR_SANCTUM_TOKEN
```

---

#### Home Feed
**Endpoint:** `GET /api/home-feed`

**Headers Required:**
```
Authorization: Bearer YOUR_SANCTUM_TOKEN
```

---

#### Toggle Like on Post
**Endpoint:** `POST /api/posts/{post_id}/like`

**Headers Required:**
```
Authorization: Bearer YOUR_SANCTUM_TOKEN
```

---

### Notifications

#### View Notifications
**Endpoint:** `GET /api/notifications`

**Headers Required:**
```
Authorization: Bearer YOUR_SANCTUM_TOKEN
```

---

### Account Management

#### Logout
**Endpoint:** `POST /api/auth/logout`

**Headers Required:**
```
Authorization: Bearer YOUR_SANCTUM_TOKEN
```

---

#### Delete Account
**Endpoint:** `DELETE /api/users/account`

**Headers Required:**
```
Authorization: Bearer YOUR_SANCTUM_TOKEN
```

---

## How to Test the API

### Step 1: Register a New Account
1. Use the **Register** endpoint to create a test account
2. Use your own email address so you can receive OTP codes
3. Save the user credentials for testing

### Step 2: Login and Get Token (Sanctum)
1. Use the **Login** endpoint with your registered email and password
2. Copy the **Sanctum token** from the response
3. Use this token for all authenticated requests

### Step 3: Test Authenticated Endpoints
1. Add the token to the Authorization header: `Bearer YOUR_SANCTUM_TOKEN`
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
5. Paste your Sanctum token
6. Send requests

### Option 2: Using cURL (Command Line)
```bash
# Register (form-data)
curl -X POST https://social-api.hushstackcambodia.site/api/auth/register \
  -H "Accept: application/json" \
  -F first_name=John \
  -F last_name=Doe \
  -F email=user@example.com \
  -F password=YourPassword123 \
  -F password_confirmation=YourPassword123

# Login (form-data)
curl -X POST https://social-api.hushstackcambodia.site/api/auth/login \
  -H "Accept: application/json" \
  -F email=user@example.com \
  -F password=YourPassword123

# Get Profile (replace TOKEN with actual Sanctum token)
curl -X GET https://social-api.hushstackcambodia.site/api/users/profile \
  -H "Authorization: Bearer TOKEN" \
  -H "Accept: application/json"
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

## Testing Checklist

- Register a new user and receive OTP email.
- OTP verification succeeds; subsequent login works.
- Sanctum token is returned and accepted in protected endpoints.
- View and update profile; fields persist.
- Create a post; it appears in **My Posts** and **Feed**.
- Like/unlike a post and see the like count change.
- Notifications endpoint returns recent events.
- Logout revokes the current token; calling a protected route without a fresh token returns 401.
- Delete account; login now fails.

---

## Request Examples

### Complete Testing Workflow

**1. Register (form-data):**
```
POST https://social-api.hushstackcambodia.site/api/auth/register
first_name=Test
last_name=User
email=testuser@gmail.com
password=TestPass123!
password_confirmation=TestPass123!
```

**2. Login (form-data):**
```
POST https://social-api.hushstackcambodia.site/api/auth/login
email=testuser@gmail.com
password=TestPass123!
```
*Response will contain Sanctum token*

**3. Verify OTP (form-data, if required):**
```
POST https://social-api.hushstackcambodia.site/api/auth/otp-verification
email=testuser@gmail.com
otp_code=123456
```

**4. Get Profile:**
```
GET https://social-api.hushstackcambodia.site/api/users/profile
Headers:
Authorization: Bearer YOUR_SANCTUM_TOKEN
Accept: application/json
```

**5. Update Profile (multipart):**
```
POST https://social-api.hushstackcambodia.site/api/profile/update
Headers:
Authorization: Bearer YOUR_SANCTUM_TOKEN

form-data fields (only send what you change):
first_name=Test
bio=I am testing the API
picture=@/path/to/avatar.jpg   # optional file
cover=@/path/to/cover.jpg      # optional file
```

**6. Create Post (multipart):**
```
POST https://social-api.hushstackcambodia.site/api/posts
Headers:
Authorization: Bearer YOUR_SANCTUM_TOKEN

form-data:
caption=Hello, Social Platform!
picture=@/path/to/image.png    # optional
```

**7. Get Feed:**
```
GET https://social-api.hushstackcambodia.site/api/home-feed
Headers:
Authorization: Bearer YOUR_SANCTUM_TOKEN
Accept: application/json
```

---

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| 401 Unauthorized | Token missing, revoked, or expired. Re-login or create a new token |
| 400 Bad Request | Check request body format and required fields |
| 404 Not Found | Verify endpoint URL is correct |
| 500 Server Error | Server issue, try again later |
| CORS Error | Ensure proper headers are set |

---

## Common Error Codes

| HTTP | Meaning | Typical Fix |
|------|---------|-------------|
| 400 Bad Request | Missing/invalid payload fields | Re-check required JSON keys and types |
| 401 Unauthorized | No token or expired/invalid token | Log in again and replace the Bearer token |
| 403 Forbidden | Token valid but action not allowed | Ensure the user owns the resource or has permission |
| 404 Not Found | Resource or route not found | Confirm URL and identifiers (e.g., `post_id`) |
| 429 Too Many Requests | Rate limit hit | Wait and retry with backoff |
| 500 Server Error | Unexpected server issue | Retry later; report if persistent |

---

## Notes

- All API requests must use **HTTPS**
- Sanctum tokens can be revoked; if you receive 401, log in again or create a new token
- Always include `Content-Type: application/json` header
- OTP codes are typically valid for 10 minutes
- Passwords must meet security requirements (minimum 8 characters recommended)

---

## Coda Reference

The Coda doc is the source of truth for full API specs, field definitions, and changelog.

- Link: https://coda.io/d/API-Endpoint-of-Social-Platform-App_dAPYG9tYwDw
- Sign in if prompted, then reopen the link.
- Click “Copy doc” to clone it into your Coda workspace if you want to edit.
- If you see an access screen, choose “Request access” to notify the doc owner.

---

## Support

For API issues or questions, refer to the original Coda documentation:
https://coda.io/d/API-Endpoint-of-Social-Platform-App_dAPYG9tYwDw

---

**Last Updated:** February 23, 2026  
**Status:** Active & Ready for Testing

