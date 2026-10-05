# Advanced API Testing — SDET Knowledge Book

> **Purpose:** Advanced API + Postman concepts for SDET interviews and enterprise/Fortune 500 environments.
> **Focus:** API behavior, functional testing, contracts, security, microservices, reliability, data, performance and observability.
> **Excluded:** REST Assured and API automation implementation.

---

# 1. API Fundamentals & HTTP

## REST / Resource-Oriented Design

```text
GET    /users
GET    /users/101
POST   /users
PUT    /users/101
PATCH  /users/101
DELETE /users/101
```

Avoid:

```text
❌ /getUsers
❌ /createUser
❌ /deleteUser/101
```

## Stateless API

Each request contains the information required to process it.

**Benefits:**

* Horizontal scaling
* Load balancing
* Easier recovery
* No session affinity

## HTTP Methods

| Method  | Typical Use    | Safe | Idempotent |
| ------- | -------------- | ---: | ---------: |
| GET     | Read           |    ✅ |          ✅ |
| HEAD    | Headers        |    ✅ |          ✅ |
| OPTIONS | Capabilities   |    ✅ |          ✅ |
| POST    | Create/action  |    ❌ |         ❌* |
| PUT     | Replace        |    ❌ |          ✅ |
| PATCH   | Partial update |    ❌ |    Depends |
| DELETE  | Delete         |    ❌ |          ✅ |

> *POST can be made idempotent using an idempotency mechanism.

## Important Status Codes

```text
200 OK
201 Created
202 Accepted
204 No Content

301/302 Redirect
304 Not Modified

400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
412 Precondition Failed
413 Payload Too Large
415 Unsupported Media Type
422 Unprocessable Content
429 Too Many Requests

500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
504 Gateway Timeout
```

---

# 2. Idempotency, Concurrency & Caching

## Idempotency

Repeated execution produces the same **intended server-state effect**.

```text
PUT /users/101
DELETE /users/101
```

> Idempotent does not mean every response must be identical.

## Idempotency-Key

Used to prevent duplicate processing.

```http
POST /payments
Idempotency-Key: abc-123
```

### Test

```text
Same key + same request
Same key + different request
New key
Missing key
Expired key
Concurrent same-key requests
Retry after timeout
Retry after network failure
```

## Optimistic Concurrency

Prevents lost updates.

```text
Client A → Version 5
Client B → Version 5

A → Update → Version 6
B → Update using Version 5 → ❌
```

Common mechanism:

```http
If-Match: "version-5"
```

Expected:

```text
412 Precondition Failed
```

## ETag

Identifies a resource representation/version.

```http
ETag: "abc123"
```

### Conditional GET

```http
If-None-Match: "abc123"
```

Unchanged:

```text
304 Not Modified
```

### Conditional Update

```http
If-Match: "abc123"
```

## HTTP Caching

Know:

```text
Cache-Control
ETag
If-None-Match
Last-Modified
If-Modified-Since
Vary
Expires
```

Test:

```text
Fresh response
Cached response
Stale response
Cache invalidation
Conditional GET
Incorrect cache behavior
Sensitive data caching
```

---

# 3. API Design, Versioning & Compatibility

## Pagination

### Offset/Page

```text
?page=2&size=20
```

### Cursor

```text
?cursor=abc123
```

### Test

```text
First page
Middle page
Last page
Empty page
Invalid page
Negative page
Maximum size
Invalid cursor
Expired cursor
Duplicate records
Missing records
```

## Filtering & Sorting

```text
?status=active
?sort=name&order=asc
```

Test:

```text
Valid
Invalid
Multiple filters
Multiple sort fields
Null values
Case sensitivity
Special characters
Unsupported fields
```

## Field Selection

```text
?fields=id,name,email
```

Test:

```text
Requested fields
Excluded fields
Invalid fields
Empty fields
Duplicate fields
```

## API Versioning

Common approaches:

```text
/api/v1/users

Accept: application/vnd.company.v2+json

/api/users?version=2
```

Test:

```text
Old client + new API
New client + old API
Version-specific behavior
Deprecated endpoints
Breaking changes
```

## Backward Compatibility

### Breaking

