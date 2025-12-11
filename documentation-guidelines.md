# Documentation Standards Guidelines

## Overview
This document establishes documentation standards to ensure clear, comprehensive, and maintainable documentation across all repositories.

## Documentation Philosophy

### 1. Core Principles
- **Accuracy**: Documentation must reflect current implementation
- **Clarity**: Write for your audience's understanding level
- **Completeness**: Cover all necessary aspects
- **Maintainability**: Keep documentation up-to-date
- **Accessibility**: Make documentation easy to find and navigate
- **Conciseness**: Be thorough but avoid unnecessary verbosity

### 2. Types of Documentation
- **README files**: Project overview and getting started
- **API documentation**: Interface specifications
- **Code comments**: Inline code explanations
- **Architecture documentation**: System design and structure
- **User guides**: End-user instructions
- **Developer guides**: Technical implementation details
- **Tutorials**: Step-by-step learning materials
- **Runbooks**: Operational procedures

## README Documentation

### 3. README Structure
Every project should have a comprehensive README.md:

```markdown
# Project Name

Brief description of what the project does (1-2 sentences).

## Table of Contents
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [API Documentation](#api-documentation)
- [Contributing](#contributing)
- [Testing](#testing)
- [Deployment](#deployment)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Features
- Feature 1
- Feature 2
- Feature 3

## Prerequisites
- Node.js 18+
- PostgreSQL 14+
- Redis 6+

## Installation

\`\`\`bash
# Clone repository
git clone https://github.com/user/repo.git

# Install dependencies
npm install

# Set up environment
cp .env.example .env
\`\`\`

## Usage

\`\`\`bash
# Development mode
npm run dev

# Production mode
npm start
\`\`\`

## Configuration

Environment variables:
- `DATABASE_URL` - PostgreSQL connection string
- `REDIS_URL` - Redis connection string
- `API_KEY` - External API key

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## License

This project is licensed under MIT License - see [LICENSE](LICENSE).
```

### 4. README Best Practices
- Keep README concise but comprehensive
- Use badges for build status, coverage, version
- Include screenshots or demos for visual projects
- Provide quick start guide
- Link to detailed documentation
- Keep information current

## Code Comments

### 5. When to Comment
```javascript
// Good - explains why, not what
// Using exponential backoff to prevent API rate limiting
await retryWithBackoff(apiCall);

// Good - explains complex logic
// Calculate weighted average using inverse variance weighting
// to give more weight to measurements with lower uncertainty
const weightedAvg = calculateWeightedAverage(measurements);

// Good - explains non-obvious behavior
// API returns null for empty results instead of empty array
const results = apiResponse.data || [];

// Avoid - states the obvious
// Increment counter by 1
counter++;

// Avoid - outdated comment
// TODO: Fix this bug (already fixed but comment remains)
```

### 6. Comment Types
```javascript
// Single-line comment for brief explanations

/*
 * Multi-line comment for longer explanations
 * that span multiple lines
 */

/**
 * JSDoc comment for functions, classes
 * @param {string} name - The user's name
 * @returns {object} User object
 */
function getUser(name) { }

// TODO: Add error handling
// FIXME: Memory leak in this function
// HACK: Temporary workaround for API bug
// NOTE: This assumes data is already validated
```

### 7. Comment Best Practices
- Explain "why", not "what" or "how"
- Keep comments up-to-date with code
- Remove commented-out code
- Use proper grammar and spelling
- Be concise but clear
- Avoid redundant comments

## API Documentation

### 8. API Documentation Structure
```markdown
# API Documentation

## Authentication
All API requests require authentication using Bearer tokens.

\`\`\`
Authorization: Bearer YOUR_API_TOKEN
\`\`\`

## Endpoints

### Get User
Retrieves a user by ID.

**Endpoint:** `GET /api/users/{id}`

**Parameters:**
- `id` (required, integer) - The user ID

**Response:**
\`\`\`json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com",
  "createdAt": "2024-01-01T00:00:00Z"
}
\`\`\`

**Status Codes:**
- `200 OK` - Success
- `404 Not Found` - User doesn't exist
- `401 Unauthorized` - Invalid or missing token

**Example:**
\`\`\`bash
curl -H "Authorization: Bearer TOKEN" \
  https://api.example.com/v1/users/123
\`\`\`
```

