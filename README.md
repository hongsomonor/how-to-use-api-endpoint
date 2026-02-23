API Guide – Social Platform API V2

Overview
- Base URL: `https://social-api.hushstackcambodia.site`
- Auth flow: Register → Verify OTP (or Resend OTP if needed) → Login → Use Bearer token for protected routes.
- Requests use `multipart/form-data` when files or form values are posted; responses are JSON.

Common Headers
- `Accept: application/json`
- `Authorization: Bearer <token>` for protected routes (token from Login).

Auth Endpoints
1) Register – `POST https://social-api.hushstackcambodia.site/api/auth/register`
- Body (form-data): `username`, `first_name`, `last_name`, `email`, `password`, `password_confirmation`
- Success 200:
  ```json
  {"status":"success","status_code":200,"message":"Registered successfully. OTP sent to email."}
  ```

2) Resend OTP – `POST https://social-api.hushstackcambodia.site/api/auth/resend-otp`
- Body (form-data): `email`
- Success 200:
  ```json
  {"status":"success","message":"OTP resent successfully."}
  ```

3) Verify OTP – `POST https://social-api.hushstackcambodia.site/api/auth/verify-otp`
- Body (form-data): `email`, `otp_code`
- Success 200:
  ```json
  {"status":"success","status_code":200,"message":"Account verified successfully."}
  ```

4) Login – `POST https://social-api.hushstackcambodia.site/api/auth/login`
- Body (form-data): `email`, `password`
- Success 200 (example):
  ```json
  {
    "status":"success",
    "status_code":200,
    "data":{
      "id":7,
      "first_name":"Somonor",
      "last_name":"Hong",
      "token":"11|7vYA0F8fqY7D88l9W2kQSH5TDy7WRMVz1stHlh9C46d620f6"
    }
  }
  ```

5) Delete Account – `DELETE https://social-api.hushstackcambodia.site/api/delete-account`
- Auth required. No body.
- Success 200:
  ```json
  {"status":"success","message":"Account will be deleted after 5 minutes."}
  ```

Profile Endpoints
6) View Own Profile – `GET https://social-api.hushstackcambodia.site/api/profile/me`
- Auth required. No body.
- Returns profile fields and counters.

7) Update Profile – `POST https://social-api.hushstackcambodia.site/api/profile/update`
- Auth required. Body (form-data): `username`, `first_name`, `last_name`, `picture` (file), `cover` (file), `bio`, `note`, `birth_of_date`, `age`, `nationality_id`, `contact_url`, `address`
- Updates profile and media.

Social/Post Endpoints
8) Create Post – `POST https://social-api.hushstackcambodia.site/api/posts`
- Auth required. Body (form-data): `caption`, `picture` (file), optional `is_onlyme`
- Success 200 returns created post with image URL and user.

9) Own Posts (photos) – `GET https://social-api.hushstackcambodia.site/api/profile/posts/photos`
- Auth required. Lists user’s post images.

10) Home Feed – `GET https://social-api.hushstackcambodia.site/api/home-feed`
- Auth required. Shows all user posts with user info, picture URL, time, likes/comments counts.

11) Toggle Like – `POST https://social-api.hushstackcambodia.site/api/posts/{post_id}/like-toggle`
- Auth required. No body.
- Success 200: `{"success":true,"liked":true,"likes_count":1}`

12) Notifications – `GET https://social-api.hushstackcambodia.site/api/notifications`
- Auth required. Lists notifications with actor, post, type, read status, and timestamps.

Quick cURL Samples
- Register:
  ```bash
  curl -X POST "https://social-api.hushstackcambodia.site/api/auth/register" \
    -F "username=hsomonor" \
    -F "first_name=Somonor" \
    -F "last_name=Hong" \
    -F "email=hsomonor@gmail.com" \
    -F "password=Password123@" \
    -F "password_confirmation=Password123@"
  ```
- Verify OTP:
  ```bash
  curl -X POST "https://social-api.hushstackcambodia.site/api/auth/verify-otp" \
    -F "email=hsomonor@gmail.com" \
    -F "otp_code=437013"
  ```
- Login:
  ```bash
  curl -X POST "https://social-api.hushstackcambodia.site/api/auth/login" \
    -F "email=hsomonor@gmail.com" \
    -F "password=Password123@"
  ```
- Delete account:
  ```bash
  curl -X DELETE "https://social-api.hushstackcambodia.site/api/delete-account" \
    -H "Authorization: Bearer <token>"
  ```

Testing Order
- Register with new email → Verify OTP (or Resend OTP if needed) → Login → Test profile and social endpoints with the Bearer token.

Troubleshooting
- 400/422: Missing fields or invalid validation (e.g., passwords mismatch).
- 401: Missing/expired Bearer token; log in again.
- 409: Email or username already exists; register with different values.
