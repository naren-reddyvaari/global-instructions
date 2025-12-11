# Naming Conventions Guidelines

## Overview
Consistent naming conventions improve code readability, maintainability, and collaboration. This document establishes naming standards across all repositories.

## General Principles

### 1. Core Principles
- **Descriptive**: Names should clearly describe purpose or content
- **Consistent**: Follow the same patterns throughout the codebase
- **Concise**: Avoid unnecessary verbosity while maintaining clarity
- **Searchable**: Avoid single-letter names except in specific contexts
- **Pronounceable**: Names should be easy to say and discuss

### 2. Avoid
- Abbreviations unless widely recognized (HTTP, API, URL, ID)
- Encoding type information in names (Hungarian notation)
- Single-letter variables except for loop counters and mathematical formulas
- Ambiguous names that could mean multiple things
- Names that differ only by case

## Language-Specific Conventions

### JavaScript/TypeScript

#### Variables and Functions
```javascript
// camelCase for variables and functions
const userName = 'John';
const isUserActive = true;
function calculateTotalPrice() { }
async function fetchUserData() { }
```

#### Classes and Interfaces
```typescript
// PascalCase for classes and interfaces
class UserAccount { }
interface PaymentProcessor { }
type HttpResponse = { };
```

#### Constants
```javascript
// UPPER_SNAKE_CASE for constants
const MAX_RETRY_ATTEMPTS = 3;
const API_BASE_URL = 'https://api.example.com';
```

#### Private Members
```typescript
// Prefix with underscore (optional, prefer TypeScript private keyword)
class Service {
  private _cache: Map<string, any>;
  #privateField: string; // or use # for truly private fields
}
```

#### File Names
```
// kebab-case for files
user-service.ts
payment-processor.js
api-client.spec.ts
```

### Python

#### Variables and Functions
```python
# snake_case for variables and functions
user_name = 'John'
is_user_active = True
def calculate_total_price():
    pass
async def fetch_user_data():
    pass
```

#### Classes
```python
# PascalCase for classes
class UserAccount:
    pass
class PaymentProcessor:
    pass
```

#### Constants
```python
# UPPER_SNAKE_CASE for constants
MAX_RETRY_ATTEMPTS = 3
API_BASE_URL = 'https://api.example.com'
```

#### Private Members
```python
# Prefix with underscore for internal use
class Service:
    def __init__(self):
        self._cache = {}
    
    def _internal_method(self):
        pass
```

#### File Names
```
# snake_case for files
user_service.py
payment_processor.py
test_api_client.py
```

### Java/C#

#### Variables and Methods
```java
// camelCase for variables and methods
String userName = "John";
boolean isUserActive = true;
public void calculateTotalPrice() { }
```

#### Classes and Interfaces
```java
// PascalCase for classes and interfaces
public class UserAccount { }
public interface IPaymentProcessor { } // C# interface prefix
public interface PaymentProcessor { }  // Java - no prefix
```

#### Constants
```java
// UPPER_SNAKE_CASE for constants
public static final int MAX_RETRY_ATTEMPTS = 3;
public static final String API_BASE_URL = "https://api.example.com";
```

#### Packages/Namespaces
```java
// lowercase for packages
package com.company.project.module;

// PascalCase for C# namespaces
namespace Company.Project.Module
```

#### File Names
```
// PascalCase matching class name
UserService.java
PaymentProcessor.cs
```

### Go

#### Variables and Functions
```go
// camelCase for unexported, PascalCase for exported
var userName string
var IsUserActive bool // exported
func calculateTotalPrice() { } // unexported
func FetchUserData() { }       // exported
```

#### Interfaces
```go
// PascalCase, often with -er suffix
type Reader interface { }
type PaymentProcessor interface { }
```

#### Constants
```go
// camelCase or PascalCase (not UPPER_SNAKE_CASE)
const maxRetryAttempts = 3
const MaxRetryAttempts = 3 // exported
```

#### File Names
```
// snake_case for files
user_service.go
payment_processor.go
api_client_test.go
```

### Ruby

#### Variables and Methods
```ruby
# snake_case for variables and methods
user_name = 'John'
is_user_active = true
def calculate_total_price
end
```

#### Classes and Modules
```ruby
# PascalCase for classes and modules
class UserAccount
end
module PaymentProcessor
end
```

#### Constants
```ruby
# UPPER_SNAKE_CASE for constants
MAX_RETRY_ATTEMPTS = 3
API_BASE_URL = 'https://api.example.com'
```

#### File Names
```
# snake_case for files
user_service.rb
payment_processor.rb
api_client_spec.rb
```

## Database Conventions

### Tables
```sql
-- snake_case plural for table names
users
product_orders
payment_transactions
```

### Columns
```sql
-- snake_case for column names
user_id
first_name
created_at
is_active
```

### Indexes
```sql
-- descriptive names with idx_ prefix
idx_users_email
idx_orders_user_id_created_at
```

