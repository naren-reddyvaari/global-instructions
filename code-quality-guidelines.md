# Code Quality and Standards Guidelines

## Overview
This document establishes code quality standards and best practices to ensure maintainable, readable, and robust code across all repositories.

## Code Organization

### 1. File Structure
- Organize files by feature or domain, not by type
- Keep related code together (cohesion)
- Limit file size (typically 300-500 lines)
- One class/component per file (with exceptions for closely related helpers)
- Group imports logically (standard library, third-party, internal)

### 2. Directory Structure
```
// Feature-based organization (preferred)
src/
  features/
    user/
      user.service.ts
      user.controller.ts
      user.model.ts
      user.types.ts
      user.test.ts
    payment/
      payment.service.ts
      payment.controller.ts
      payment.model.ts
      payment.test.ts

// Layer-based organization
src/
  controllers/
  services/
  models/
  utils/
  tests/
```

## Code Readability

### 3. Function Design
- Keep functions small and focused (single responsibility)
- Limit function length (typically 20-30 lines)
- Use descriptive function names that indicate purpose
- Limit function parameters (ideally 3 or fewer)
- Use parameter objects for multiple related parameters
- Avoid deeply nested code (max 3-4 levels)

```javascript
// Good - clear, focused function
function calculateOrderTotal(items, taxRate, discountCode) {
  const subtotal = calculateSubtotal(items);
  const discount = applyDiscount(subtotal, discountCode);
  const tax = calculateTax(subtotal - discount, taxRate);
  return subtotal - discount + tax;
}

// Avoid - doing too much
function processOrder(order) {
  // 100+ lines of mixed responsibilities
}
```

### 4. Variable Declaration
- Use meaningful variable names
- Declare variables close to their usage
- Initialize variables when declared
- Use const by default, let when needed, avoid var
- Avoid magic numbers - use named constants

```javascript
// Good
const MAX_LOGIN_ATTEMPTS = 3;
const TIMEOUT_DURATION = 5000;

if (loginAttempts >= MAX_LOGIN_ATTEMPTS) {
  lockAccount();
}

// Avoid
if (loginAttempts >= 3) {  // magic number
  lockAccount();
}
```

### 5. Comments
- Write self-documenting code that needs minimal comments
- Use comments to explain "why", not "what"
- Keep comments up to date with code changes
- Remove commented-out code
- Use JSDoc/docstrings for public APIs

```javascript
// Good - explains why
// Using exponential backoff to avoid overwhelming the API during outages
await retryWithBackoff(apiCall, maxRetries);

// Avoid - states the obvious
// Increment counter by 1
counter++;
```

## Error Handling

### 6. Exception Handling
- Use exceptions for exceptional cases, not control flow
- Catch specific exceptions, not generic ones
- Always clean up resources (use try-finally or RAII)
- Provide meaningful error messages
- Don't swallow exceptions without logging

```javascript
// Good
try {
  const data = await fetchData();
  return processData(data);
} catch (error) {
  logger.error('Failed to fetch data', { error, context });
  throw new DataFetchError('Unable to retrieve data', { cause: error });
} finally {
  cleanup();
}

// Avoid
try {
  const data = await fetchData();
  return processData(data);
} catch (error) {
  // Silent failure
}
```

### 7. Error Messages
- Include actionable information in error messages
- Provide context about what failed
- Don't expose sensitive information
- Use error codes for programmatic handling
- Log errors with sufficient context

## Code Duplication

### 8. DRY Principle (Don't Repeat Yourself)
- Extract common logic into reusable functions
- Use inheritance or composition appropriately
- Create utility functions for repeated operations
- Balance DRY with readability (avoid premature abstraction)

```javascript
// Good - reusable validation
function validateEmail(email) {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
}

// Use consistently
if (!validateEmail(userEmail)) { }
if (!validateEmail(adminEmail)) { }

// Avoid - duplicated logic
if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(userEmail)) { }
if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(adminEmail)) { }
```

## SOLID Principles