### 9. API Documentation Tools
- **OpenAPI/Swagger**: Industry-standard API specification
- **Postman**: API testing and documentation
- **API Blueprint**: Markdown-based API documentation
- **RAML**: RESTful API Modeling Language
- **GraphQL Schema**: Self-documenting GraphQL APIs

### 10. OpenAPI Example
```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0
  description: API for managing users

paths:
  /users/{id}:
    get:
      summary: Get user by ID
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          description: User not found

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        email:
          type: string
          format: email
```

## Function/Method Documentation

### 11. JSDoc Example (JavaScript/TypeScript)
```javascript
/**
 * Calculates the total price including tax and discounts.
 * 
 * @param {number} basePrice - The base price before tax
 * @param {number} taxRate - Tax rate as decimal (e.g., 0.08 for 8%)
 * @param {number} [discount=0] - Optional discount amount
 * @returns {number} The final price after tax and discounts
 * @throws {Error} If basePrice or taxRate is negative
 * 
 * @example
 * calculateTotal(100, 0.08, 10); // Returns 97.2
 * 
 * @example
 * // Without discount
 * calculateTotal(100, 0.08); // Returns 108
 */
function calculateTotal(basePrice, taxRate, discount = 0) {
  if (basePrice < 0 || taxRate < 0) {
    throw new Error('Price and tax rate must be non-negative');
  }
  return (basePrice - discount) * (1 + taxRate);
}
```

### 12. Python Docstring Example
```python
def calculate_total(base_price: float, tax_rate: float, discount: float = 0) -> float:
    """
    Calculate the total price including tax and discounts.
    
    Args:
        base_price (float): The base price before tax
        tax_rate (float): Tax rate as decimal (e.g., 0.08 for 8%)
        discount (float, optional): Discount amount. Defaults to 0.
    
    Returns:
        float: The final price after tax and discounts
    
    Raises:
        ValueError: If base_price or tax_rate is negative
    
    Examples:
        >>> calculate_total(100, 0.08, 10)
        97.2
        
        >>> calculate_total(100, 0.08)
        108.0
    """
    if base_price < 0 or tax_rate < 0:
        raise ValueError('Price and tax rate must be non-negative')
    return (base_price - discount) * (1 + tax_rate)
```

### 13. Java Javadoc Example
```java
/**
 * Calculates the total price including tax and discounts.
 * 
 * @param basePrice the base price before tax
 * @param taxRate tax rate as decimal (e.g., 0.08 for 8%)
 * @param discount optional discount amount
 * @return the final price after tax and discounts
 * @throws IllegalArgumentException if basePrice or taxRate is negative
 * 
 * @see #calculateTax(double, double)
 * @since 1.0
 */
public double calculateTotal(double basePrice, double taxRate, double discount) {
    if (basePrice < 0 || taxRate < 0) {
        throw new IllegalArgumentException("Price and tax rate must be non-negative");
    }
    return (basePrice - discount) * (1 + taxRate);
}
```

## Architecture Documentation

### 14. Architecture Overview
```markdown
# System Architecture

## Overview
High-level description of the system architecture.

## Components

### Frontend
- **Technology**: React 18
- **Responsibility**: User interface and client-side logic
- **Communication**: REST API calls to backend

### Backend
- **Technology**: Node.js with Express
- **Responsibility**: Business logic and data processing
- **Communication**: 
  - REST API endpoints for frontend
  - Database queries to PostgreSQL
  - Message queue for async processing

### Database
- **Technology**: PostgreSQL 14
- **Responsibility**: Persistent data storage
- **Schema**: See [schema documentation](./schema.md)

### Message Queue
- **Technology**: RabbitMQ
- **Responsibility**: Asynchronous task processing
- **Use cases**: Email sending, report generation

## Architecture Diagram

\`\`\`
┌─────────┐      ┌─────────┐      ┌──────────┐
│ Frontend│─────▶│ Backend │─────▶│ Database │
└─────────┘      └─────────┘      └──────────┘
                      │
                      ▼
                 ┌──────────┐
                 │  Queue   │
                 └──────────┘
\`\`\`

## Data Flow
1. User interacts with frontend
2. Frontend makes API call to backend
3. Backend processes request and queries database
4. Backend enqueues async tasks if needed
5. Backend returns response to frontend
6. Frontend updates UI

## Security
- All communication over HTTPS
- JWT-based authentication
- Role-based authorization
- Input validation at all layers
```