```text
❌ Remove field
❌ Rename field
❌ Change data type
❌ Make optional field mandatory
❌ Change authentication
❌ Change error structure
```

### Usually Safer

```text
✅ Add optional field
✅ Add new endpoint
✅ Add optional request field
```

## API Lifecycle

```text
Design
 ↓
Review
 ↓
Develop
 ↓
Test
 ↓
Publish
 ↓
Consume
 ↓
Monitor
 ↓
Version
 ↓
Deprecate
 ↓
Retire
```

## API Governance

Know:

```text
Naming standards
URI standards
HTTP standards
Status-code standards
Error format
Authentication standards
Versioning policy
Documentation standards
Security standards
Compatibility rules
Deprecation policy
```

---

# 4. API Contracts & Schema Testing

## OpenAPI / Swagger

Defines:

```text
Endpoints
Methods
Parameters
Headers
Request schemas
Response schemas
Authentication
Status codes
Examples
```

## JSON Schema Validation

Validate:

```text
Required fields
Optional fields
Data types
Enums
Minimum / Maximum
String length
Regex
Arrays
Nested objects
Nullable fields
```

## Contract Testing

Validates consumer-provider compatibility.

```text
Consumer
   ↓
Expected Contract
   ↓
Provider
   ↓
Actual Response
```

Detects:

```text
Field removal
Type changes
Missing fields
Unexpected structure
Incorrect interactions
Breaking changes
```

### Schema vs Contract

| Schema          | Contract                    |
| --------------- | --------------------------- |
| Structure       | Consumer-provider agreement |
| Data types      | Expected interaction        |
| Required fields | Expected behavior           |
| JSON/OpenAPI    | Consumer expectations       |

## Compatibility Testing

```text
Old client + New API
New client + Old API
Old API version
New API version
Different consumers
Different payload versions
```

---

# 5. Functional & Business Validation

## Business Rule Testing

Don't validate only:

```text
HTTP 200
```

Validate actual business behavior.

Example:

```text
Balance = ₹1,000
Withdrawal = ₹1,500

Expected:
Transaction rejected
Balance unchanged
Correct business error
```

Test:

```text
Business rules
Validation rules
Calculations
Eligibility
Limits
Duplicate transactions
State restrictions
```

## State Transition Testing

Example:

```text
PENDING
   ↓
PROCESSING
   ↓
COMPLETED
```

Invalid:

```text
COMPLETED → PENDING ❌
```

Test:

```text
Valid transitions
Invalid transitions
Duplicate transitions
Concurrent transitions
Retry after transition
Failure during transition
```

## Positive Testing

```text
Valid request
Valid authentication
Valid data
Expected workflow
```

## Negative Testing

```text
Missing field
Null
Empty string
Wrong type
Wrong format
Malformed JSON
Invalid token
Expired token
Unsupported value
Invalid Content-Type
Unknown field
```

## Boundary Testing

For `1–100`:

```text
0
1
2
99
100
101
```

Test:

```text
Minimum
Minimum-1
Minimum+1
Maximum-1
Maximum
Maximum+1
```

---

# 6. Error Handling & Reliability

## Error Validation

Validate:

```text
HTTP status
Error code
Message
Response structure
Field-level errors
Correlation/trace ID
Sensitive information
```

Example:

```json
{
  "code": "USER_NOT_FOUND",
  "message": "User not found",
  "traceId": "abc-123"
}
```

## Error Categories

```text
Validation
Business
Authentication
Authorization
Not Found
Conflict
Rate Limit
Dependency
Timeout
System
```

## Retryable vs Non-Retryable

Often retryable:

```text
429
502
503
504
Network timeout
Temporary dependency failure
```

Usually non-retryable:

```text
400
401
403
404
422
Business validation errors
```

> Actual behavior depends on API design.

## Retry Testing

Validate:

```text
Retry count
Retry interval
Backoff
Maximum retries
Final failure
Duplicate prevention
Idempotency
```

## Exponential Backoff

```text
1 sec
2 sec
4 sec
8 sec
16 sec
```

Test:

```text
429
503
Timeout
Maximum retries
Non-retryable errors
Retry storms
```

