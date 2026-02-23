API Guide – Social Platform API V2

Overview
- Base URL placeholder: `{{base_url}}` (local dev defaults to `http://localhost:8000`).
- Auth flow: Register → Verify OTP → Login → Use Bearer token for protected routes → Delete account.
- All request/response bodies are JSON; requests below use `multipart/form-data` to match the current controllers.

Common Headers
- `Accept: application/json`
- For form-data posts, Postman will set `Content-Type: multipart/form-data` automatically.
- Protected routes: `Authorization: Bearer <token>`

1) Register
- Method/Path: `POST {{base_url}}/api/auth/register`
- Body (form-data): `username`, `first_name`, `last_name`, `email`, `password`, `password_confirmation`
- Success response 200:
  ```json
  {
    "status": "success",
    "status_code": 200,
    "message": "Registered successfully. OTP sent to email."
  }
  ```
- Notes: Use a unique `username` and `email`. Password and confirmation must match.

2) Verify OTP
- Method/Path: `POST {{base_url}}/api/auth/verify-otp`
- Body (form-data): `email`, `otp_code`
- Success response 200:
  ```json
  {
    "status": "success",
    "status_code": 200,
    "message": "Account verified successfully."
  }
  ```

3) Login
- Method/Path: `POST {{base_url}}/api/auth/login`
- Body (form-data): `email`, `password`
- Success response 200 (example):
  ```json
  {
    "status": "success",
    "status_code": 200,
    "data": {
      "id": 7,
      "first_name": "Somonor",
      "last_name": "Hong",
      "token": "11|7vYA0F8fqY7D88l9W2kQSH5TDy7WRMVz1stHlh9C46d620f6"
    }
  }
  ```
- Keep the `token`; it is required for any protected endpoint (e.g., delete account).

4) Delete Account
- Method/Path: `DELETE {{base_url}}/api/delete-account`
- Auth: `Authorization: Bearer <token>` (from Login).
- Success response 200:
  ```json
  {
    "status": "success",
    "message": "Account will be deleted after 5 minutes."
  }
  ```
- Notes: No body required. The account deletion is delayed by 5 minutes.

Quick cURL Samples
- Register:
  ```bash
  curl -X POST "{{base_url}}/api/auth/register" \
    -F "username=hsomonor" \
    -F "first_name=Somonor" \
    -F "last_name=Hong" \
    -F "email=hsomonor@gmail.com" \
    -F "password=Password123@" \
    -F "password_confirmation=Password123@"
  ```
- Verify OTP:
  ```bash
  curl -X POST "{{base_url}}/api/auth/verify-otp" \
    -F "email=hsomonor@gmail.com" \
    -F "otp_code=437013"
  ```
- Login:
  ```bash
  curl -X POST "{{base_url}}/api/auth/login" \
    -F "email=hsomonor@gmail.com" \
    -F "password=Password123@"
  ```
- Delete account:
  ```bash
  curl -X DELETE "{{base_url}}/api/delete-account" \
    -H "Authorization: Bearer <token>"
  ```

Testing Order
- Step 1: Register with new email.
- Step 2: Retrieve OTP from email inbox and call Verify OTP.
- Step 3: Login to obtain Bearer token.
- Step 4: Call protected routes (e.g., Delete Account) with the token.

Troubleshooting
- 400/422: Check for missing fields or mismatched password confirmation.
- 401: Missing/expired Bearer token; log in again.
- 409: Email or username already exists; register with different values.
