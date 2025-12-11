# API Design Guidelines

## Overview
This document establishes best practices for designing robust, scalable, and developer-friendly APIs across all repositories.

## REST API Design Principles

### 1. Resource-Oriented Design
- Use nouns for resource names, not verbs
- Use plural nouns for collections
- Keep URLs simple and intuitive
- Maintain consistent naming conventions

```
// Good
GET    /api/users
GET    /api/users/{id}
POST   /api/users
PUT    /api/users/{id}
DELETE /api/users/{id}

// Avoid
GET    /api/getAllUsers
POST   /api/createUser
GET    /api/user/{id}  // singular instead of plural
```

### 2. HTTP Methods
Use appropriate HTTP methods for operations:

- **GET**: Retrieve resources (safe, idempotent)
- **POST**: Create new resources or non-idempotent operations
- **PUT**: Update entire resource (idempotent)
- **PATCH**: Partial update of resource (idempotent)
- **DELETE**: Remove resource (idempotent)
- **HEAD**: Get metadata without body
- **OPTIONS**: Get supported methods

### 3. Nested Resources
Express relationships through URL structure:

```
// Good - shows relationship
GET    /api/users/{userId}/orders
GET    /api/users/{userId}/orders/{orderId}
POST   /api/users/{userId}/orders

// Limit nesting depth (max 2-3 levels)
// Avoid
GET /api/companies/{id}/departments/{id}/teams/{id}/members/{id}
```

## URL Design

### 4. URL Structure
```
// Consistent structure
https://api.example.com/v1/resource-name
https://api.example.com/v1/users
https://api.example.com/v1/product-categories

// Use kebab-case for multi-word resources
/api/payment-methods
/api/shipping-addresses
/api/order-items
```

### 5. Query Parameters
Use query parameters for filtering, sorting, pagination, and searching:

```
// Filtering
GET /api/users?status=active&role=admin

// Sorting
GET /api/products?sortBy=price&order=desc

// Pagination
GET /api/users?page=2&pageSize=50

// Searching
GET /api/products?search=laptop&category=electronics

// Field selection
GET /api/users?fields=id,name,email
```

## Versioning

### 6. API Versioning
Choose one versioning strategy and use consistently:

```
// URL versioning (recommended for major versions)
https://api.example.com/v1/users
https://api.example.com/v2/users

// Header versioning
Accept: application/vnd.company.api+json;version=1

// Query parameter (not recommended)
https://api.example.com/users?version=1
```

**Best Practice**: 
- Use URL versioning for major breaking changes
- Maintain backward compatibility within versions
- Deprecate old versions with proper notice

## Request Design

### 7. Request Headers
```
// Standard headers
Content-Type: application/json
Accept: application/json
Authorization: Bearer {token}
X-API-Key: {api-key}
Accept-Language: en-US
User-Agent: MyApp/1.0

// Custom headers (use X- prefix)
X-Request-ID: unique-request-id
X-Correlation-ID: correlation-id
X-Client-Version: 1.2.3
```

### 8. Request Body
```json
// Good - clear, structured JSON
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "age": 30,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zipCode": "10001"
  }
}

// Use camelCase for JSON properties
// Provide clear validation error messages
```

## Response Design

### 9. HTTP Status Codes
Use appropriate status codes:

**Success (2xx)**
- `200 OK` - Successful GET, PUT, PATCH, DELETE
- `201 Created` - Successful POST with resource creation
- `202 Accepted` - Request accepted for async processing
- `204 No Content` - Successful request with no response body

**Client Errors (4xx)**
- `400 Bad Request` - Invalid request syntax or validation error
- `401 Unauthorized` - Missing or invalid authentication
- `403 Forbidden` - Authenticated but not authorized
- `404 Not Found` - Resource doesn't exist
- `405 Method Not Allowed` - HTTP method not supported
- `409 Conflict` - Conflict with current state
- `422 Unprocessable Entity` - Semantic validation errors
- `429 Too Many Requests` - Rate limit exceeded