## Timeout Testing

```text
Connection timeout
Read timeout
Dependency timeout
Gateway timeout
Slow response
```

---

# 7. Rate Limiting & Quotas

## Rate Limiting

Example:

```text
100 requests / minute
```

Response:

```text
429 Too Many Requests
```

Headers:

```text
Retry-After
X-RateLimit-Limit
X-RateLimit-Remaining
X-RateLimit-Reset
```

Test:

```text
Below limit
At limit
Above limit
Reset
Multiple users
Multiple clients
Burst traffic
```

## Rate Limit vs Quota

```text
Rate Limit → How frequently?

Quota → How much total usage?
```

Example:

```text
100 requests/minute
10,000 requests/day
```

---

# 8. Microservices & Distributed API Testing

## API Gateway

May handle:

```text
Authentication
Authorization
Routing
Rate limiting
SSL termination
Logging
Caching
Transformation
```

Test:

```text
Correct routing
Invalid route
Authentication
Headers
Timeouts
Rate limiting
Service unavailable
Gateway failures
```

## Microservices

Typical flow:

```text
Client
 ↓
Gateway
 ↓
Order Service
 ↓
Payment Service
 ↓
Inventory Service
 ↓
Database
```

Test:

```text
Service APIs
Contracts
Dependencies
Authentication
Failure handling
Retries
Timeouts
Data consistency
```

## Dependency Testing

Test dependency states:

```text
Available
Unavailable
Slow
Timeout
500
502
503
429
Malformed response
Changed response
```

## Third-Party APIs

Test:

```text
Success
Timeout
Retry
Rate limit
Failure
Malformed response
Changed response
Unavailable service
```

## Service Virtualization

Useful when dependency is:

```text
Unavailable
Expensive
Slow
Unstable
Still under development
```

Test simulated:

```text
Success
Timeout
5xx
Slow response
Malformed response
Unavailable service
```

## Mock vs Stub

**Stub**

```text
Request → Predefined Response
```

**Mock**

```text
Request → Simulated Response
          +
       Interaction Verification
```

---

# 9. Async APIs, Events & Webhooks

## Async API

Example:

```http
POST /reports
```

Response:

```text
202 Accepted
```

Then:

```http
GET /reports/101/status
```

States:

```text
QUEUED
PROCESSING
COMPLETED
FAILED
```

Test:

```text
Accepted
Polling
Completion
Failure
Timeout
Duplicate request
Cancellation
```

## Eventual Consistency

```text
POST Order
 ↓
Order Created
 ↓
Event Published
 ↓
Inventory Updated
 ↓
Eventually Consistent
```

Test:

```text
Create
 ↓
Immediate GET
 ↓
Poll
 ↓
Verify final state
```

Key concepts:

```text
Strong consistency
Eventual consistency
Read-after-write
Stale data
Duplicate data
Missing data
```

## Webhooks

```text
System A
 ↓
Event
 ↓
Webhook
 ↓
System B
```

Test:

```text
Valid event
Invalid payload
Duplicate event
Retry
Signature validation
Out-of-order events
Timeout
Consumer unavailable
```

## Event-Driven Concepts

Know:

```text
Event
Producer
Consumer
Topic
Partition
Message ordering
Duplicate events
At-most-once
At-least-once
Exactly-once
Dead-letter queue
Retry
Event replay
```

---

# 10. Transactions, Consistency & Data

## Transaction Testing

Example:

```text
Create Order
 ↓
Payment
 ↓
Inventory
 ↓
Failure
```

Expected:

```text
Rollback
OR
Compensation
```

Test:

```text
Commit
Rollback
Partial failure
Duplicate transaction
Retry
Timeout
Dependency failure
```

## Saga Pattern

Distributed transaction:

```text
Create Order
 ↓
Payment Success
 ↓
Inventory Failure
 ↓
Refund Payment
 ↓
Cancel Order
```

Test:

```text
Compensation triggered
Compensation success
Compensation failure
Duplicate compensation
Partial failure
Recovery
```

## Data Integrity

Validate:

```text
Insert
Update
Delete
Rollback
Duplicate records
Null handling
Data types
Referential integrity
API ↔ DB consistency
```

