# API Testing & Postman — SDET Knowledge Book

> **Purpose:** Quick revision for SDET interviews and practical API testing.
> **Focus:** API Testing + REST + HTTP + Postman + Automation concepts.

---

# 1. API Testing

## API

* **API** → Application Programming Interface
* Allows two applications/systems to communicate.
* API testing validates:

  * Request
  * Response
  * Business logic
  * Data
  * Status codes
  * Headers
  * Authentication
  * Performance

## API Testing Advantages

* Faster than UI testing
* More stable than UI testing
* Can test backend before UI
* Easy automation
* Validates business logic directly
* Useful for integration testing

## API Testing Layers

```text
Request
   ↓
Authentication
   ↓
Business Logic
   ↓
Database / External Services
   ↓
Response
```

---

# 2. API Terminology

| Term        | Meaning                                 |
| ----------- | --------------------------------------- |
| API         | Interface for application communication |
| Endpoint    | Specific API URL                        |
| Request     | Data sent to server                     |
| Response    | Data returned by server                 |
| Resource    | Entity exposed by API                   |
| Payload     | Data sent in request body               |
| Header      | Metadata about request/response         |
| Parameter   | Dynamic value passed to API             |
| Token       | Credential used for authentication      |
| Status Code | Result of HTTP request                  |

Example:

```http
GET https://api.example.com/users/101
```

* `https://api.example.com` → Base URL
* `/users/101` → Endpoint path
* `101` → Path parameter

---

# 3. REST API

## REST

**REST → Representational State Transfer**

REST is an architectural style for building web APIs.

### REST Characteristics

* Client-Server
* Stateless
* Cacheable
* Uniform Interface
* Layered System
* Resource-based

### REST API Common Format

```http
METHOD /resource/{id}
```

Example:

```http
GET /users/101
```

---

# 4. REST vs SOAP

| REST                  | SOAP                                |
| --------------------- | ----------------------------------- |
| Architectural style   | Protocol                            |
| Usually JSON          | Usually XML                         |
| Lightweight           | Heavyweight                         |
| Faster                | Relatively slower                   |
| HTTP commonly used    | HTTP, SMTP, etc.                    |
| Easy to use           | More complex                        |
| Common in modern APIs | Common in enterprise/legacy systems |

---

# 5. HTTP Methods

## GET

Retrieve data.

```http
GET /users
GET /users/101
```

* Should not modify server data.
* Usually no request body.

## POST

Create a resource.

```http
POST /users
```

```json
{
  "name": "Raj",
  "email": "raj@example.com"
}
```

## PUT

Complete update/replacement of a resource.

```http
PUT /users/101
```

## PATCH

Partial update.

```http
PATCH /users/101
```

```json
{
  "email": "new@example.com"
}
```

## DELETE

Delete resource.

```http
DELETE /users/101
```

## HEAD

Same as GET but returns headers without response body.

## OPTIONS

Returns supported communication methods/options.

---

# 6. PUT vs PATCH

| PUT                             | PATCH                  |
| ------------------------------- | ---------------------- |
| Full update                     | Partial update         |
| Usually sends complete resource | Sends changed fields   |
| Replacement semantics           | Modification semantics |

---

# 7. HTTP Status Codes

## 1xx — Informational

* `100` Continue

## 2xx — Success

* `200` OK
* `201` Created
* `202` Accepted
* `204` No Content

## 3xx — Redirection

* `301` Moved Permanently
* `302` Found
* `304` Not Modified

## 4xx — Client Error

* `400` Bad Request
* `401` Unauthorized
* `403` Forbidden
* `404` Not Found
* `405` Method Not Allowed
* `409` Conflict
* `415` Unsupported Media Type
* `422` Unprocessable Content
* `429` Too Many Requests

## 5xx — Server Error

* `500` Internal Server Error
* `501` Not Implemented
* `502` Bad Gateway
* `503` Service Unavailable
* `504` Gateway Timeout

### Important

```text
401 → Authentication problem
403 → Authorization/permission problem
404 → Resource not found
400 → Invalid request
409 → Conflict
500 → Server-side error
```

