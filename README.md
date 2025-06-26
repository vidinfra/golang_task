# ⚙️ Task: Golang Auth API with Gin, Uber Fx, JWT, Redis & Org Switch

You're tasked with building a **secure, modular authentication and organization context API** using **Golang** and tools like **Gin**, **Uber Fx**, **Redis**, and **JWT**. Your goal is to demonstrate solid backend engineering with dependency injection, token management, and rate limiting.

---

## 🔗 Resources

* **Seed Users/Orgs**: Must be created via hardcoded fixtures in code
* **Reference Stack**: Golang, Gin, Redis, Uber Fx, JWT, Viper
* **Sample JWT Claims**:

  ```json
  {
    "user_id": "uuid",
    "org_id": "uuid",
    "exp": 1712345678
  }
  ```

---

## 🛠 Tech Stack

Use the following tools:

* ✅ **Golang**
* ✅ **Gin**
* ✅ **Uber Fx**
* ✅ **Redis**
* ✅ **JWT** (access & refresh)
* ✅ **bcrypt**
* ✅ **stretchr/testify** (unit testing)
* ✅ **Viper** (for configuration management)

---

## 📌 API Endpoints

### 1. `POST /login`

* Input: `email`, `password`
* On success:

  * Return `access_token` (JWT)
  * Return `refresh_token` (JWT or UUID)
  * Store refresh token in Redis with 7-day TTL
* Rate limit: **5 requests/min per IP**

---

### 2. `POST /refresh`

* Input: `refresh_token`
* On valid token:

  * Issue a new access token
  * (Bonus: rotate refresh token)

---

### 3. `POST /logout`

* Input: refresh token (or in header)
* Deletes refresh token from Redis

---

### 4. `GET /me`

* Authenticated route
* Requires valid access token
* Returns:

```jsonjson
  {
    "user": { "id": "uuid", "name": "Sohel" },
    "current_org": { "id": "uuid", "name": "Tenbyte" },
    "orgs": [
      { "id": "uuid", "name": "Tenbyte" },
      { "id": "uuid", "name": "OpenResty" }
    ]
  }
```

---

### 5. `POST /orgs/switch`

* Input: `org_id`
* Authenticated route
* Switches active org for the user
* Save selected org in Redis or use in JWT claims

---

## 🧬 Seed Data (on App Start)

Seed 2 orgs + 2 users per org:

```go
// Sample Org
{
  ID: "uuid-1", Name: "Tenbyte"
}

// Sample User
{
  Name: "Sohel",
  Email: "sohel@tenbyte.com",
  Password: "hashed:123456",
  Orgs: [Tenbyte]
}
```

Use in-memory DB or a mock repository layer.

---

## 🔐 JWT Guidelines

* **Access Token**: short-lived (e.g., 15 min)
* **Refresh Token**: long-lived (e.g., 7 days)
* Store refresh tokens in Redis
* JWT claims must include `user_id` and `org_id`

---

## 🧪 Unit Tests

Write tests for:

* Login
* Refresh token flow
* Logout
* Org switch
* Middleware (auth, rate limit)

Use mocks where needed.

---

## 🔁 Git Instructions

* Use feature branches:

  * `feature/auth-login`
  * `feature/token-refresh`
  * `feature/org-switch`
* Follow conventional commits
* Push to a **public GitHub repo**
* Include:

  * `.env.example`
  * `README.md` with:

    * Setup instructions
    * API examples
    * Seed credentials

---

## 🧠 Evaluation Criteria

| Area               | Expectation                                 |
| ------------------ | ------------------------------------------- |
| **Code Quality**   | Idiomatic, clean, modular                   |
| **Token Handling** | Secure JWT with proper expiration and Redis |
| **Org Context**    | Can list and switch orgs securely           |
| **Rate Limiting**  | Working with Redis                          |
| **Testing**        | Clear and meaningful test coverage          |
| **Docs**           | Simple to run, clear examples               |