## Data Consistency

Check:

```text
API
 ↓
Database
 ↓
Cache
 ↓
Other Services
 ↓
Events
```

## Database Validation

Validate:

```text
Record creation
Updates
Deletes
Rollback
Duplicate records
Foreign keys
Null values
Transformation
Precision
```

## Concurrency

Test:

```text
Concurrent updates
Duplicate creation
Double payment
Inventory decrement
Same idempotency key
Stale data
Race conditions
```

## Multi-Tenant APIs

Critical test:

```text
Tenant A token
      ↓
Tenant B resource
      ↓
❌ Access denied
```

Validate:

```text
Tenant isolation
Data leakage
Authorization
Filtering
Cache isolation
```

---

# 11. File, Date, Localization & Financial Data

## File APIs

### Upload

```text
Valid file
Invalid type
Large file
Empty file
Corrupted file
Duplicate file
Multiple files
Special filename
Unsupported extension
```

### Download

Validate:

```text
Content-Type
Content-Length
Filename
File integrity
Permissions
Range requests
```

## Content Negotiation

```http
Accept: application/json
Content-Type: application/json
```

Test:

```text
Supported format
Unsupported format
Missing Accept
Multiple formats
Content-Type mismatch
```

## Date & Time

Test:

```text
UTC
Time zones
Offsets
DST
Leap years
Month-end
Year-end
Midnight
Unix timestamp
ISO-8601
```

## Localization / Internationalization

```text
Languages
Unicode
UTF-8
Time zones
Currency
Date formats
Decimal separators
Locale rules
```

## Currency & Precision

Test:

```text
Decimal precision
Rounding
Currency precision
Negative values
Zero
Very large values
Very small values
Floating-point behavior
```

---

# 12. API Security

## Authentication

Test:

```text
No token
Invalid token
Expired token
Malformed token
Wrong token
Revoked token
```

## Authorization

Test:

```text
Admin
User
Read-only
Different role
Different tenant
Different resource owner
```

## OAuth 2.0 / OIDC

Know:

```text
Authorization Code
PKCE
Client Credentials
Refresh Token
Access Token
Scopes
Issuer
Audience
Claims
Token Expiry
```

## JWT

Validate:

```text
Signature
Expiry
Issuer
Audience
Claims
Algorithm
Token tampering
```

## RBAC

**Role-Based Access Control**

```text
Admin → CRUD
User  → Read
```

Test every role against protected endpoints.

## ABAC

**Attribute-Based Access Control**

Access may depend on:

```text
User
Role
Department
Location
Resource
Time
Tenant
```

## IDOR

Example:

```text
GET /accounts/101
GET /accounts/102
```

Verify user 101 cannot access account 102 without authorization.

## Field-Level Authorization

Verify sensitive fields are not exposed to unauthorized roles.

```text
Allowed:
name
email

Restricted:
salary
```

## Mass Assignment

Example attack:

```json
{
  "name": "Raj",
  "role": "ADMIN"
}
```

Verify protected fields cannot be modified by unauthorized users.

## Sensitive Data / PII

Check:

```text
Response
Headers
URL
Query parameters
Logs
Errors
```

Sensitive data:

```text
PII
Credentials
Tokens
Financial information
Account information
```

## Data Masking

```text
Original:
4111111111111111

Masked:
************1111
```

Check:

```text
Responses
Logs
Errors
Reports
Monitoring
```

## Security Attack Areas

Know:

```text
SQL Injection
NoSQL Injection
Command Injection
XSS
IDOR
Mass Assignment
Broken Authentication
Broken Authorization
Sensitive Data Exposure
Replay Attack
Brute Force
Rate Limit Bypass
SSRF
Path Traversal
Header Injection
```

## Replay Attack

Controls:

```text
Nonce
Timestamp
Idempotency Key
Short-lived Token
Request Signature
```

## TLS / HTTPS

Test:

```text
HTTPS enforced
HTTP blocked/redirected
Certificate validity
Expired certificate
Invalid certificate
TLS configuration
Sensitive data over HTTP
```

## CORS / CSRF

### CORS

Know:

