# Getting Started

This guide will help you get My Microservice up and running in your local development environment.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** 18 or higher
- **npm** or **yarn** package manager
- **Git** for version control
- **Docker** (optional, for containerized development)
- **curl** or **Postman** for API testing

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/your-org/my-microservice.git
cd my-microservice
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Environment Setup

Copy the example environment file and configure it:

```bash
cp .env.example .env
```

Edit the `.env` file with your configuration:

```env
NODE_ENV=development
PORT=3000
LOG_LEVEL=debug

# Database (optional for development)
DATABASE_URL=postgresql://user:password@localhost:5432/myservice

# Cache (optional for development)
REDIS_URL=redis://localhost:6379

# External services (optional for development)
AUTH_SERVICE_URL=https://auth.example.com
```

### 4. Start the Development Server

```bash
npm run dev
```

The service will start on `http://localhost:3000`.

## Verification

### Health Check

Verify the service is running:

```bash
curl http://localhost:3000/health
```

Expected response:
```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "uptime": 45.123,
  "memory": {
    "rss": 50331648,
    "heapTotal": 20971520,
    "heapUsed": 15728640
  },
  "version": "v18.17.0"
}
```

### API Test

Test the users endpoint:

```bash
curl http://localhost:3000/api/users
```

Expected response:
```json
{
  "data": [
    {
      "id": 1,
      "name": "John Doe",
      "email": "john@example.com"
    },
    {
      "id": 2,
      "name": "Jane Smith",
      "email": "jane@example.com"
    }
  ],
  "total": 2,
  "timestamp": "2024-01-15T10:30:00.000Z"
}
```

## Development Workflow

### Running Tests

```bash
# Run all tests
npm test

# Run tests with coverage
npm run test:coverage

# Run tests in watch mode
npm run test:watch
```

### Code Quality

```bash
# Run linting
npm run lint

# Fix linting issues
npm run lint:fix
```

### Docker Development

If you prefer containerized development:

```bash
# Build the Docker image
npm run docker:build

# Run the container
npm run docker:run

# Or use Docker Compose
docker-compose up
```

## Next Steps

- [Architecture](architecture.md) - Understand the system design
- [API Reference](api-reference.md) - Explore the complete API
- [Development](development.md) - Learn about development practices
- [Deployment](deployment.md) - Deploy to production environments