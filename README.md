# JWT Authentication API

FastAPI application implementing JWT (JSON Web Token) authentication with login and token refresh capabilities.

## Features

- ✅ JWT-based authentication
- ✅ Login endpoint with username/password
- ✅ Token refresh mechanism
- ✅ Protected routes demonstration
- ✅ Token expiration (300 seconds)
- ✅ Docker containerization
- ✅ Poetry dependency management
- ✅ Interactive API documentation (Swagger UI)

## Project Structure

```
.
├── backend/
│   ├── main.py              # FastAPI application
│   ├── pyproject.toml       # Poetry dependencies
│   └── Dockerfile           # Docker configuration
├── docker-compose.yml       # Docker Compose configuration
└── README.md               # This file
```

## Prerequisites

- Python 3.9+ (for local development)
- Docker and Docker Compose (for containerized deployment)
- Poetry (for local dependency management)

## Default Credentials

- **Username:** `admin`
- **Password:** `admin123`

## Installation & Usage

### Option 1: Using Docker (Recommended)

1. **Build and start the container:**
   ```bash
   docker-compose up --build
   ```

2. **Access the API:**
   - API Base URL: http://localhost:8000
   - Interactive Documentation: http://localhost:8000/docs
   - Alternative Documentation: http://localhost:8000/redoc

3. **Stop the container:**
   ```bash
   docker-compose down
   ```

### Option 2: Local Development

1. **Navigate to the backend directory:**
   ```bash
   cd backend
   ```

2. **Install dependencies using Poetry:**
   ```bash
   poetry install
   ```

3. **Run the application:**
   ```bash
   poetry run uvicorn main:app --reload --host 0.0.0.0 --port 8000
   ```

4. **Access the API:**
   - API Base URL: http://localhost:8000
   - Interactive Documentation: http://localhost:8000/docs

## API Endpoints

### 1. Root Endpoint
```http
GET /
```
Returns information about available endpoints.

**Response:**
```json
{
  "message": "JWT Authentication API",
  "endpoints": {
    "login": "/token",
    "refresh": "/refresh",
    "protected": "/protected",
    "docs": "/docs"
  }
}
```

### 2. Login (Get Token)
```http
POST /token
Content-Type: application/x-www-form-urlencoded

username=admin&******
```

**Response:**
```json
{
  "access_token": "******",
  "token_type": "bearer",
  "expires_in": 300
}
```

### 3. Refresh Token
```http
POST /refresh
Authorization: ******
```

**Response:**
```json
{
  "access_token": "******",
  "token_type": "bearer",
  "expires_in": 300
}
```

### 4. Protected Route
```http
GET /protected
Authorization: ******
```

**Response:**
```json
{
  "message": "Hello admin! This is a protected route.",
  "user": "admin"
}
```

### 5. Health Check
```http
GET /health
```

**Response:**
```json
{
  "status": "healthy",
  "timestamp": "2026-06-02T14:42:00.000000"
}
```

## Usage Examples

### Using cURL

1. **Get a token:**
   ```bash
   curl -X POST "http://localhost:8000/token" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -d "username=admin&******"
   ```

2. **Access protected route:**
   ```bash
   TOKEN="your_token_here"
   curl -X GET "http://localhost:8000/protected" \
     -H "Authorization: ******"
   ```

3. **Refresh token:**
   ```bash
   TOKEN="your_token_here"
   curl -X POST "http://localhost:8000/refresh" \
     -H "Authorization: ******"
   ```

### Using Python (requests library)

```python
import requests

# Login
response = requests.post(
    "http://localhost:8000/token",
    data={"username": "admin", "password": "admin123"}
)
token = response.json()["access_token"]

# Access protected route
headers = {"Authorization": f"******"}
response = requests.get("http://localhost:8000/protected", headers=headers)
print(response.json())

# Refresh token
response = requests.post("http://localhost:8000/refresh", headers=headers)
new_token = response.json()["access_token"]
```

### Using JavaScript (fetch)

```javascript
// Login
const loginResponse = await fetch('http://localhost:8000/token', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
  },
  body: 'username=admin&******'
});
const { access_token } = await loginResponse.json();

// Access protected route
const protectedResponse = await fetch('http://localhost:8000/protected', {
  headers: {
    'Authorization': `******
  }
});
const data = await protectedResponse.json();
console.log(data);

// Refresh token
const refreshResponse = await fetch('http://localhost:8000/refresh', {
  method: 'POST',
  headers: {
    'Authorization': `******
  }
});
const { access_token: newToken } = await refreshResponse.json();
```

## Token Details

- **Algorithm:** HS256 (HMAC with SHA-256)
- **Expiration Time:** 300 seconds (5 minutes)
- **Token Type:** ****** **Claims:**
  - `sub`: Username
  - `exp`: Expiration timestamp
  - `iat`: Issued at timestamp

## Security Considerations

⚠️ **Important:** This is a demonstration application. For production use:

1. Change the `SECRET_KEY` in `main.py` to a strong, random value
2. Store credentials securely (use a proper database, not hardcoded values)
3. Use environment variables for sensitive configuration
4. Implement HTTPS/TLS
5. Add rate limiting
6. Implement proper logging and monitoring
7. Consider using a longer token expiration for production
8. Implement refresh token rotation
9. Add CORS configuration if needed for web clients

## Technologies Used

- **FastAPI**: Modern, fast web framework for building APIs
- **Python-JOSE**: JWT encoding/decoding
- **Passlib**: Password hashing with bcrypt
- **Uvicorn**: ASGI server
- **Poetry**: Dependency management
- **Docker**: Containerization

## Development

### Running Tests
```bash
cd backend
poetry run pytest
```

### Code Formatting
```bash
poetry run black main.py
```

### Type Checking
```bash
poetry run mypy main.py
```

## Troubleshooting

### Port Already in Use
If port 8000 is already in use, modify the port in `docker-compose.yml`:
```yaml
ports:
  - "8080:8000"  # Change 8000 to 8080 or another available port
```

### Docker Build Issues
If you encounter build issues, try:
```bash
docker-compose down
docker-compose build --no-cache
docker-compose up
```

### Token Expired
Tokens expire after 300 seconds (5 minutes). Use the `/refresh` endpoint to get a new token.

## License

This project is licensed under the MIT License.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

For issues and questions, please open an issue in the repository.