```text
Access-Control-Allow-Origin
Access-Control-Allow-Methods
Access-Control-Allow-Headers
Access-Control-Allow-Credentials
Preflight
```

### CSRF

Relevant mainly when authentication relies on automatically attached browser credentials.

> **CORS ≠ CSRF protection.**

---

# 13. Performance, Resilience & Chaos

## Performance Metrics

```text
Response Time
Latency
Throughput
Requests/sec
Concurrent Users
Error Rate
CPU
Memory
```

### Percentiles

```text
P50 → Median
P90 → 90% of requests at/below value
P95 → 95% of requests at/below value
P99 → 99% of requests at/below value
```

Example:

```text
P95 < 500 ms
Error Rate < 1%
```

## Performance Types

```text
Load
Stress
Spike
Endurance / Soak
Volume
Scalability
```

## Resilience Testing

Simulate:

```text
Service unavailable
Database unavailable
Network delay
Timeout
High traffic
Partial failure
Duplicate messages
Dependency failure
```

Goal:

> Verify safe failure + recovery.

## Chaos Testing

Intentionally introduce failures:

```text
Kill service
Introduce latency
Drop requests
Disable dependency
Increase traffic
```

Validate:

```text
Recovery
Failover
Retry
Circuit breaker
Data consistency
Alerting
```

## Circuit Breaker

```text
CLOSED
   ↓
Failure Threshold
   ↓
OPEN
   ↓
Wait
   ↓
HALF-OPEN
   ↓
Success → CLOSED
Failure → OPEN
```

Test:

```text
Failure threshold
Open state
Recovery
Half-open state
Fallback
Timeout
```

---

# 14. Observability & Production Testing

## Three Pillars

```text
Logs
Metrics
Traces
```

## Logs

Look for:

```text
Timestamp
Endpoint
Request ID
Status
Error
```

## Metrics

```text
Latency
Throughput
Error Rate
Availability
CPU
Memory
```

## Distributed Tracing

```text
Client
 ↓
Gateway
 ↓
Service A
 ↓
Service B
 ↓
Service C
```

Know:

```text
Trace ID
Span ID
Parent Span
Service
Latency
Error
```

## Correlation ID

```http
X-Correlation-ID: 12345-abc
```

Used to track a transaction across services.

## Health Checks

```text
/health
/liveness
/readiness
```

```text
Liveness  → Is service alive?
Readiness → Can service receive traffic?
```

## Production Testing

Prefer:

```text
Smoke Tests
Health Checks
Synthetic Monitoring
Post-Deployment Validation
Canary Validation
Read-only Testing
Production-safe Data
Rollback Validation
SLA Monitoring
```

Avoid destructive testing in production.

## SLA / SLO / SLI

```text
SLI → Measurement
SLO → Target
SLA → Business Agreement
```

Example:

```text
SLI → API latency
SLO → P95 < 500ms
SLA → 99.9% availability
```

---

# 15. API Test Data & Environments

## Test Data

Strategies:

```text
Test data API
Database fixtures
Unique data
Dynamic data
Synthetic data
Masked data
Cleanup
```

Lifecycle:

```text
Create
 ↓
Test
 ↓
Validate
 ↓
Cleanup
```

## Environment Strategy

Know:

```text
DEV
QA
SIT
UAT
STAGE
PROD
```

Test environment-specific:

```text
URLs
Credentials
Feature flags
Dependencies
Test data
Configuration
```

---

# 16. Feature Flags & API Governance

## Feature Flags

Test:

```text
Flag ON
Flag OFF
User-specific
Tenant-specific
Gradual rollout
Old behavior
New behavior
```

## API Documentation Testing

Verify:

```text
Endpoint
Method
Parameters
Required fields
Authentication
Status codes
Schema
Examples
Errors
```

## API Governance

Enterprise standards:

```text
Naming
Versioning
Security
Authentication
Error format
Documentation
Compatibility
Deprecation
Logging
Monitoring
```

---

# 17. API Testing Strategy & CI/CD

## API Test Pyramid

```text
             E2E
            /   \
       Integration
          /       \
       Contract
        /         \
    API Functional
       /           \
       Unit Tests
```

Principle:

> More fast, focused tests. Fewer expensive E2E tests.