### 15. Architecture Decision Records (ADR)
```markdown
# ADR 001: Use PostgreSQL for Primary Database

## Status
Accepted

## Context
We need to choose a database for storing application data.

## Decision
We will use PostgreSQL as our primary database.

## Consequences

### Positive
- ACID compliance ensures data integrity
- Excellent support for complex queries
- Strong ecosystem and community
- Good performance for our use case

### Negative
- May require more setup than NoSQL alternatives
- Vertical scaling limitations
- More complex than simple key-value stores

## Alternatives Considered
- MongoDB (rejected: need ACID guarantees)
- MySQL (rejected: prefer PostgreSQL features)
- DynamoDB (rejected: vendor lock-in concerns)
```

## User Documentation

### 16. User Guide Structure
```markdown
# User Guide

## Getting Started
- Account creation
- First-time setup
- Basic concepts

## Features

### Feature 1: User Management
How to create, edit, and delete users.

**Step 1:** Navigate to Users page
Click on "Users" in the main navigation menu.

**Step 2:** Click "Add User"
Fill in the required fields:
- Name (required)
- Email (required)
- Role (required)

**Step 3:** Save
Click "Save" to create the user.

### Feature 2: Reports
How to generate and view reports.

## Troubleshooting

### Issue: Cannot login
**Symptom:** Login button doesn't respond
**Solution:** 
1. Clear browser cache
2. Ensure JavaScript is enabled
3. Try different browser

### Issue: Slow performance
**Symptom:** Pages load slowly
**Solution:**
1. Check network connection
2. Clear browser cache
3. Contact support if issue persists
```

## Developer Documentation

### 17. Developer Guide Structure
```markdown
# Developer Guide

## Getting Started
- Development environment setup
- Building the project
- Running tests

## Architecture
- System overview
- Component relationships
- Design patterns used

## Development Workflow
1. Create feature branch
2. Make changes
3. Write tests
4. Run linters
5. Submit pull request

## Coding Standards
- Follow style guide
- Write meaningful tests
- Document public APIs
- Review checklist

## Common Tasks

### Adding a New Feature
1. Create feature branch
2. Implement feature
3. Add tests
4. Update documentation
5. Submit PR

### Fixing a Bug
1. Write failing test
2. Fix bug
3. Verify test passes
4. Submit PR

## API Reference
- Detailed API documentation
- Request/response examples
- Error codes and handling
```

## Runbooks

### 18. Runbook Structure
```markdown
# Production Runbook

## Service Overview
Description of the service and its dependencies.

## Deployment

### Pre-deployment Checklist
- [ ] All tests passing
- [ ] Code review approved
- [ ] Database migrations ready
- [ ] Monitoring configured

### Deployment Steps
1. Backup database
2. Run database migrations
3. Deploy new version
4. Verify health checks
5. Monitor logs for errors

### Rollback Procedure
1. Deploy previous version
2. Rollback database migrations if needed
3. Verify service health

## Monitoring

### Key Metrics
- Response time (target: < 200ms)
- Error rate (target: < 0.1%)
- CPU usage (alert: > 80%)
- Memory usage (alert: > 85%)

### Alerts
- **Critical**: Service down
- **Warning**: High error rate
- **Info**: Deployment complete

## Troubleshooting

### Service Won't Start
**Symptoms:**
- Process crashes immediately
- Health check fails

**Investigation:**
1. Check application logs
2. Verify environment variables
3. Check database connectivity

**Resolution:**
1. Fix configuration issues
2. Restart service
3. Monitor logs

### Database Connection Issues
**Symptoms:**
- Connection timeout errors
- Intermittent failures

**Investigation:**
1. Check database status
2. Verify connection pool settings
3. Check network connectivity

**Resolution:**
1. Restart database if needed
2. Adjust connection pool
3. Check firewall rules

## Emergency Contacts
- On-call engineer: [contact info]
- Database admin: [contact info]
- Infrastructure team: [contact info]
```

