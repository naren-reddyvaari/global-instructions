# Testing Best Practices Guidelines

## Overview
This document establishes comprehensive testing standards to ensure software quality, reliability, and maintainability across all repositories.

## Testing Philosophy

### 1. Testing Principles
- **Write tests first** (Test-Driven Development when applicable)
- **Test behavior, not implementation**
- **Keep tests simple and readable**
- **Tests should be fast and independent**
- **Maintain tests like production code**
- **Aim for high test coverage on critical paths**

### 2. Testing Pyramid
```
        /\
       /  \      E2E Tests (Few)
      /____\     - Slow, expensive
     /      \    - Test complete workflows
    /________\   
   /          \  Integration Tests (Some)
  /____________\ - Test component interactions
 /              \
/________________\ Unit Tests (Many)
                   - Fast, isolated
                   - Test individual units
```

## Unit Testing

### 3. Unit Test Characteristics
- Test individual functions/methods in isolation
- Fast execution (milliseconds)
- No external dependencies (database, network, file system)
- Use mocks/stubs for dependencies
- One logical assertion per test
- Test one thing at a time

### 4. Unit Test Structure (AAA Pattern)
```javascript
// Arrange - Set up test data and dependencies
// Act - Execute the code under test
// Assert - Verify the expected outcome

describe('calculateDiscount', () => {
  it('should apply 10% discount for orders over $100', () => {
    // Arrange
    const orderTotal = 150;
    const discountRate = 0.10;
    
    // Act
    const result = calculateDiscount(orderTotal, discountRate);
    
    // Assert
    expect(result).toBe(15);
  });
});
```

### 5. What to Unit Test
- Core business logic
- Edge cases and boundary conditions
- Error handling
- Input validation
- Calculations and transformations
- Conditional logic
- Helper functions and utilities

### 6. Unit Test Best Practices
```javascript
// Good - descriptive test name
it('should throw error when email format is invalid', () => {
  expect(() => validateEmail('invalid-email'))
    .toThrow('Invalid email format');
});

// Good - testing edge cases
it('should return empty array when no items match filter', () => {
  const items = [{ status: 'active' }];
  const result = filterItems(items, 'inactive');
  expect(result).toEqual([]);
});

// Good - testing error conditions
it('should handle null input gracefully', () => {
  expect(() => processData(null))
    .toThrow('Input cannot be null');
});

// Avoid - testing implementation details
it('should call private method twice', () => {
  // Don't test internal implementation
});
```

## Integration Testing

### 7. Integration Test Characteristics
- Test interactions between components
- May involve database, file system, or external services
- Slower than unit tests
- Test realistic scenarios
- Verify data flow between components

### 8. Integration Test Scope
```javascript
// Test database integration
describe('UserRepository', () => {
  beforeEach(async () => {
    await database.clear();
  });

  it('should save and retrieve user from database', async () => {
    // Arrange
    const user = { name: 'John', email: 'john@example.com' };
    
    // Act
    await userRepository.save(user);
    const retrieved = await userRepository.findByEmail('john@example.com');
    
    // Assert
    expect(retrieved.name).toBe('John');
    expect(retrieved.email).toBe('john@example.com');
  });
});

// Test API integration
describe('Payment API', () => {
  it('should process payment successfully', async () => {
    const payment = {
      amount: 100,
      currency: 'USD',
      method: 'credit_card'
    };
    
    const response = await paymentService.processPayment(payment);
    
    expect(response.status).toBe('success');
    expect(response.transactionId).toBeDefined();
  });
});
```

### 9. Integration Test Best Practices
- Use test databases or containers
- Clean up test data after each test
- Use realistic test data
- Test happy paths and error scenarios
- Mock external services when appropriate
- Use test fixtures for consistent data

## End-to-End (E2E) Testing

### 10. E2E Test Characteristics
- Test complete user workflows
- Test from user perspective
- Slowest tests
- Most comprehensive
- Run against production-like environment

### 11. E2E Test Examples
```javascript
// Web application E2E test
describe('User Registration Flow', () => {
  it('should allow new user to register and login', async () => {
    // Navigate to registration page
    await page.goto('/register');
    
    // Fill registration form
    await page.fill('[name="email"]', 'newuser@example.com');
    await page.fill('[name="password"]', 'SecurePass123');
    await page.fill('[name="confirmPassword"]', 'SecurePass123');
    
    // Submit form
    await page.click('button[type="submit"]');
    
    // Verify redirect to dashboard
    await expect(page).toHaveURL('/dashboard');
    await expect(page.locator('h1')).toContainText('Welcome');
  });
});

// API E2E test
describe('Order Processing Workflow', () => {
  it('should create order, process payment, and send confirmation', async () => {
    // Create order
    const order = await createOrder({ items: [{ id: 1, quantity: 2 }] });
    expect(order.status).toBe('pending');
    
    // Process payment
    const payment = await processPayment(order.id, { method: 'card' });
    expect(payment.status).toBe('completed');
    
    // Verify order updated
    const updatedOrder = await getOrder(order.id);
    expect(updatedOrder.status).toBe('confirmed');
    
    // Verify email sent
    const emails = await getTestEmails();
    expect(emails).toContainEqual(
      expect.objectContaining({
        to: order.customerEmail,
        subject: expect.stringContaining('Order Confirmation')
      })
    );
  });
});
```

