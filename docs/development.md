# Development Guide

Guidelines and best practices for developing My Microservice.

## Development Setup

### Local Environment

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Set up environment:**
   ```bash
   cp .env.example .env
   ```

3. **Start development server:**
   ```bash
   npm run dev
   ```

### Development Dependencies

- **nodemon** - Auto-restart on file changes
- **jest** - Testing framework
- **supertest** - HTTP testing
- **eslint** - Code linting

## Code Standards

### ESLint Configuration

```javascript
// .eslintrc.js
module.exports = {
  env: {
    node: true,
    es2021: true,
    jest: true
  },
  extends: ['eslint:recommended'],
  rules: {
    'no-console': 'warn',
    'no-unused-vars': 'error'
  }
};
```

### Code Style

- Use **camelCase** for variables and functions
- Use **PascalCase** for classes
- Use **UPPER_SNAKE_CASE** for constants
- Maximum line length: 100 characters

## Testing Strategy

### Unit Tests

```javascript
// tests/unit/userService.test.js
const userService = require('../../src/services/userService');

describe('UserService', () => {
  test('should create user', async () => {
    const user = await userService.createUser({
      name: 'John Doe',
      email: 'john@example.com'
    });
    expect(user.id).toBeDefined();
  });
});
```

### Integration Tests

```javascript
// tests/integration/users.test.js
const request = require('supertest');
const app = require('../../src/app');

describe('Users API', () => {
  test('GET /api/users', async () => {
    const response = await request(app)
      .get('/api/users')
      .expect(200);
    
    expect(response.body.data).toBeInstanceOf(Array);
  });
});
```

## Git Workflow

### Branch Naming

- `feature/feature-name` - New features
- `bugfix/bug-description` - Bug fixes
- `hotfix/critical-fix` - Critical production fixes

### Commit Messages

Follow conventional commits:

```
feat: add user creation endpoint
fix: resolve database connection issue
docs: update API documentation
test: add unit tests for user service
```

## API Development

### Adding New Endpoints

1. **Create route handler:**
   ```javascript
   // src/controllers/userController.js
   const getUser = async (req, res) => {
     // Implementation
   };
   ```

2. **Add route:**
   ```javascript
   // src/routes/users.js
   router.get('/:id', userController.getUser);
   ```

3. **Add tests:**
   ```javascript
   // tests/integration/users.test.js
   test('GET /api/users/:id', async () => {
     // Test implementation
   });
   ```

4. **Update OpenAPI spec:**
   ```yaml
   # openapi.yaml
   /api/users/{id}:
     get:
       summary: Get user by ID
   ```

## Database Migrations

### Creating Migrations

```javascript
// migrations/001_create_users_table.js
exports.up = function(knex) {
  return knex.schema.createTable('users', table => {
    table.increments('id').primary();
    table.string('name').notNullable();
    table.string('email').unique().notNullable();
    table.timestamps(true, true);
  });
};

exports.down = function(knex) {
  return knex.schema.dropTable('users');
};
```

## Performance Optimization

### Caching Strategy

```javascript
// src/middleware/cache.js
const cache = (duration) => {
  return (req, res, next) => {
    const key = req.originalUrl;
    const cached = redis.get(key);
    
    if (cached) {
      return res.json(JSON.parse(cached));
    }
    
    res.sendResponse = res.json;
    res.json = (body) => {
      redis.setex(key, duration, JSON.stringify(body));
      res.sendResponse(body);
    };
    
    next();
  };
};
```

### Database Optimization

- Use connection pooling
- Implement proper indexing
- Use prepared statements
- Monitor query performance

## Security Best Practices

### Input Validation

```javascript
// src/middleware/validation.js
const { body, validationResult } = require('express-validator');

const validateUser = [
  body('name').isLength({ min: 1 }).trim().escape(),
  body('email').isEmail().normalizeEmail(),
  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    next();
  }
];
```

### Authentication Middleware

```javascript
// src/middleware/auth.js
const authenticateToken = (req, res, next) => {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.sendStatus(401);
  }
  
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) return res.sendStatus(403);
    req.user = user;
    next();
  });
};
```

## Monitoring and Logging

### Structured Logging

```javascript
// src/utils/logger.js
const winston = require('winston');

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'app.log' })
  ]
});
```

### Metrics Collection

```javascript
// src/middleware/metrics.js
const prometheus = require('prom-client');

const httpRequestDuration = new prometheus.Histogram({
  name: 'http_request_duration_seconds',
  help: 'Duration of HTTP requests in seconds',
  labelNames: ['method', 'route', 'status']
});

const metricsMiddleware = (req, res, next) => {
  const start = Date.now();
  
  res.on('finish', () => {
    const duration = (Date.now() - start) / 1000;
    httpRequestDuration
      .labels(req.method, req.route?.path || req.path, res.statusCode)
      .observe(duration);
  });
  
  next();
};
```

## Deployment Preparation

### Environment Configuration

```javascript
// src/config/index.js
module.exports = {
  port: process.env.PORT || 3000,
  nodeEnv: process.env.NODE_ENV || 'development',
  database: {
    url: process.env.DATABASE_URL,
    pool: {
      min: 2,
      max: 10
    }
  },
  redis: {
    url: process.env.REDIS_URL
  }
};
```

### Health Checks

```javascript
// src/routes/health.js
router.get('/health', async (req, res) => {
  const health = {
    status: 'healthy',
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
    memory: process.memoryUsage(),
    version: process.version
  };
  
  res.json(health);
});

router.get('/ready', async (req, res) => {
  try {
    // Check database connection
    await db.raw('SELECT 1');
    
    // Check Redis connection
    await redis.ping();
    
    res.json({
      status: 'ready',
      timestamp: new Date().toISOString(),
      checks: {
        database: 'ok',
        cache: 'ok'
      }
    });
  } catch (error) {
    res.status(503).json({
      status: 'not ready',
      error: error.message
    });
  }
});
```