## Enterprise API Test Strategy

```text
Contract
   ↓
Authentication
   ↓
Authorization
   ↓
Functional
   ↓
Negative
   ↓
Boundary
   ↓
Schema
   ↓
Business Rules
   ↓
State
   ↓
Idempotency
   ↓
Concurrency
   ↓
Retry / Timeout
   ↓
Data Integrity
   ↓
Security
   ↓
Performance
   ↓
Resilience
   ↓
Observability
```

## CI/CD Quality Gate

```text
Code
 ↓
Build
 ↓
Deploy Test Environment
 ↓
API Tests
 ↓
Contract Tests
 ↓
Integration Tests
 ↓
Reports
 ↓
Quality Gate
 ↓
Deploy
```

---

# 18. Postman — Advanced Knowledge

## Collections

Group related requests:

```text
Collection
 ├── Authentication
 ├── Users
 ├── Orders
 └── Payments
```

## Variables

Know scopes:

```text
Global
Collection
Environment
Data
Local
```

### Important

Understand:

```text
Variable scope
Variable precedence
Current value
Initial value
Secret variables
Environment switching
```

## Pre-request Scripts

Used to prepare request data:

```text
Generate token
Generate timestamp
Generate dynamic data
Set variables
Calculate signatures
```

## Test Scripts

Validate:

```text
Status
Headers
Response body
Schema
Business conditions
Variables
```

Common `pm` concepts:

```text
pm.response
pm.request
pm.environment
pm.collectionVariables
pm.variables
pm.expect
pm.test
```

## Request Chaining

```text
Login
 ↓
Capture Token
 ↓
Store Variable
 ↓
Call API
 ↓
Capture ID
 ↓
Use ID in Next API
```

## Collection Runner

Know:

```text
Iterations
Data files
Environment
Test execution
Results
```

## Dynamic Variables

Examples:

```text
{{$guid}}
{{$timestamp}}
{{$randomEmail}}
```

## Examples / Mocks

Useful for:

```text
Frontend development
Contract understanding
Dependency simulation
Response examples
```

## Monitors

Used for scheduled API checks.

Know:

```text
Scheduled execution
Availability
Response validation
Failure notification
```

## Postman Console

Useful for debugging:

```text
Requests
Responses
Headers
Scripts
Variables
Errors
```

---

# 19. Other API Technologies — Awareness

For Fortune 500 SDET roles, know the basics of:

## GraphQL

```text
Query
Mutation
Subscription
Schema
Resolver
```

## gRPC

```text
Protocol Buffers
Unary RPC
Streaming
Service Definition
```

## WebSockets

```text
Persistent connection
Full-duplex communication
Real-time updates
```

## SSE

```text
Server → Client
Persistent HTTP connection
Real-time events
```

## SOAP

Know:

```text
XML
WSDL
SOAP Envelope
SOAP Header
SOAP Body
Fault
```

## AsyncAPI

Specification for:

```text
Event-driven APIs
Messages
Channels
Publish/Subscribe
```

## Service Mesh

Know:

```text
Service-to-service communication
mTLS
Traffic management
Retries
Observability
Load balancing
```

---

# 20. Enterprise API Test Checklist

```text
□ HTTP Method
□ URL
□ Path Parameters
□ Query Parameters
□ Headers
□ Authentication
□ Authorization
□ Request Body
□ Response Status
□ Response Headers
□ Response Body
□ Schema
□ Business Rules
□ Positive Testing
□ Negative Testing
□ Boundary Testing
□ Pagination
□ Filtering
□ Sorting
□ Idempotency
□ Concurrency
□ Rate Limiting
□ Retry
□ Timeout
□ Error Handling
□ Security
□ Performance
□ Contract
□ Versioning
□ Compatibility
□ Observability
□ CI/CD
□ Database Validation
□ Data Consistency
□ Dependencies
□ Production Validation
```

---

# 21. SDET Interview Keywords