---

# 8. HTTP Request

A request can contain:

```text
Method
URL
Headers
Parameters
Body
Authentication
```

Example:

```http
POST /users
Content-Type: application/json
Authorization: Bearer <token>
```

```json
{
  "name": "Raj",
  "age": 24
}
```

---

# 9. HTTP Response

Response commonly contains:

```text
Status Code
Headers
Response Body
Response Time
```

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "id": 101,
  "name": "Raj"
}
```

---

# 10. Parameters

## Path Parameter

Identifies a specific resource.

```http
GET /users/101
```

`101` → Path parameter

## Query Parameter

Used for filtering/searching/sorting/pagination.

```http
GET /users?page=2&limit=10
```

## Request Body

Used to send data.

```json
{
  "name": "Raj"
}
```

### Quick Comparison

```text
/users/101
       ↑
Path Parameter

/users?page=2
       ↑
Query Parameter
```

---

# 11. Headers

Headers provide metadata.

### Common Request Headers

```text
Content-Type
Accept
Authorization
User-Agent
Cache-Control
```

### Content-Type

Specifies request body format.

```http
Content-Type: application/json
```

### Accept

Specifies expected response format.

```http
Accept: application/json
```

### Authorization

```http
Authorization: Bearer <token>
```

---

# 12. Content-Type vs Accept

| Content-Type              | Accept                    |
| ------------------------- | ------------------------- |
| Format being sent         | Format expected           |
| Request body              | Response                  |
| Example: application/json | Example: application/json |

---

# 13. Authentication vs Authorization

### Authentication

**Who are you?**

Example:

```text
Username + Password
Token
JWT
```

### Authorization

**What are you allowed to access?**

Example:

```text
Admin → Delete users
User → View users
```

---

# 14. API Authentication

Common methods:

* Basic Authentication
* API Key
* Bearer Token
* OAuth 2.0
* JWT

## Basic Auth

```text
Username + Password
```

## API Key

```http
x-api-key: abc123
```

## Bearer Token

```http
Authorization: Bearer <token>
```

---

# 15. JWT

**JWT → JSON Web Token**

Structure:

```text
Header.Payload.Signature
```

Example:

```text
xxxxx.yyyyy.zzzzz
```

JWT is commonly used for stateless authentication.

---

# 16. JSON

JSON:

```json
{
  "id": 101,
  "name": "Raj",
  "skills": [
    "Java",
    "Selenium",
    "API Testing"
  ]
}
```

### JSON Data Types

```text
String
Number
Boolean
Object
Array
Null
```

---

# 17. API Test Scenarios

For every API, validate:

### Request

* HTTP method
* URL
* Path parameters
* Query parameters
* Headers
* Authentication
* Request body

### Response

* Status code
* Response body
* Response schema
* Headers
* Response time
* Data correctness

### Negative Testing

Test:

* Missing required field
* Invalid data
* Invalid token
* Expired token
* Invalid endpoint
* Invalid HTTP method
* Empty request
* Boundary values
* Duplicate data
* Incorrect content type

---

# 18. CRUD Testing

```text
Create → POST
Read   → GET
Update → PUT / PATCH
Delete → DELETE
```

Typical flow:

```text
POST User
   ↓
GET User
   ↓
PUT/PATCH User
   ↓
GET User
   ↓
DELETE User
   ↓
GET User → 404
```

---

# 19. API Validation

## Status Code Validation

```text
Expected: 200
Actual:   200
```

## Response Body Validation

Check:

```text
Required fields
Values
Data types
Business rules
Nested objects
Arrays
```

## Header Validation

Check:

```text
Content-Type
Authorization
Cache-Control
Correlation ID
```

## Response Time

Example:

```text
Response time < 2 seconds
```

## Schema Validation

Validate:

```text
Field names
Data types
Required fields
Nested structure
```

---

# 20. Postman

Postman is an API development and testing tool.

Used for:

* API requests
* API testing
* Collections
* Environment management
* Automation
* Mocking
* Monitoring
* Documentation

---

# 21. Postman Collection

Collection = Group of API requests.

Example:

```text
User API Collection
│
├── Create User
├── Get User
├── Update User
└── Delete User
```

---

# 22. Postman Environment

Environment stores reusable variables.

Example:

```text
baseUrl = https://qa.example.com
token = abc123
userId = 101
```

Use:

```text
{{baseUrl}}/users/{{userId}}
```

---

# 23. Postman Variables

### Variable Scopes

```text
Global
Collection
Environment
Data
Local
```

### Priority

```text
Local
  ↓
