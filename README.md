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
- Sample body
  ```bash
  username=hsomonor
  first_name=Somonor
  last_name=Hong
  email=hsomonor@gmail.com
  password=Password123@
  password_confirmation=Password123@
  ```
- Success 200
  ```json
  {
    "status":"success",
    "status_code":200,
    "message":"Registered successfully. OTP sent to email."
  }
  ```

2) Resend OTP – `POST https://social-api.hushstackcambodia.site/api/auth/resend-otp`
- Body (form-data): `email`
- Sample body
  ```bash
  email=hsomonor@gmail.com
  ```
- Success 200
  ```json
  {
    "status":"success",
    "message":"OTP resent successfully."
  }
  ```

3) Verify OTP – `POST https://social-api.hushstackcambodia.site/api/auth/verify-otp`
- Body (form-data): `email`, `otp_code`
- Sample body
  ```bash
  email=hsomonor@gmail.com
  otp_code=437013
  ```
- Success 200
  ```json
  {
    "status":"success",
    "status_code":200,
    "message":"Account verified successfully."
  }
  ```

4) Login – `POST https://social-api.hushstackcambodia.site/api/auth/login`
- Body (form-data): `email`, `password`
- Sample body
  ```bash
  email=hsomonor@gmail.com
  password=Password123@
  ```
- Success 200 (example)
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
- Success 200
  ```json
  {
    "status":"success",
    "message":"Account will be deleted after 5 minutes."
  }
  ```

Profile Endpoints
6) View Own Profile – `GET https://social-api.hushstackcambodia.site/api/profile/me`
- Auth required. No body.
- Returns profile fields and counters.
```json
{
  "status":"success",
  "status_code":200,
  "data":{
    "username":"hsomonor",
    "email":"hsomonor@gmail.com",
    "first_name":"Somonor",
    "last_name":"Hong",
    "picture":null,
    "cover":null,
    "bio":null,
    "note":null,
    "birth_of_date":null,
    "age":null,
    "nationality":null,
    "contact_url":null,
    "address":null,
    "followers_count":0,
    "following_count":0,
    "posts_count":0
  }
}
```

7) Update Profile – `POST https://social-api.hushstackcambodia.site/api/profile/update`
- Auth required. Body (form-data): `username`, `first_name`, `last_name`, `picture` (file), `cover` (file), `bio`, `note`, `birth_of_date`, `age`, `nationality_id`, `contact_url`, `address`
- Updates profile and media.
```bash
username=ceyzer_rarr
first_name=HH
last_name=LL
picture=@/path/pic.jpg
cover=@/path/cover.jpg
bio=Hello
note=Testing
birth_of_date=2000-01-01
age=25
nationality_id=1
contact_url=https://www.instagram.com/ceyzer_rarr/
address=Phnom Penh
```

Social/Post Endpoints
8) Create Post – `POST https://social-api.hushstackcambodia.site/api/posts`
- Auth required. Body (form-data): `caption`, `picture` (file), optional `is_onlyme`
- Success 200 returns created post with image URL and user.
```bash
caption=Welcome
picture=@/path/photo.jpg
```
```json
{
  "status":"success",
  "message":"Post created successfully",
  "data":{
    "id":3,
    "caption":"Welcome",
    "picture":"https://pub-bbc7c2fe34614a5794960788c8da82e1.r2.dev/posts/0e0bcd08-4b74-4cad-bb9e-ca35a8950029.jpg",
    "is_onlyme":false,
    "created_at":"2026-02-23T07:19:26.000000Z",
    "user":{
      "id":8,
      "username":"hsomonor",
      "picture":null
    }
  }
}
```

9) Own Posts (photos) – `GET https://social-api.hushstackcambodia.site/api/profile/posts/photos`
- Auth required. Lists user’s post images.
```json
{
  "status":"success",
  "data":[
    {"id":4,"picture":"https://pub-bbc7c2fe34614a5794960788c8da82e1.r2.dev/posts/5153db34-3c21-4d5e-9c1d-2621e103b767.jpg"},
    {"id":3,"picture":"https://pub-bbc7c2fe34614a5794960788c8da82e1.r2.dev/posts/0e0bcd08-4b74-4cad-bb9e-ca35a8950029.jpg"}
  ]
}
```