### Constraints
```sql
-- descriptive names with prefix
pk_users            -- primary key
fk_orders_user_id   -- foreign key
uk_users_email      -- unique key
ck_users_age        -- check constraint
```

## API Conventions

### REST Endpoints
```
// kebab-case, plural nouns for collections
GET    /api/users
GET    /api/users/{id}
POST   /api/users
PUT    /api/users/{id}
DELETE /api/users/{id}

// Nested resources
GET    /api/users/{id}/orders
POST   /api/users/{id}/payment-methods
```

### Query Parameters
```
// camelCase or snake_case (be consistent)
?page=1
?pageSize=20
?sortBy=createdAt
?orderDirection=desc
?includeDeleted=false
```

### Request/Response Fields
```json
// camelCase for JSON fields
{
  "userId": 123,
  "firstName": "John",
  "lastName": "Doe",
  "isActive": true,
  "createdAt": "2024-01-01T00:00:00Z"
}
```

### Headers
```
// Kebab-Case for custom headers
X-Request-ID: 123
X-API-Key: secret
X-Rate-Limit: 100
```

## Environment Variables

```bash
# UPPER_SNAKE_CASE for environment variables
DATABASE_URL=postgresql://localhost/db
API_KEY=secret123
MAX_CONNECTIONS=100
NODE_ENV=production
```

## Configuration Files

```
# kebab-case for config files
app-config.json
database-config.yml
.eslintrc.js
docker-compose.yml
```

## Git Conventions

### Branch Names
```
# kebab-case with type prefix
feature/user-authentication
bugfix/fix-login-error
hotfix/security-patch
release/v1.2.0
```

### Commit Messages
```
# Imperative mood, capitalize first letter
Add user authentication feature
Fix login validation bug
Update dependencies to latest versions
Refactor payment processing logic
```

### Tag Names
```
# Semantic versioning
v1.0.0
v1.2.3-beta
v2.0.0-rc.1
```

## Boolean Naming

### Prefix with is, has, can, should
```javascript
// Good
isActive
isVisible
hasPermission
hasAccess
canEdit
canDelete
shouldUpdate
shouldRetry

// Avoid
active      // ambiguous
visible     // could be verb
permission  // not clearly boolean
```

## Collection Naming

### Use Plural for Collections
```javascript
// Good
const users = [];
const orderItems = [];
function getProducts() { }

// Avoid
const userList = [];    // redundant
const arrayOfOrders = []; // redundant
```

## Function Naming

### Use Verbs for Functions
```javascript
// Good - action verbs
function createUser() { }
function updateOrder() { }
function deleteItem() { }
function calculateTotal() { }
function validateInput() { }
function fetchData() { }
function transformResponse() { }

// Good - getter/setter
function getUser() { }
function setPassword() { }

// Good - boolean returns
function isValid() { }
function hasPermission() { }
function canAccess() { }
```

## Event Naming

### Use Past Tense for Events
```javascript
// Good
userCreated
orderUpdated
paymentProcessed
fileUploaded
dataFetched

// Component events
onUserCreated
onOrderUpdated
handleButtonClick
handleFormSubmit
```

## Test Naming

### Descriptive Test Names
```javascript
// Good - describes what is being tested
describe('UserService', () => {
  it('should create a new user with valid data', () => {});
  it('should throw error when email is invalid', () => {});
  it('should return user by ID', () => {});
});

// Python
def test_create_user_with_valid_data():
    pass

def test_throws_error_when_email_is_invalid():
    pass
```

## Package/Module Naming

### Descriptive Package Names
```
// Good
user-authentication
payment-processing
data-validation
api-client

// Avoid
utils          // too generic
helpers        // too generic
misc           // too vague
```

## Acronyms and Abbreviations

### Commonly Accepted
```
API   - Application Programming Interface
HTTP  - Hypertext Transfer Protocol
URL   - Uniform Resource Locator
ID    - Identifier
JSON  - JavaScript Object Notation
XML   - Extensible Markup Language
SQL   - Structured Query Language
UI    - User Interface
DB    - Database
I/O   - Input/Output
```

### Casing in Identifiers
```javascript
// Good
const apiUrl = 'https://api.example.com';
const userId = 123;
class HttpClient { }

// Avoid
const aPIURL = '...';  // inconsistent casing
const userID = 123;    // prefer Id
```

## Documentation

### Clear and Consistent
```javascript
/**
 * Calculates the total price including tax
 * @param {number} subtotal - The subtotal amount
 * @param {number} taxRate - The tax rate as decimal (e.g., 0.08 for 8%)
 * @returns {number} The total price including tax
 */
function calculateTotalPrice(subtotal, taxRate) {
  return subtotal * (1 + taxRate);
}
```

## References
- Language-specific style guides (Google, Airbnb, etc.)
- Clean Code by Robert C. Martin
- Code Complete by Steve McConnell