```text
Idempotency
Idempotency-Key
ETag
If-Match
If-None-Match
Optimistic Locking
Conditional Request
Caching
Pagination
Cursor Pagination
API Versioning
Backward Compatibility
Forward Compatibility
OpenAPI
Swagger
JSON Schema
Contract Testing
Consumer-Driven Contract
API Gateway
Microservices
Service Virtualization
Mock
Stub
Retry
Exponential Backoff
Rate Limiting
Quota
Timeout
Circuit Breaker
Async API
Webhook
Eventual Consistency
Saga
Compensation
Transaction
Rollback
Concurrency
Race Condition
Distributed Tracing
Correlation ID
Trace ID
Span ID
Multi-Tenancy
RBAC
ABAC
IDOR
Mass Assignment
PII
Data Masking
OAuth
OIDC
JWT
TLS
CORS
CSRF
Replay Attack
API Security
Performance Testing
Resilience Testing
Chaos Testing
Observability
SLA
SLO
SLI
Health Check
Feature Flag
API Governance
API Lifecycle
API Deprecation
```

---

# 22. Priority Map

## ⭐⭐⭐ Must Know

```text
HTTP
REST
Status Codes
Authentication
Authorization
Business Rules
Positive / Negative Testing
Boundary Testing
Pagination
Filtering
Sorting
Idempotency
ETag
Optimistic Locking
API Versioning
Backward Compatibility
OpenAPI
JSON Schema
Contract Testing
API Gateway
Microservices
Retry
Timeout
Rate Limiting
Concurrency
Data Consistency
Error Handling
RBAC
IDOR
API Security
Performance Basics
Observability
Postman Variables
Postman Scripts
Postman Chaining
```

## ⭐⭐ Good to Know

```text
Async APIs
Webhooks
Eventual Consistency
Saga
Compensation
Service Virtualization
Circuit Breaker
Distributed Tracing
Multi-Tenant Testing
Resilience Testing
Chaos Testing
Feature Flags
Production Testing
SLA / SLO / SLI
API Governance
API Lifecycle
OAuth / OIDC
JWT
```

## ⭐ Advanced / Awareness

```text
GraphQL
gRPC
AsyncAPI
Service Mesh
Advanced OAuth
Distributed Systems
Advanced Performance Engineering
```

---

# 23. Rapid Revision

```text
Idempotency
→ Same intended server-state effect when repeated

Idempotency-Key
→ Prevent duplicate processing during retries

ETag
→ Resource representation/version identifier

If-None-Match
→ Conditional GET / caching

If-Match
→ Conditional update / concurrency control

304
→ Not Modified

412
→ Precondition Failed

429
→ Too Many Requests

202
→ Accepted / asynchronous processing

Contract Testing
→ Consumer ↔ Provider compatibility

OpenAPI
→ API contract specification

API Gateway
→ Routing + cross-cutting API policies

Correlation ID
→ Track one transaction across services

Trace ID
→ Identify distributed request trace

Eventual Consistency
→ Data becomes consistent after propagation

Saga
→ Distributed transaction pattern

Compensation
→ Reversal/recovery action

Circuit Breaker
→ Stop calls to failing dependency

Exponential Backoff
→ Increasing retry intervals

P95
→ 95% of requests complete at/below the measured value

IDOR
→ Unauthorized access to another object's resource

RBAC
→ Access based on role

ABAC
→ Access based on attributes

PII
→ Personally identifiable information

Observability
→ Logs + Metrics + Traces

SLI
→ Measurement

SLO
→ Target

SLA
→ Business agreement

Liveness
→ Is service alive?

Readiness
→ Can service receive traffic?
```

---

# 24. Fortune 500 SDET Mindset

Don't think only:

```text
"Does the API return 200?"
```

Think:

```text
Can I access it?
        ↓
Am I authorized?
        ↓
Is the request valid?
        ↓
Is the response correct?
        ↓
Is the business rule correct?
        ↓
Is the data correct?
        ↓
What happens on retry?
        ↓
What happens concurrently?
        ↓
What happens when a dependency fails?
        ↓
Is sensitive data protected?
        ↓
Is it backward compatible?
        ↓
Can I observe/debug failures?
        ↓
Does it meet performance expectations?
        ↓
Does it recover safely?
```

> **Strong SDET thinking = Functional + Business + Security + Data + Reliability + Performance + Observability.**
