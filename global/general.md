# General Coding Guidelines

## 1. Code Readability
- Write clear and simple code.
- Break long methods into smaller logical units.
- Use meaningful variable, method, and class names.
- Add comments only when necessary—prefer self-explanatory code.
- Follow a consistent indentation style.

## 2. Code Structure
- Organize packages logically (controller, service, repository, config, util).
- Keep classes focused on a single responsibility.
- Avoid large classes (God classes).
- Ensure code follows SOLID principles.

## 3. Error Handling
- Handle exceptions gracefully; avoid swallowing exceptions.
- Throw specific exceptions instead of generic ones.
- Use custom exceptions where meaningful.
- Provide meaningful error messages.

## 4. Logging Practices
- Use structured and meaningful logs.
- Avoid logging sensitive data.
- Use appropriate log levels: DEBUG, INFO, WARN, ERROR.
- Do not use string concatenation in logs—use placeholders (e.g., "{}") unless performance tested.

## 5. Code Quality
- Remove unused imports, variables, and dead code.
- Avoid duplicate logic; extract reusable methods.
- Keep methods short (ideal: <40 lines).
- Prefer immutability wherever possible.

## 6. Testing
- Write unit tests for business logic.
- Use integration tests for API, DB, and workflow validation.
- Follow AAA pattern (Arrange–Act–Assert).

## 7. Documentation
- Add JavaDoc to public methods, complex logic, and APIs.
- Maintain README with setup instructions.
- Document architectural decisions and patterns used.

## 8. Code Reviews
- Ensure code passes all automated checks.
- Keep PRs small and focused on one change.
- Provide meaningful review comments.
- Accept feedback gracefully and revise code accordingly.

## 9. Dependency Management
- Use the latest stable library versions.
- Remove unused dependencies.
- Avoid heavy libraries for simple tasks.

## 10. Build & Deployment
- Ensure build is reproducible and deterministic.
- Keep configurations externalized.
- Validate environment variables before use.

---
These guidelines apply across Java, Spring Boot, and general backend engineering practices.