### 9. Single Responsibility Principle
- Each class/module should have one reason to change
- Separate concerns (business logic, data access, presentation)
- Keep classes focused on a single task

### 10. Open/Closed Principle
- Open for extension, closed for modification
- Use interfaces and abstract classes
- Favor composition over inheritance
- Use design patterns appropriately

### 11. Liskov Substitution Principle
- Derived classes should be substitutable for base classes
- Don't strengthen preconditions or weaken postconditions
- Maintain behavioral compatibility

### 12. Interface Segregation Principle
- Many specific interfaces are better than one general interface
- Don't force clients to depend on unused methods
- Keep interfaces focused and minimal

### 13. Dependency Inversion Principle
- Depend on abstractions, not concretions
- Use dependency injection
- Invert control flow for better testability

## Code Smells to Avoid

### 14. Common Code Smells
- **Long Methods**: Break into smaller functions
- **Large Classes**: Split into multiple classes
- **Long Parameter Lists**: Use parameter objects
- **Duplicate Code**: Extract common logic
- **Dead Code**: Remove unused code
- **Speculative Generality**: Don't over-engineer for future needs
- **Feature Envy**: Method uses another class more than its own
- **Data Clumps**: Group related data into objects
- **Primitive Obsession**: Use domain objects instead of primitives
- **Switch Statements**: Consider polymorphism

## Defensive Programming

### 15. Input Validation
- Validate all external inputs
- Check preconditions at function entry
- Use type checking and assertions
- Fail fast on invalid inputs
- Provide clear error messages

```javascript
// Good - validates inputs
function calculateDiscount(price, discountPercent) {
  if (typeof price !== 'number' || price < 0) {
    throw new Error('Price must be a non-negative number');
  }
  if (typeof discountPercent !== 'number' || 
      discountPercent < 0 || 
      discountPercent > 100) {
    throw new Error('Discount must be between 0 and 100');
  }
  return price * (discountPercent / 100);
}
```

### 16. Null Safety
- Avoid null/undefined when possible
- Use optional types or Maybe/Option patterns
- Check for null/undefined before use
- Use nullish coalescing and optional chaining
- Return empty collections instead of null

```javascript
// Good - null-safe with defaults
function getUserName(user) {
  return user?.profile?.name ?? 'Anonymous';
}

// Good - empty array instead of null
function getOrders(userId) {
  const orders = database.findOrders(userId);
  return orders || [];
}
```

## Testing

### 17. Test Coverage
- Write tests for all critical paths
- Aim for high test coverage (80%+ for critical code)
- Test edge cases and error conditions
- Write tests before fixing bugs (TDD)
- Keep tests maintainable and readable

### 18. Test Quality
- Tests should be fast and independent
- One assertion per test (generally)
- Use descriptive test names
- Follow AAA pattern (Arrange, Act, Assert)
- Mock external dependencies
- Don't test implementation details

```javascript
// Good - clear, focused test
describe('calculateOrderTotal', () => {
  it('should calculate total with tax and discount', () => {
    // Arrange
    const items = [{ price: 100 }, { price: 50 }];
    const taxRate = 0.08;
    const discount = 10;
    
    // Act
    const total = calculateOrderTotal(items, taxRate, discount);
    
    // Assert
    expect(total).toBe(151.2); // (150 - 10) * 1.08
  });
});
```

## Code Reviews

### 19. Review Checklist
- Code follows style guide and conventions
- Logic is clear and correct
- Error handling is appropriate
- Tests are adequate
- Performance considerations addressed
- Security implications reviewed
- Documentation is updated
- No unnecessary complexity

### 20. Review Process
- Review code promptly
- Provide constructive feedback
- Ask questions for clarification
- Suggest improvements, don't demand
- Approve when standards are met
- Use code review tools effectively

## Documentation

### 21. Code Documentation
- Document public APIs and interfaces
- Explain complex algorithms
- Document assumptions and constraints
- Include usage examples
- Keep documentation close to code
- Update docs with code changes