### 12. E2E Test Best Practices
- Focus on critical user journeys
- Keep tests stable and maintainable
- Use page object pattern for UI tests
- Run in isolated environment
- Use proper wait strategies (not sleep)
- Implement retry logic for flaky tests
- Run on multiple browsers/devices

## Test Data Management

### 13. Test Data Strategies
```javascript
// Use factories for test data
const userFactory = {
  create: (overrides = {}) => ({
    id: generateId(),
    name: 'Test User',
    email: 'test@example.com',
    role: 'user',
    ...overrides
  })
};

// Use in tests
const adminUser = userFactory.create({ role: 'admin' });
const activeUser = userFactory.create({ isActive: true });

// Use fixtures for consistent data
const testFixtures = {
  users: [
    { id: 1, name: 'User 1', email: 'user1@example.com' },
    { id: 2, name: 'User 2', email: 'user2@example.com' }
  ],
  orders: [
    { id: 1, userId: 1, total: 100, status: 'completed' }
  ]
};
```

### 14. Test Data Best Practices
- Use meaningful test data
- Keep test data minimal
- Avoid hardcoded IDs (use factories)
- Clean up test data after tests
- Use realistic data for edge cases
- Don't share mutable test data between tests

## Mocking and Stubbing

### 15. When to Mock
- External services (APIs, databases)
- Time-dependent code
- Random number generation
- File system operations
- Network calls
- Complex dependencies

### 16. Mocking Examples
```javascript
// Mock external API
jest.mock('./apiClient');
apiClient.fetchUser.mockResolvedValue({
  id: 1,
  name: 'John Doe'
});

// Mock date/time
jest.useFakeTimers();
jest.setSystemTime(new Date('2024-01-01'));

// Spy on functions
const mockCallback = jest.fn();
processItems(items, mockCallback);
expect(mockCallback).toHaveBeenCalledTimes(3);

// Stub database calls
sinon.stub(database, 'query').resolves([{ id: 1, name: 'Test' }]);
```

### 17. Mocking Best Practices
- Mock at the boundary (not deep in code)
- Verify mock behavior
- Reset mocks between tests
- Don't over-mock (integration tests should use real components)
- Use appropriate mocking level for test type

## Test Coverage

### 18. Coverage Metrics
- **Line Coverage**: Percentage of code lines executed
- **Branch Coverage**: Percentage of decision branches executed
- **Function Coverage**: Percentage of functions called
- **Statement Coverage**: Percentage of statements executed

### 19. Coverage Goals
- Aim for 80%+ coverage on critical code
- 100% coverage on security-critical code
- Lower coverage acceptable for UI/presentation layers
- Focus on meaningful coverage, not just numbers
- Use coverage to identify untested code

### 20. Coverage Tools
```bash
# JavaScript/TypeScript
npm test -- --coverage

# Python
pytest --cov=src --cov-report=html

# Java
mvn test jacoco:report

# Go
go test -coverprofile=coverage.out
go tool cover -html=coverage.out
```

## Test Organization

### 21. Test File Structure
```
src/
  services/
    user.service.js
    user.service.test.js       # Co-located tests
    payment.service.js
    payment.service.test.js

tests/
  unit/                        # Or separate test directory
    user.service.test.js
  integration/
    api.test.js
  e2e/
    user-registration.test.js
```

### 22. Test Naming Conventions
```javascript
// Descriptive test names
describe('UserService', () => {
  describe('createUser', () => {
    it('should create user with valid data', () => {});
    it('should throw error when email is invalid', () => {});
    it('should hash password before saving', () => {});
  });
  
  describe('deleteUser', () => {
    it('should delete user and related data', () => {});
    it('should throw error when user not found', () => {});
  });
});

// Alternative naming style
test('createUser: should create user with valid data', () => {});
test('createUser: should throw error when email is invalid', () => {});
```

## Testing Anti-Patterns