Data
  ↓
Environment
  ↓
Collection
  ↓
Global
```

When scopes overlap, the narrower/higher-precedence scope is used.

---

# 24. Dynamic Variables

Postman provides dynamic variables for test data.

Examples:

```text
{{$randomUUID}}
{{$randomEmail}}
{{$randomFirstName}}
{{$randomInt}}
```

Useful for:

* Unique data
* Random test data
* Avoiding duplicate records

---

# 25. Postman Pre-request Script

Runs **before** request execution.

Used for:

* Generate token
* Generate dynamic data
* Set variables
* Create timestamps
* Prepare request data

Example:

```javascript
pm.environment.set("userId", "101");
```

Flow:

```text
Pre-request Script
        ↓
Request
```

---

# 26. Postman Test Script

Runs **after** response is received.

Used for:

* Assertions
* Response validation
* Extracting values
* Saving variables
* Chaining APIs

Example:

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

---

# 27. Pre-request vs Test Script

| Pre-request    | Test                  |
| -------------- | --------------------- |
| Before request | After response        |
| Prepare data   | Validate response     |
| Generate token | Validate status       |
| Set variables  | Extract response data |

---

# 28. Postman Assertions

Common validations:

```javascript
pm.response.to.have.status(200);
```

```javascript
pm.expect(pm.response.json().name).to.eql("Raj");
```

```javascript
pm.expect(pm.response.responseTime).to.be.below(2000);
```

---

# 29. Extract Response Data

Example response:

```json
{
  "id": 101,
  "token": "abc123"
}
```

Extract:

```javascript
const response = pm.response.json();

pm.environment.set("userId", response.id);
pm.environment.set("token", response.token);
```

Use later:

```text
{{userId}}
{{token}}
```

---

# 30. API Chaining

Use response data from one API in another API.

Example:

```text
Login API
   ↓
Extract Token
   ↓
Create User
   ↓
Extract User ID
   ↓
Get User
   ↓
Update User
   ↓
Delete User
```

This is called **API chaining / correlation**.

---

# 31. Collection Runner

Used to execute multiple requests.

Supports:

* Multiple requests
* Iterations
* Data files
* Environment variables
* Test execution
* Test results

Data sources:

```text
CSV
JSON
```

---

# 32. Data-Driven Testing

Example CSV:

```csv
username,password
user1,password1
user2,password2
user3,password3
```

Use variables:

```text
{{username}}
{{password}}
```

Each iteration uses different test data.

---

# 33. Newman

**Newman → Command-line collection runner for Postman**

Useful for:

* CI/CD
* Jenkins
* Azure DevOps
* GitHub Actions

Example:

```bash
newman run collection.json
```

With environment:

```bash
newman run collection.json -e environment.json
```

---

# 34. Postman CI/CD Flow

```text
Git
 ↓
CI/CD Pipeline
 ↓
Newman
 ↓
Postman Collection
 ↓
API Tests
 ↓