**Server Errors (5xx)**
- `500 Internal Server Error` - Unexpected server error
- `502 Bad Gateway` - Invalid upstream response
- `503 Service Unavailable` - Temporary unavailability
- `504 Gateway Timeout` - Upstream timeout

### 10. Response Body Structure
```json
// Success response
{
  "data": {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com"
  },
  "meta": {
    "timestamp": "2024-01-01T12:00:00Z",
    "version": "1.0"
  }
}

// Collection response with pagination
{
  "data": [
    {"id": 1, "name": "Item 1"},
    {"id": 2, "name": "Item 2"}
  ],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "totalPages": 5,
    "totalItems": 100,
    "hasNext": true,
    "hasPrev": false
  }
}

// Error response
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format",
        "code": "INVALID_FORMAT"
      }
    ]
  },
  "meta": {
    "timestamp": "2024-01-01T12:00:00Z",
    "requestId": "abc-123"
  }
}
```

### 11. Response Headers
```
// Standard headers
Content-Type: application/json
Cache-Control: no-cache, no-store, must-revalidate
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
X-Rate-Limit-Limit: 1000
X-Rate-Limit-Remaining: 999
X-Rate-Limit-Reset: 1640995200

// CORS headers
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
```

## Pagination

### 12. Pagination Strategies
```
// Offset-based pagination
GET /api/users?page=2&pageSize=50

// Cursor-based pagination (for large datasets)
GET /api/users?cursor=eyJpZCI6MTAwfQ&limit=50

// Response includes pagination metadata
{
  "data": [...],
  "pagination": {
    "next": "/api/users?cursor=eyJpZCI6MTUwfQ",
    "prev": "/api/users?cursor=eyJpZCI6NTB9"
  }
}
```

## Filtering and Searching

### 13. Filtering
```
// Simple filters
GET /api/products?category=electronics&price_min=100&price_max=500

// Complex filters (using query language)
GET /api/products?filter=category eq 'electronics' and price gt 100

// Multiple values
GET /api/users?roles=admin,editor&status=active
```

### 14. Searching
```
// Full-text search
GET /api/products?search=wireless+headphones

// Field-specific search
GET /api/users?email=*@example.com&name=John*

// Search with filters
GET /api/products?search=laptop&category=electronics&inStock=true
```

## Sorting

### 15. Sorting Parameters
```
// Single field
GET /api/products?sortBy=price&order=desc

// Multiple fields
GET /api/products?sortBy=category,price&order=asc,desc

// Alternative syntax
GET /api/products?sort=-price,+name  // - for desc, + for asc
```

## Authentication & Authorization

### 16. Authentication Methods
```
// Bearer token (OAuth 2.0, JWT)
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

// API Key
X-API-Key: your-api-key-here

// Basic Auth (only over HTTPS)
Authorization: Basic base64(username:password)
```

### 17. Authorization
- Implement proper role-based access control (RBAC)
- Return 401 for authentication issues
- Return 403 for authorization issues
- Don't expose whether a resource exists if user lacks permission

## Rate Limiting

### 18. Rate Limit Implementation
```
// Rate limit headers
X-Rate-Limit-Limit: 1000
X-Rate-Limit-Remaining: 999
X-Rate-Limit-Reset: 1640995200
Retry-After: 3600

// Rate limit error response (429)
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "API rate limit exceeded",
    "retryAfter": 3600
  }
}
```

## Caching

### 19. Cache Headers
```
// Cache control
Cache-Control: public, max-age=3600
Cache-Control: private, no-cache
Cache-Control: no-store

// ETags for conditional requests
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
If-None-Match: "33a64df551425fcc55e4d42a148795d9f25f89d4"
// Returns 304 Not Modified if unchanged

// Last-Modified
Last-Modified: Wed, 21 Oct 2023 07:28:00 GMT
If-Modified-Since: Wed, 21 Oct 2023 07:28:00 GMT
```

## Asynchronous Operations