### 23. Avoid These Patterns
```javascript
// Avoid - testing multiple things
it('should create, update, and delete user', () => {
  // Too much in one test
});

// Avoid - test depends on another test
it('should create user', () => {
  userId = createUser();
});
it('should update user', () => {
  updateUser(userId); // Depends on previous test
});

// Avoid - unclear expectations
it('should work', () => {
  const result = doSomething();
  expect(result).toBeTruthy(); // What does "work" mean?
});

// Avoid - testing implementation
it('should call private method', () => {
  // Don't test internal implementation
});

// Avoid - fragile tests
it('should display welcome message', () => {
  expect(element.textContent).toBe('Welcome, John Doe!'); // Breaks if text changes slightly
});
```

## Async Testing

### 24. Testing Async Code
```javascript
// Using async/await
it('should fetch user data', async () => {
  const user = await fetchUser(1);
  expect(user.name).toBe('John');
});

// Using promises
it('should fetch user data', () => {
  return fetchUser(1).then(user => {
    expect(user.name).toBe('John');
  });
});

// Testing rejections
it('should handle fetch error', async () => {
  await expect(fetchUser(999))
    .rejects
    .toThrow('User not found');
});

// Testing with callbacks
it('should process data with callback', (done) => {
  processData((result) => {
    expect(result).toBe('success');
    done();
  });
});
```

## Performance Testing

### 25. Load Testing
```javascript
// Test response time
it('should respond within 200ms', async () => {
  const start = Date.now();
  await apiCall();
  const duration = Date.now() - start;
  expect(duration).toBeLessThan(200);
});

// Benchmark tests
describe('Performance', () => {
  it('should handle 1000 concurrent requests', async () => {
    const requests = Array(1000).fill().map(() => apiCall());
    const results = await Promise.all(requests);
    expect(results.every(r => r.status === 200)).toBe(true);
  });
});
```

## Security Testing

### 26. Security Test Examples
```javascript
// Test input validation
it('should reject SQL injection attempts', () => {
  const maliciousInput = "'; DROP TABLE users; --";
  expect(() => processInput(maliciousInput))
    .toThrow('Invalid input');
});

// Test authentication
it('should reject request without valid token', async () => {
  const response = await fetch('/api/protected', {
    headers: { 'Authorization': 'Bearer invalid' }
  });
  expect(response.status).toBe(401);
});

// Test authorization
it('should prevent user from accessing admin resources', async () => {
  const response = await fetch('/api/admin/users', {
    headers: { 'Authorization': `Bearer ${userToken}` }
  });
  expect(response.status).toBe(403);
});
```

## Continuous Integration

### 27. CI/CD Testing
- Run tests on every commit
- Run full test suite before merging
- Fail build on test failures
- Generate and publish test reports
- Track test trends over time
- Run tests in parallel when possible

### 28. Test Environment
- Use consistent test environment
- Use containers for reproducibility
- Seed databases with test data
- Use feature flags for testing
- Test in production-like environment

## Test Maintenance

### 29. Keeping Tests Healthy
- Remove obsolete tests
- Update tests when code changes
- Refactor tests for clarity
- Fix flaky tests immediately
- Review test coverage regularly
- Keep tests fast

### 30. Test Documentation
```javascript
/**
 * Tests the user authentication flow
 * 
 * Verifies:
 * - Valid credentials allow login
 * - Invalid credentials are rejected
 * - Account lockout after failed attempts
 * - Session creation and management
 */
describe('User Authentication', () => {
  // tests
});
```

## Testing Tools

### 31. Common Testing Frameworks
- **JavaScript**: Jest, Mocha, Jasmine, Vitest
- **Python**: pytest, unittest, nose2
- **Java**: JUnit, TestNG, Mockito
- **Go**: testing package, testify
- **Ruby**: RSpec, Minitest
- **C#**: NUnit, xUnit, MSTest

### 32. Additional Tools
- **Mocking**: Sinon, Jest mocks, mockito
- **E2E**: Playwright, Cypress, Selenium
- **API Testing**: Postman, REST Assured, SuperTest
- **Load Testing**: JMeter, k6, Locust
- **Coverage**: Istanbul, Coverage.py, JaCoCo

## Best Practices Summary

1. **Write tests early and often**
2. **Keep tests simple and focused**
3. **Test behavior, not implementation**
4. **Maintain high coverage on critical code**
5. **Make tests fast and independent**
6. **Use descriptive test names**
7. **Follow the testing pyramid**
8. **Mock external dependencies**
9. **Clean up test data**
10. **Treat tests as production code**

## References
- Test-Driven Development by Kent Beck
- Growing Object-Oriented Software, Guided by Tests
- xUnit Test Patterns
- The Art of Unit Testing