Test Report
```

---

# 35. API Testing vs UI Testing

| API Testing              | UI Testing                |
| ------------------------ | ------------------------- |
| Backend                  | Frontend                  |
| Faster                   | Slower                    |
| More stable              | More fragile              |
| Less maintenance         | Higher maintenance        |
| Validates business logic | Validates user experience |
| No browser required      | Browser usually required  |

---

# 36. Common API Testing Questions

### What is API testing?

Testing APIs by validating requests, responses, business logic, data, authentication, status codes and performance.

### What is an endpoint?

A specific URL through which an API resource/function can be accessed.

### PUT vs PATCH?

```text
PUT   → Full update
PATCH → Partial update
```

### 401 vs 403?

```text
401 → Authentication required/failed
403 → Authenticated but not authorized
```

### Path vs Query Parameter?

```text
Path  → Identify resource
Query → Filter/search/sort/paginate
```

### POST vs PUT?

```text
POST → Create / non-idempotent operation generally
PUT  → Replace/update resource / idempotent generally
```

### What is API chaining?

Using output from one API as input to another API.

### What is correlation?

Extracting dynamic data from one response and using it in subsequent requests.

### What is idempotency?

An operation is idempotent if repeating it produces the same intended server state.

Commonly:

```text
GET    → Idempotent
PUT    → Idempotent
DELETE → Idempotent
POST   → Generally non-idempotent
PATCH  → Depends on implementation
```

---

# 37. Important Interview Keywords

```text
API
REST
RESTful
HTTP
HTTPS
Endpoint
URI
URL
Request
Response
Payload
Headers
Parameters
Path Parameter
Query Parameter
CRUD
JSON
XML
Status Code
Authentication
Authorization
JWT
OAuth 2.0
Bearer Token
Idempotency
Stateless
Caching
Schema Validation
Contract Testing
Positive Testing
Negative Testing
Boundary Testing
API Chaining
Correlation
Postman
Collection
Environment
Variables
Pre-request Script
Test Script
Assertions
Collection Runner
Data-driven Testing
Newman
CI/CD
REST Assured
```

---

# 38. SDET API Testing Flow

```text
Understand API Contract
        ↓
Identify Endpoints
        ↓
Understand Request
        ↓
Understand Response
        ↓
Identify Authentication
        ↓
Create Positive Tests
        ↓
Create Negative Tests
        ↓
Validate Status Codes
        ↓
Validate Headers
        ↓
Validate Response Body
        ↓
Validate Schema
        ↓
Validate Business Rules
        ↓
Add Data-driven Tests
        ↓
Chain APIs
        ↓
Automate
        ↓
Integrate with CI/CD
```

---

# 39. Must-Know Postman Commands / Concepts

```text
{{variable}}
pm.test()
pm.expect()
pm.response
pm.response.json()
pm.environment.set()
pm.environment.get()
pm.collectionVariables.set()
pm.variables.get()
pm.sendRequest()
pm.iterationData.get()
```

---

# 40. SDET Priority

### Must Know ⭐⭐⭐

```text
REST
HTTP Methods
Status Codes
Headers
Path Parameters
Query Parameters
Request Body
JSON
Authentication
Authorization
CRUD
API Validation
Postman
Collections
Environments
Variables
Pre-request Scripts
Test Scripts
Assertions
API Chaining
Collection Runner
Newman
```

### Good to Know ⭐⭐

```text
OAuth 2.0
JWT
Schema Validation
Idempotency
Data-driven Testing
Mock Servers
API Monitoring
CI/CD Integration
Contract Testing
```

### Advanced ⭐

```text
REST Assured
API Framework Design
Service Virtualization
Contract Testing
Performance Testing
Security Testing
Microservices Testing
Event-driven API Testing
API Gateway Testing
```

---

# 41. Quick Revision

```text
GET     → Read
POST    → Create
PUT     → Full Update
PATCH   → Partial Update
DELETE  → Delete

200 → OK
201 → Created
204 → No Content

400 → Bad Request
401 → Authentication
403 → Authorization
404 → Not Found
409 → Conflict
429 → Rate Limit

500 → Server Error
502 → Bad Gateway
503 → Service Unavailable
504 → Gateway Timeout

Path Parameter → Identify resource
Query Parameter → Filter/search/etc.

Content-Type → What I send
Accept → What I expect

Pre-request → Before request
Tests → After response

Collection → Group of requests
Environment → Variables/configuration
Runner → Execute collection
Newman → CLI execution

Authentication → Who are you?
Authorization → What can you access?

API Chaining → Response → Next Request
Correlation → Extract & reuse dynamic data
```