```javascript
/**
 * Calculates shipping cost based on weight and distance.
 * 
 * @param {number} weight - Package weight in kilograms
 * @param {number} distance - Shipping distance in kilometers
 * @param {string} serviceLevel - 'standard' or 'express'
 * @returns {number} Shipping cost in dollars
 * @throws {Error} If weight or distance is negative
 * 
 * @example
 * calculateShipping(5, 100, 'standard') // Returns 15.50
 */
function calculateShipping(weight, distance, serviceLevel) {
  // implementation
}
```

## Performance Considerations

### 22. Performance Best Practices
- Profile before optimizing
- Avoid premature optimization
- Use appropriate data structures
- Minimize I/O operations
- Cache expensive computations
- Use lazy loading when appropriate
- Optimize hot paths

### 23. Resource Management
- Close resources explicitly (files, connections, streams)
- Use connection pooling
- Implement proper cleanup in destructors/finally blocks
- Avoid resource leaks
- Monitor resource usage

## Security Considerations

### 24. Secure Coding
- Validate and sanitize all inputs
- Use parameterized queries for database access
- Implement proper authentication and authorization
- Don't expose sensitive information in logs or errors
- Use secure random number generators
- Keep dependencies updated
- Follow principle of least privilege

## Dependency Management

### 25. Dependencies
- Minimize external dependencies
- Keep dependencies updated
- Use lock files for reproducible builds
- Review dependency licenses
- Monitor for security vulnerabilities
- Prefer well-maintained libraries
- Document why dependencies are needed

## Version Control

### 26. Git Best Practices
- Write clear commit messages
- Make atomic commits (one logical change)
- Don't commit generated files or secrets
- Use .gitignore appropriately
- Review changes before committing
- Keep commits small and focused

```
// Good commit message
feat: Add user authentication with JWT

Implements JWT-based authentication for API endpoints.
Includes token refresh mechanism and proper error handling.

Closes #123
```

## Continuous Improvement

### 27. Code Metrics
- Monitor code complexity (cyclomatic complexity)
- Track code coverage
- Measure technical debt
- Use static analysis tools
- Review and act on code quality reports

### 28. Refactoring
- Refactor regularly to improve code quality
- Make small, incremental changes
- Ensure tests pass after refactoring
- Don't change behavior during refactoring
- Use refactoring tools when available

## Language-Specific Idioms

### 29. Use Language Features
- Use language-specific idioms and patterns
- Leverage standard library
- Follow community conventions
- Use modern language features appropriately
- Avoid anti-patterns

```javascript
// Good - uses modern JavaScript
const activeUsers = users.filter(user => user.isActive);
const userNames = users.map(user => user.name);
const hasAdmin = users.some(user => user.role === 'admin');

// Avoid - old style
const activeUsers = [];
for (let i = 0; i < users.length; i++) {
  if (users[i].isActive) {
    activeUsers.push(users[i]);
  }
}
```

## Tools and Automation

### 30. Quality Tools
- Use linters (ESLint, Pylint, RuboCop)
- Use formatters (Prettier, Black, gofmt)
- Use static analyzers (SonarQube, CodeClimate)
- Automate code quality checks in CI/CD
- Use IDE extensions for real-time feedback

### 31. Build Automation
- Automate builds and tests
- Use consistent build processes
- Run quality checks in CI/CD pipeline
- Fail builds on quality violations
- Generate quality reports

## Best Practices Summary

1. **Write readable code** - clarity over cleverness
2. **Keep it simple** - avoid unnecessary complexity
3. **Test thoroughly** - automated tests are essential
4. **Document wisely** - explain the why, not the what
5. **Review carefully** - code reviews catch issues early
6. **Refactor regularly** - pay down technical debt
7. **Follow standards** - consistency aids maintainability
8. **Think about security** - security is everyone's responsibility
9. **Consider performance** - but don't optimize prematurely
10. **Learn continuously** - improve skills and practices

## References
- Clean Code by Robert C. Martin
- Code Complete by Steve McConnell
- Refactoring by Martin Fowler
- The Pragmatic Programmer
- Design Patterns: Elements of Reusable Object-Oriented Software
