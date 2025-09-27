# API Reference

Complete API documentation for My Microservice endpoints.

## Base URL

- **Production**: `https://my-microservice.example.com`
- **Staging**: `https://my-microservice-staging.example.com`
- **Development**: `http://localhost:3000`

## Authentication

All API endpoints (except health checks) require authentication via Bearer token:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
  https://my-microservice.example.com/api/users
```

## Health & System Endpoints

### GET /

Returns basic service information.

**Response:**
```json
{
  "message": "Welcome to My Microservice",
  "version": "1.0.0",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "environment": "production"
}
```

### GET /health

Health check endpoint for liveness probes.

**Response:**
```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "uptime": 3600.123,
  "memory": {
    "rss": 50331648,
    "heapTotal": 20971520,
    "heapUsed": 15728640
  },
  "version": "v18.17.0"
}
```

### GET /ready

Readiness check endpoint for readiness probes.

**Response:**
```json
{
  "status": "ready",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "checks": {
    "database": "ok",
    "externalService": "ok",
    "cache": "ok"
  }
}
```

## User Management Endpoints

### GET /api/users

Retrieve a paginated list of users.

**Query Parameters:**
- `page` (integer, optional): Page number (default: 1)
- `limit` (integer, optional): Items per page (default: 10, max: 100)
- `search` (string, optional): Search term for filtering

**Example Request:**
```bash
curl "https://my-microservice.example.com/api/users?page=1&limit=10&search=john"
```

**Response:**
```json
{
  "data": [
    {
      "id": 1,
      "name": "John Doe",
      "email": "john@example.com",
      "status": "active",
      "createdAt": "2024-01-15T10:30:00.000Z",
      "updatedAt": "2024-01-15T10:30:00.000Z"
    }
  ],
  "total": 1,
  "page": 1,
  "limit": 10,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

### POST /api/users

Create a new user.

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Example Request:**
```bash
curl -X POST https://my-microservice.example.com/api/users \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"name": "John Doe", "email": "john@example.com"}'
```

**Response (201 Created):**
```json
{
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "status": "active",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  },
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

### GET /api/users/{id}

Retrieve a specific user by ID.

**Path Parameters:**
- `id` (integer, required): User ID

**Example Request:**
```bash
curl https://my-microservice.example.com/api/users/1
```

**Response:**
```json
{
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "status": "active",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:30:00.000Z"
  },
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

### PUT /api/users/{id}

Update an existing user.

**Path Parameters:**
- `id` (integer, required): User ID

**Request Body:**
```json
{
  "name": "John Smith",
  "email": "john.smith@example.com",
  "status": "active"
}
```

**Example Request:**
```bash
curl -X PUT https://my-microservice.example.com/api/users/1 \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"name": "John Smith", "email": "john.smith@example.com"}'
```

**Response:**
```json
{
  "data": {
    "id": 1,
    "name": "John Smith",
    "email": "john.smith@example.com",
    "status": "active",
    "createdAt": "2024-01-15T10:30:00.000Z",
    "updatedAt": "2024-01-15T10:35:00.000Z"
  },
  "timestamp": "2024-01-15T10:35:00.000Z"
}
```

### DELETE /api/users/{id}

Delete a user.

**Path Parameters:**
- `id` (integer, required): User ID

**Example Request:**
```bash
curl -X DELETE https://my-microservice.example.com/api/users/1 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**Response (204 No Content):**
No response body.

## Error Responses

All error responses follow this format:

```json
{
  "error": "Error message",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "details": "Additional error details (optional)"
}
```

### Common HTTP Status Codes

- **200 OK** - Request successful
- **201 Created** - Resource created successfully
- **204 No Content** - Request successful, no content returned
- **400 Bad Request** - Invalid request parameters
- **401 Unauthorized** - Authentication required
- **403 Forbidden** - Access denied
- **404 Not Found** - Resource not found
- **409 Conflict** - Resource already exists
- **500 Internal Server Error** - Server error

## Rate Limiting

API requests are rate limited:

- **100 requests per minute** per IP address
- **1000 requests per hour** per authenticated user

Rate limit headers are included in responses:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1642248000
```

## OpenAPI Specification

The complete OpenAPI 3.0 specification is available at:
- [OpenAPI YAML](../openapi.yaml)
- [Interactive Documentation](https://my-microservice.example.com/docs)

## SDKs and Client Libraries

Official client libraries are available for:

- **JavaScript/TypeScript** - `npm install @company/my-microservice-client`
- **Python** - `pip install my-microservice-client`
- **Go** - `go get github.com/company/my-microservice-go`

## Postman Collection

Import our Postman collection for easy API testing:

[Download Postman Collection](https://github.com/your-org/my-microservice/blob/main/postman/collection.json)