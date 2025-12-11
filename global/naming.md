# Naming Conventions

A clear and consistent naming strategy improves readability, maintainability, and scalability of your codebase. Follow these guidelines across all projects.

---

## 1. General Principles
- Names should be **meaningful**, **self-explanatory**, and **consistent**.
- Avoid abbreviations unless they are **well-known** (e.g., `URL`, `ID`, `HTML`).
- Use **English** for all identifiers.
- Prefer **full words** over short cryptic names.
- Reflect the **intent and purpose** of the variable, method, or class.

---

## 2. Case Conventions

### **Camel Case (camelCase)**
Used for:
- Variables: `employeeName`, `totalAmount`
- Method names: `calculateTax()`, `getEmployeeDetails()`
- Function parameters: `orderId`, `statusFlag`

### **Pascal Case (PascalCase)**
Used for:
- Class names: `EmployeeService`, `OrderController`
- Enum names: `OrderStatus`, `PaymentMode`
- DTOs, models, components

### **Snake Case (snake_case)**
Used for:
- Database columns: `employee_id`, `created_at`
- Environment variable names: `DB_HOST`, `API_KEY`

### **Upper Case (UPPER_CASE)**
Used for:
- Constants: `MAX_RETRY_COUNT`, `DEFAULT_TIMEOUT`
- Enum values: `PENDING`, `FAILED`, `SUCCESS`

---

## 3. Naming Rules by Category

### **3.1 Variables**
- Must reflect the stored value: `customerAge`, not `ca`.
- Avoid generic names like `data`, `value`, `temp`.
- Booleans should read like questions: `isActive`, `hasPermission`, `canRetry`.

### **3.2 Methods**
- Must start with a **verb**: `saveOrder()`, `updateProfile()`.
- For boolean methods use prefixes: `is`, `has`, `can`, `should`.
- Use descriptive names even if long.

### **3.3 Classes and Interfaces**
- Class names should be nouns: `OrderProcessor`, `PaymentValidator`.
- Interface names can describe capability: `Serializable`, `Persistable`.
- Avoid suffixes like `Manager`, `Util`—be specific.

### **3.4 Packages**
- Always lowercase: `com.example.orderservice.controller`.
- Use layered naming: `controller`, `service`, `repository`, etc.

### **3.5 Constants**
- Must be **UPPER_SNAKE_CASE**.
- Should reflect the meaning, not the usage context.
- Example:
  - `MIN_PASSWORD_LENGTH`
  - `JWT_EXPIRATION_MS`

### **3.6 Enums**
- Enum name: PascalCase → `OrderType`
- Enum values: UPPER_CASE → `ONLINE`, `IN_STORE`

---

## 4. Special Naming Guidelines

### **Collections**
- Use plural names: `employees`, `activeOrders`.
- For maps include key/value hint: `employeeById`, `orderByCustomer`.

### **DTOs / Entities**
- End DTO classes with `Request`, `Response`, `DTO` based on usage.

### **Async Methods**
- Suffix with `Async`: `sendEmailAsync()`.

### **Event Names**
- Use past tense for events: `OrderCreatedEvent`, `PaymentFailedEvent`.

---

## 5. File & Directory Names
- Match class name exactly.
- Configuration files should be meaningful: `application-dev.yml`, `logging-config.xml`.
- Markdown documentation: `coding-guidelines.md`, `api-contracts.md`.

---

## 6. REST API Naming Conventions
- Resource names must be **plural**: `/api/v1/orders`
- No verbs in URL.
- Use kebab-case for paths: `/employee-details/{id}`.
- Standard HTTP verbs:
  - GET `/orders`
  - POST `/orders`
  - PUT `/orders/{id}`
  - DELETE `/orders/{id}`

---

## 7. Database Naming
- Tables: plural nouns → `employees`, `orders`
- Columns: snake_case → `created_at`, `employee_name`
- Primary keys: `id` or `table_name_id`
- Foreign keys: `employee_id`, `order_id`

---

## 8. Names to Avoid
- Single-letter names (except loop counters like `i`, `j`).
- Too generic names: `helper`, `util`, `manager`.
- Ambiguous names: `obj`, `flag`, `stuff`.
- Redundant prefixes: `theEmployee`, `myService`.

---

## 9. Naming Checklist
Before finalizing a name, ask:
- Does it reflect the **true purpose**?
- Is it **consistent** with the conventions?
- Will another developer understand it immediately?
- Is the length appropriate?

---

If you'd like, I can now generate:
✔️ **3. Performance Guidelines**
✔️ **4. Security Guidelines**