### 20. Long-Running Operations
```
// Initial request returns 202 Accepted
POST /api/reports/generate
Response: 202 Accepted
{
  "id": "report-123",
  "status": "processing",
  "statusUrl": "/api/reports/report-123/status",
  "estimatedCompletion": "2024-01-01T12:05:00Z"
}

// Poll for status
GET /api/reports/report-123/status
Response: 200 OK
{
  "id": "report-123",
  "status": "completed",
  "resultUrl": "/api/reports/report-123/download"
}
```

## Webhooks

### 21. Webhook Design
```
// Webhook payload
POST https://client.example.com/webhooks/orders
{
  "event": "order.created",
  "timestamp": "2024-01-01T12:00:00Z",
  "data": {
    "orderId": 123,
    "status": "pending",
    "amount": 99.99
  },
  "signature": "sha256=..."  // for verification
}

// Implement retry logic for failed deliveries
// Allow webhook registration and management via API
```

## Error Handling

### 22. Error Response Format
```json
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "User with ID 123 not found",
    "details": [
      {
        "field": "userId",
        "value": 123,
        "issue": "Resource does not exist"
      }
    ],
    "documentation": "https://docs.example.com/errors/RESOURCE_NOT_FOUND"
  },
  "meta": {
    "requestId": "abc-123",
    "timestamp": "2024-01-01T12:00:00Z"
  }
}
```

### 23. Error Codes
- Use consistent error code naming conventions
- Provide machine-readable error codes
- Include human-readable error messages
- Link to error documentation
- Don't expose sensitive information in errors

## Documentation

### 24. API Documentation
- Use OpenAPI/Swagger specification
- Provide interactive API documentation
- Include example requests and responses
- Document all parameters, headers, and status codes
- Maintain up-to-date documentation
- Provide SDK/client library examples

### 25. Documentation Tools
- Swagger/OpenAPI
- Postman Collections
- API Blueprint
- RAML
- GraphQL Schema Documentation

## GraphQL Considerations

### 26. GraphQL Best Practices
```graphql
# Clear schema design
type User {
  id: ID!
  name: String!
  email: String!
  posts: [Post!]!
}

type Query {
  user(id: ID!): User
  users(first: Int, after: String): UserConnection
}

type Mutation {
  createUser(input: CreateUserInput!): CreateUserPayload!
  updateUser(id: ID!, input: UpdateUserInput!): UpdateUserPayload!
}

# Use connections for pagination
# Implement proper error handling
# Use DataLoader to prevent N+1 queries
# Set query complexity limits
```

## Security Best Practices

### 27. API Security
- Always use HTTPS
- Implement proper authentication and authorization
- Validate all inputs
- Sanitize outputs to prevent injection attacks
- Implement rate limiting
- Use CORS appropriately
- Log security-relevant events
- Keep dependencies updated
- Implement API key rotation
- Use security headers (HSTS, CSP, etc.)

## Deprecation Strategy

### 28. Deprecating APIs
```
// Provide deprecation headers
Sunset: Sat, 31 Dec 2024 23:59:59 GMT
Deprecation: true
Link: <https://api.example.com/v2/users>; rel="successor-version"

// Provide clear migration guide
// Announce deprecation well in advance
// Maintain old version during transition period
// Log usage of deprecated endpoints
```

## Testing

### 29. API Testing
- Write comprehensive integration tests
- Test all status codes and error scenarios
- Test rate limiting
- Test authentication and authorization
- Validate response schemas
- Test pagination edge cases
- Load and performance testing
- Security testing

## Monitoring

### 30. API Monitoring
- Monitor response times and latency
- Track error rates by endpoint
- Monitor rate limit usage
- Track API usage patterns
- Set up alerts for anomalies
- Implement distributed tracing
- Log all requests with correlation IDs

## References
- REST API Design Best Practices
- OpenAPI Specification
- RFC 7231 (HTTP/1.1 Semantics)
- RFC 6749 (OAuth 2.0)
- GraphQL Best Practices
