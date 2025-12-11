# Global Instructions Repository

## Overview
This repository contains comprehensive guidelines and best practices to be enforced across different repositories through GitHub Copilot. These guidelines ensure consistent code quality, security, performance, and maintainability across all projects.

## Available Guidelines

### 📋 [Naming Conventions Guidelines](./naming-guidelines.md)
Establishes consistent naming standards across all programming languages including:
- Variable, function, and class naming conventions
- Language-specific naming patterns (JavaScript, Python, Java, Go, Ruby, etc.)
- Database naming conventions
- API and REST endpoint naming
- Git commit and branch naming
- Boolean and collection naming patterns

### 🔒 [Security Best Practices](./security-guidelines.md)
Comprehensive security guidelines covering:
- Authentication and authorization
- Input validation and sanitization
- Secrets management and encryption
- API and database security
- Session management
- Error handling and logging
- Dependency management
- Security testing and compliance

### ⚡ [Performance Optimization Guidelines](./performance-guidelines.md)
Performance best practices including:
- Database query optimization
- API response optimization
- Caching strategies (multi-level, distributed)
- Frontend performance (asset optimization, loading strategies)
- Backend optimization (code, async processing, memory management)
- Network and microservices performance
- Monitoring, profiling, and testing

### 🏗️ [API Design Guidelines](./api-design-guidelines.md)
RESTful API design principles covering:
- Resource-oriented design
- HTTP methods and status codes
- URL structure and versioning
- Request/response design
- Pagination, filtering, and sorting
- Authentication and rate limiting
- Error handling and documentation
- GraphQL considerations

### ✨ [Code Quality Guidelines](./code-quality-guidelines.md)
Code quality and standards including:
- Code organization and file structure
- Function design and readability
- Error handling and validation
- SOLID principles
- Code smells to avoid
- Defensive programming
- Testing and code reviews
- Documentation and version control

### 🧪 [Testing Best Practices](./testing-guidelines.md)
Comprehensive testing standards covering:
- Testing philosophy and principles
- Unit testing best practices
- Integration and E2E testing
- Test data management
- Mocking and stubbing
- Test coverage metrics
- Async and performance testing
- Security testing

### 📚 [Documentation Standards](./documentation-guidelines.md)
Documentation guidelines including:
- README structure and content
- Code comments best practices
- API documentation (OpenAPI/Swagger)
- Function/method documentation (JSDoc, docstrings, Javadoc)
- Architecture documentation
- User and developer guides
- Runbooks and changelogs

## How to Use These Guidelines

### For Developers
1. **Read relevant guidelines** before starting a new project or feature
2. **Reference during development** to ensure compliance with best practices
3. **Use in code reviews** to maintain consistent standards
4. **Customize** for project-specific needs while maintaining core principles

### For GitHub Copilot
These guidelines are designed to be enforced through GitHub Copilot across different repositories:
1. Reference these guidelines in repository-specific instructions
2. Use them to guide code generation and suggestions
3. Ensure consistency across multiple projects
4. Validate code against these standards

### For Teams
1. **Onboarding**: Share these guidelines with new team members
2. **Code Reviews**: Use as checklist for reviewing pull requests
3. **Architecture Decisions**: Reference when making design choices
4. **Training**: Use as training material for best practices

## Guideline Categories

| Category | Focus Area | Key Topics |
|----------|------------|------------|
| **Security** | Protect against vulnerabilities | Authentication, encryption, input validation |
| **Performance** | Optimize speed and efficiency | Caching, queries, resource management |
| **Naming** | Consistent code conventions | Variables, functions, APIs, databases |
| **API Design** | RESTful interfaces | Endpoints, status codes, versioning |
| **Code Quality** | Maintainable code | SOLID, DRY, testing, reviews |
| **Testing** | Software reliability | Unit tests, integration tests, coverage |
| **Documentation** | Clear communication | READMEs, API docs, comments |

## Contributing

To add or update guidelines:
1. Ensure changes align with industry best practices
2. Provide clear examples and use cases
3. Keep guidelines language-agnostic where possible
4. Include references to authoritative sources
5. Submit a pull request with clear description

## Maintenance

These guidelines should be:
- **Reviewed regularly** to ensure they reflect current best practices
- **Updated** as technologies and standards evolve
- **Validated** against real-world usage and feedback
- **Versioned** to track changes over time

## License

These guidelines are provided as-is for use across projects. Feel free to adapt them to your organization's specific needs.

## References

- OWASP Top 10 Security Guidelines
- Clean Code by Robert C. Martin
- Code Complete by Steve McConnell
- REST API Design Best Practices
- Language-specific style guides (Google, Airbnb, etc.)

---

**Last Updated**: December 2024  
**Version**: 1.0.0