## Change Log

### 19. Change Log Format
```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]
### Added
- New feature description

### Changed
- Updated feature description

### Fixed
- Bug fix description

## [1.2.0] - 2024-01-15
### Added
- User authentication with JWT
- Password reset functionality
- Email verification

### Changed
- Improved API response time
- Updated dependencies

### Fixed
- Fixed login validation bug
- Resolved memory leak in cache

### Security
- Fixed XSS vulnerability in user input

## [1.1.0] - 2023-12-01
...
```

## Documentation Maintenance

### 20. Keeping Documentation Current
- Update docs with code changes
- Review docs during code review
- Set up documentation linting
- Regular documentation audits
- Assign documentation ownership
- Use automated doc generation where possible

### 21. Documentation in CI/CD
```yaml
# Example CI configuration
documentation:
  - name: Check for broken links
    run: npm run check-links
  
  - name: Validate OpenAPI spec
    run: npm run validate-openapi
  
  - name: Generate API docs
    run: npm run generate-docs
  
  - name: Spell check
    run: npm run spellcheck
```

## Documentation Tools

### 22. Documentation Generators
- **JSDoc**: JavaScript documentation
- **Sphinx**: Python documentation
- **Javadoc**: Java documentation
- **GoDoc**: Go documentation
- **Doxygen**: Multi-language documentation
- **MkDocs**: Markdown-based documentation
- **Docusaurus**: React-based documentation sites
- **GitBook**: Modern documentation platform

### 23. Diagramming Tools
- **Mermaid**: Text-based diagrams in Markdown
- **PlantUML**: UML diagrams from text
- **Draw.io**: Visual diagramming
- **Lucidchart**: Professional diagrams
- **Excalidraw**: Hand-drawn style diagrams

### 24. Mermaid Examples
```markdown
## Sequence Diagram

\`\`\`mermaid
sequenceDiagram
    Client->>+Server: POST /api/users
    Server->>+Database: INSERT user
    Database-->>-Server: Success
    Server-->>-Client: 201 Created
\`\`\`

## Flowchart

\`\`\`mermaid
flowchart TD
    A[Start] --> B{Is valid?}
    B -->|Yes| C[Process]
    B -->|No| D[Reject]
    C --> E[End]
    D --> E
\`\`\`
```

## Best Practices Summary

1. **Write documentation as you code**
2. **Keep documentation close to code**
3. **Use appropriate documentation type**
4. **Make documentation searchable**
5. **Include examples and tutorials**
6. **Keep documentation up-to-date**
7. **Review documentation in code reviews**
8. **Use consistent formatting**
9. **Automate documentation generation**
10. **Make documentation accessible**

## Documentation Checklist

### For Every Project
- [ ] README with setup instructions
- [ ] API documentation
- [ ] Code comments where needed
- [ ] Contributing guide
- [ ] License file
- [ ] Change log

### For Public Projects
- [ ] User documentation
- [ ] Tutorial or getting started guide
- [ ] FAQ section
- [ ] Examples and demos
- [ ] Documentation website

### For Internal Projects
- [ ] Architecture documentation
- [ ] Deployment guide
- [ ] Runbook
- [ ] Development guide
- [ ] ADRs for important decisions

## References
- [Write the Docs](https://www.writethedocs.org/)
- [Google Developer Documentation Style Guide](https://developers.google.com/style)
- [Microsoft Writing Style Guide](https://docs.microsoft.com/en-us/style-guide/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Semantic Versioning](https://semver.org/)