10) Home Feed – `GET https://social-api.hushstackcambodia.site/api/home-feed`
- Auth required. Shows all user posts with user info, picture URL, time, likes/comments counts.
```json
{
  "data":[
    {
      "id":4,
      "user":{"id":8,"first_name":"Somonor","last_name":"Hong","username":"hsomonor","picture":null},
      "caption":"Welcome",
      "picture":"https://pub-bbc7c2fe34614a5794960788c8da82e1.r2.dev/posts/5153db34-3c21-4d5e-9c1d-2621e103b767.jpg",
      "time":"2 minutes ago",
      "likes_count":0,
      "comments_count":0
    }
  ]
}
```

11) Toggle Like – `POST https://social-api.hushstackcambodia.site/api/posts/{post_id}/like-toggle`
- Auth required. No body.
- Success 200: `{"success":true,"liked":true,"likes_count":1}`
```json
{"success":true,"liked":true,"likes_count":1}
```

12) Notifications – `GET https://social-api.hushstackcambodia.site/api/notifications`
- Auth required. Lists notifications with actor, post, type, read status, and timestamps.
```json
{
  "status":"success",
  "data":[
    {
      "id":4,
      "type":"like",
      "actor":{"id":9,"username":"smn","first_name":"Somonor","last_name":"Hong","picture":null},
      "post":{"id":4,"picture":"https://pub-bbc7c2fe34614a5794960788c8da82e1.r2.dev/posts/5153db34-3c21-4d5e-9c1d-2621e103b767.jpg"},
      "is_following_actor":false,
      "is_read":false,
      "time":"34 seconds ago",
      "created_at":"2026-02-23T07:22:57.000000Z"
    }
  ]
}
```

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
- Resend OTP:
  ```bash
  curl -X POST "https://social-api.hushstackcambodia.site/api/auth/resend-otp" \
    -F "email=hsomonor@gmail.com"
  ```
- View own profile:
  ```bash
  curl -X GET "https://social-api.hushstackcambodia.site/api/profile/me" \
    -H "Authorization: Bearer <token>"
  ```
- Update profile (example without files):
  ```bash
  curl -X POST "https://social-api.hushstackcambodia.site/api/profile/update" \
    -H "Authorization: Bearer <token>" \
    -F "username=ceyzer_rarr" \
    -F "first_name=HH" \
    -F "last_name=LL" \
    -F "bio=Hello" \
    -F "note=Testing" \
    -F "birth_of_date=2000-01-01" \
    -F "age=25" \
    -F "nationality_id=1" \
    -F "contact_url=https://www.instagram.com/ceyzer_rarr/" \
    -F "address=Phnom Penh"
  # add -F "picture=@/path/to/pic.jpg" -F "cover=@/path/to/cover.jpg" to upload images
  ```
- Create post:
  ```bash
  curl -X POST "https://social-api.hushstackcambodia.site/api/posts" \
    -H "Authorization: Bearer <token>" \
    -F "caption=Welcome" \
    -F "picture=@/path/to/photo.jpg"
  ```
- Own posts (photos):
  ```bash
  curl -X GET "https://social-api.hushstackcambodia.site/api/profile/posts/photos" \
    -H "Authorization: Bearer <token>"
  ```
- Home feed:
  ```bash
  curl -X GET "https://social-api.hushstackcambodia.site/api/home-feed" \
    -H "Authorization: Bearer <token>"
  ```
- Toggle like (post id 4 as example):
  ```bash
  curl -X POST "https://social-api.hushstackcambodia.site/api/posts/4/like-toggle" \
    -H "Authorization: Bearer <token>"
  ```
- Notifications:
  ```bash
  curl -X GET "https://social-api.hushstackcambodia.site/api/notifications" \
    -H "Authorization: Bearer <token>"
  ```

Testing Order
- Register with new email → Verify OTP (or Resend OTP if needed) → Login → Test profile and social endpoints with the Bearer token.

Troubleshooting
- 400/422: Missing fields or invalid validation (e.g., passwords mismatch).
- 401: Missing/expired Bearer token; log in again.
- 409: Email or username already exists; register with different values.
