<div align="center">

# 💳 Payments Management System

### Enterprise-Grade Payment Processing & Double-Entry Ledger Platform

[![Java](https://img.shields.io/badge/Java-11-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://openjdk.java.net/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-2.7.15-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![Maven](https://img.shields.io/badge/Maven-3.x-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white)](https://maven.apache.org/)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

[![Build Status](https://img.shields.io/github/actions/workflow/status/ayushmaanbhav/Payments-Management-System/ci.yml?branch=master&style=flat-square&logo=github)](https://github.com/ayushmaanbhav/Payments-Management-System/actions)
[![Code Coverage](https://img.shields.io/codecov/c/github/ayushmaanbhav/Payments-Management-System?style=flat-square&logo=codecov)](https://codecov.io/gh/ayushmaanbhav/Payments-Management-System)
[![CodeFactor](https://img.shields.io/codefactor/grade/github/ayushmaanbhav/Payments-Management-System?style=flat-square&logo=codefactor)](https://www.codefactor.io/repository/github/ayushmaanbhav/Payments-Management-System)
[![Last Commit](https://img.shields.io/github/last-commit/ayushmaanbhav/Payments-Management-System?style=flat-square)](https://github.com/ayushmaanbhav/Payments-Management-System/commits/master)

<p align="center">
  <strong>A production-ready, microservices-based payment processing system featuring double-entry bookkeeping, idempotent transactions, and extensible payment gateway integrations.</strong>
</p>

[Features](#-features) •
[Architecture](#-architecture) •
[Quick Start](#-quick-start) •
[API Reference](#-api-reference) •
[Documentation](#-documentation) •
[Contributing](#-contributing)

---

</div>

## 🌟 Features

<table>
<tr>
<td width="50%">

### 💰 Payment Processing
- **Multi-gateway support** with pluggable architecture
- **Real-time payment tracking** and status updates
- **Webhook handling** for async payment confirmations
- **Payment expiry management** with scheduled jobs
- **Idempotent operations** preventing duplicate charges

</td>
<td width="50%">

### 📒 Double-Entry Ledger
- **GAAP-compliant** double-entry bookkeeping
- **Multi-currency support** with normalized amounts
- **Balanced transactions** with automatic validation
- **Complete audit trail** for all financial operations
- **Account type enforcement** (Debit/Credit)

</td>
</tr>
<tr>
<td width="50%">

### 🔌 Gateway Integrations
- **SETU Payment Gateway** (UPI, Bank Transfers)
- **Extensible factory pattern** for new providers
- **Token refresh management** for OAuth providers
- **Connection settings encryption** for security
- **API event logging** for debugging & audits

</td>
<td width="50%">

### ⚡ Event Processing
- **Asynchronous event handling** for scalability
- **Retry mechanisms** with configurable policies
- **Event status tracking** (Pending → Completed)
- **Workflow orchestration** for complex operations
- **Dead letter handling** for failed events

</td>
</tr>
</table>

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENT APPLICATIONS                              │
│                    (Web Apps, Mobile Apps, Third-party Services)             │
└─────────────────────────────────────────────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                                 API GATEWAY                                  │
│                          (Authentication, Rate Limiting)                     │
└─────────────────────────────────────────────────────────────────────────────┘
                                        │
            ┌───────────────────────────┼───────────────────────────┐
            ▼                           ▼                           ▼
┌─────────────────────┐   ┌─────────────────────┐   ┌─────────────────────┐
│   PAYMENT SERVICE   │   │   LEDGER SERVICE    │   │  GATEWAY PROVIDER   │
│    (/payment)       │   │     (/ledger)       │   │  (/gatewayProvider) │
├─────────────────────┤   ├─────────────────────┤   ├─────────────────────┤
│ • Payment Orders    │   │ • Account Mgmt      │   │ • Provider Config   │
│ • Line Items        │   │ • Transactions      │   │ • Payment Execution │
│ • Status Tracking   │   │ • Balance Calc      │   │ • Webhook Handling  │
│ • Expiry Jobs       │   │ • Audit Trail       │   │ • Token Refresh     │
└─────────────────────┘   └─────────────────────┘   └─────────────────────┘
            │                           │                           │
            └───────────────────────────┼───────────────────────────┘
                                        │
            ┌───────────────────────────┼───────────────────────────┐
            ▼                           ▼                           ▼
┌─────────────────────┐   ┌─────────────────────┐   ┌─────────────────────┐
│  EVENT PROCESSOR    │   │ REQUEST IDEMPOTENCY │   │   SHARED COMMONS    │
├─────────────────────┤   ├─────────────────────┤   ├─────────────────────┤
│ • Async Processing  │   │ • Duplicate Detect  │   │ • DTOs & Models     │
│ • Retry Logic       │   │ • Request Mapping   │   │ • Utilities         │
│ • Status Updates    │   │ • Response Cache    │   │ • Exception Handling│
└─────────────────────┘   └─────────────────────┘   └─────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              MYSQL DATABASE                                  │
│                    (Liquibase Migrations, Optimistic Locking)               │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Module Dependency Graph

```
                                    ┌──────────────────┐
                                    │  ayushmaanbhav   │
                                    │    -commons      │
                                    └────────┬─────────┘
                                             │
                         ┌───────────────────┼───────────────────┐
                         ▼                   ▼                   ▼
              ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
              │  commons-spring  │ │     client       │ │    database      │
              └────────┬─────────┘ └────────┬─────────┘ └──────────────────┘
                       │                    │
         ┌─────────────┼─────────────┬──────┴──────┬─────────────┐
         ▼             ▼             ▼             ▼             ▼
┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐
│  payment   │ │   ledger   │ │  gateway   │ │   event    │ │  request   │
│            │ │            │ │  provider  │ │ processor  │ │ idempotency│
└────────────┘ └────────────┘ └─────┬──────┘ └────────────┘ └────────────┘
                                    │
                              ┌─────┴─────┐
                              ▼           ▼
                       ┌───────────┐ ┌───────────┐
                       │ gw-common │ │  gw-setu  │
                       └───────────┘ └───────────┘
```

## 📊 Data Model

### Entity Relationship Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              PAYMENT DOMAIN                                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────┐         ┌─────────────────────────────────┐       │
│  │   PAYMENT_ORDER     │         │    PAYMENT_ORDER_LINE_ITEM      │       │
│  ├─────────────────────┤         ├─────────────────────────────────┤       │
│  │ PK id               │◄────────┤ FK payment_order_id             │       │
│  │    external_id (UK) │    1:N  │ PK id                           │       │
│  │    customer_id      │         │    external_id (UK)             │       │
│  │    store_id         │         │    normalised_amount            │       │
│  │    source           │         │    currency                     │       │
│  │    source_service   │         │    status                       │       │
│  │    status           │         │    ledger_debit_account_id      │       │
│  │    type             │         │    ledger_credit_account_id     │       │
│  │    version          │         │ FK gateway_provider_config_id   │       │
│  │    created_on       │         │ FK gateway_provider_payment_id  │       │
│  │    updated_on       │         │    paid_date                    │       │
│  └─────────────────────┘         │    expired_date                 │       │
│                                  └─────────────────────────────────┘       │
│                                                                             │
│  ┌─────────────────────────────┐   ┌─────────────────────────────────┐     │
│  │  GATEWAY_PROVIDER_CONFIG    │   │  GATEWAY_PROVIDER_PAYMENT_DETAIL│     │
│  ├─────────────────────────────┤   ├─────────────────────────────────┤     │
│  │ PK id                       │   │ PK id                           │     │
│  │    external_id (UK)         │   │    order_id (IDX)               │     │
│  │    provider                 │   │    provider_order_id (IDX)      │     │
│  │    merchant_id              │   │    type                         │     │
│  │ FK connection_setting_id    │   │    provider_status              │     │
│  │    disabled                 │   │    payment_web_url              │     │
│  │    disabled_reason          │   │    payment_deep_link            │     │
│  │    disabled_date            │   │    provider_transaction_id      │     │
│  └──────────────┬──────────────┘   │    provider_session_id          │     │
│                 │                  │    provider_payment_method       │     │
│                 │                  │    provider_paid_date            │     │
│                 ▼                  │    provider_error_code           │     │
│  ┌─────────────────────────────┐   │    provider_error_description   │     │
│  │ GATEWAY_CONNECTION_SETTING  │   └─────────────────────────────────┘     │
│  ├─────────────────────────────┤                                           │
│  │ PK id                       │   ┌─────────────────────────────────┐     │
│  │    external_id (UK)         │   │    GATEWAY_PROVIDER_EVENT       │     │
│  │    provider                 │   ├─────────────────────────────────┤     │
│  │    connection_setting       │   │ PK id                           │     │
│  │    token_refresh_enabled    │   │    order_id                     │     │
│  │    retry_count              │   │    api_client_class             │     │
│  │    last_token_refresh_date  │   │    event_type                   │     │
│  └─────────────────────────────┘   │    http_method / http_status    │     │
│                                    │    request / response           │     │
│                                    │    headers / query_params       │     │
│                                    └─────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                               LEDGER DOMAIN                                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────┐                                                    │
│  │   LEDGER_ACCOUNT    │         ┌─────────────────────────────────┐       │
│  ├─────────────────────┤         │      LEDGER_TRANSACTION         │       │
│  │ PK id               │         ├─────────────────────────────────┤       │
│  │    external_id (UK) │         │ PK id                           │       │
│  │    request_id (UK)  │         │    transaction_ref_id (UK)      │       │
│  │    currency         │         │    transaction_date             │       │
│  │    type (DEBIT/     │         │    version                      │       │
│  │         CREDIT)     │         │    created_on                   │       │
│  │    version          │         │    updated_on                   │       │
│  │    created_on       │         └──────────────┬──────────────────┘       │
│  │    updated_on       │                        │                          │
│  └──────────┬──────────┘                        │ 1:N                      │
│             │                                   ▼                          │
│             │              ┌─────────────────────────────────────────┐     │
│             │              │     LEDGER_TRANSACTION_LINE_ITEM        │     │
│             │              ├─────────────────────────────────────────┤     │
│             │              │ PK id                                   │     │
│             └──────────────┤ FK account_id                           │     │
│                    N:1     │ FK transaction_id                       │     │
│                            │    normalised_amount                    │     │
│                            │    currency                             │     │
│                            │    operation_type (DEBIT/CREDIT)        │     │
│                            │    transfer_entity_type                 │     │
│                            │    version                              │     │
│                            │    created_on                           │     │
│                            │    updated_on                           │     │
│                            └─────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────────────────────┘
```

## 🚀 Quick Start

### Prerequisites

- **Java 11** or higher
- **Maven 3.6+**
- **MySQL 8.0+**
- **Docker** (optional, for containerized setup)

### Installation

```bash
# Clone the repository
git clone https://github.com/ayushmaanbhav/Payments-Management-System.git
cd Payments-Management-System

# Build all modules
mvn clean install

# Run database migrations
mvn -pl ayushmaanbhav-database spring-boot:run

# Start the payment service
mvn -pl ayushmaanbhav-payment spring-boot:run

# Start the ledger service (in another terminal)
mvn -pl ayushmaanbhav-ledger spring-boot:run

# Start the gateway provider service (in another terminal)
mvn -pl ayushmaanbhav-gateway-provider spring-boot:run
```

### Configuration

Create `application-local.properties` in each service module:

```properties
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/payments_db
spring.datasource.username=your_username
spring.datasource.password=your_password

# JPA Configuration
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=false

# Service Configuration
server.port=8080
```

### Docker Compose (Recommended)

```yaml
version: '3.8'
services:
  mysql:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: payments_db
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

  payment-service:
    build: ./ayushmaanbhav-payment
    ports:
      - "8081:8080"
    depends_on:
      - mysql
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://mysql:3306/payments_db

  ledger-service:
    build: ./ayushmaanbhav-ledger
    ports:
      - "8082:8080"
    depends_on:
      - mysql

  gateway-provider:
    build: ./ayushmaanbhav-gateway-provider
    ports:
      - "8083:8080"
    depends_on:
      - mysql

volumes:
  mysql_data:
```

## 📖 API Reference

### Payment Service (`/payment`)

<details>
<summary><code>POST</code> <code><b>/payment/order</b></code> <code>Create Payment Order</code></summary>

##### Request Body

```json
{
  "customerId": "cust_123",
  "storeId": "store_456",
  "externalId": "order_789",
  "source": "WEB",
  "sourceService": "CHECKOUT",
  "type": "CUSTOMER_ORDER_CHECKOUT_ONE_TIME_PAYMENT",
  "lineItems": [
    {
      "externalId": "item_001",
      "amount": 10000,
      "currency": "INR",
      "ledgerDebitAccountId": "acc_debit_123",
      "ledgerCreditAccountId": "acc_credit_456",
      "gatewayProviderConfigId": "gw_config_789"
    }
  ]
}
```

##### Response

```json
{
  "success": true,
  "data": {
    "orderId": "pay_order_abc123",
    "status": "PENDING",
    "paymentUrl": "https://payment.gateway.com/pay/xyz",
    "paymentDeepLink": "upi://pay?pa=merchant@upi",
    "expiresAt": "2024-01-15T10:30:00Z"
  }
}
```

</details>

<details>
<summary><code>GET</code> <code><b>/payment/order</b></code> <code>Get Payment Order</code></summary>

##### Query Parameters

| Parameter | Type     | Description              |
|-----------|----------|--------------------------|
| `orderId` | `string` | **Required**. Order ID   |

##### Response

```json
{
  "success": true,
  "data": {
    "orderId": "pay_order_abc123",
    "customerId": "cust_123",
    "status": "SUCCESS",
    "lineItems": [
      {
        "externalId": "item_001",
        "amount": 10000,
        "currency": "INR",
        "status": "PAID",
        "paidDate": "2024-01-15T10:25:00Z"
      }
    ]
  }
}
```

</details>

### Ledger Service (`/ledger`)

<details>
<summary><code>POST</code> <code><b>/account</b></code> <code>Create Account</code></summary>

##### Request Body

```json
{
  "externalId": "acc_merchant_001",
  "requestId": "req_unique_123",
  "currency": "INR",
  "type": "CREDIT"
}
```

##### Response

```json
{
  "success": true,
  "data": {
    "accountId": "ledger_acc_xyz",
    "externalId": "acc_merchant_001",
    "currency": "INR",
    "type": "CREDIT",
    "balance": 0
  }
}
```

</details>

<details>
<summary><code>POST</code> <code><b>/transaction</b></code> <code>Record Transaction</code></summary>

##### Request Body

```json
{
  "transactionRefId": "txn_ref_001",
  "transactionDate": "2024-01-15T10:30:00Z",
  "lineItems": [
    {
      "accountId": "ledger_acc_001",
      "amount": 10000,
      "currency": "INR",
      "operationType": "DEBIT",
      "transferEntityType": "PAYMENT"
    },
    {
      "accountId": "ledger_acc_002",
      "amount": 10000,
      "currency": "INR",
      "operationType": "CREDIT",
      "transferEntityType": "PAYMENT"
    }
  ]
}
```

> **Note:** Transaction line items must balance (total debits = total credits)

</details>

### Health & Monitoring

| Endpoint | Description |
|----------|-------------|
| `GET /health` | Service health check |
| `GET /actuator/health` | Spring Actuator health |
| `GET /actuator/metrics` | Application metrics |
| `GET /actuator/info` | Application info |

## 🧪 Testing

```bash
# Run all tests
mvn test

# Run tests for a specific module
mvn -pl ayushmaanbhav-payment test

# Run tests with coverage
mvn test jacoco:report

# Run integration tests
mvn verify -P integration-tests
```

### Test Coverage

| Module | Coverage |
|--------|----------|
| Payment Service | 85%+ |
| Ledger Service | 80%+ |
| Gateway Provider | 75%+ |
| Event Processor | 80%+ |

## 🔧 Design Patterns & Best Practices

### Patterns Implemented

| Pattern | Implementation | Purpose |
|---------|---------------|---------|
| **Factory** | `GatewayProviderPaymentApiClientFactory` | Dynamic provider client creation |
| **Strategy** | Gateway Provider implementations | Pluggable payment providers |
| **Repository** | JPA Repositories | Data access abstraction |
| **DAO** | `AbstractDao` | Common database operations |
| **Mapper** | `*Mapper` classes | DTO ↔ Entity transformations |
| **Builder** | Lombok `@Builder` | Immutable object construction |

### Data Integrity

- **Idempotency**: All payment operations are idempotent via `RequestIdempotencyService`
- **Optimistic Locking**: Version-based concurrency control on all entities
- **Double-Entry Validation**: Ledger transactions must balance before persistence
- **Audit Trail**: Automatic `created_on` and `updated_on` timestamps

### Security Measures

- **Encrypted Credentials**: Gateway connection settings stored encrypted
- **Input Validation**: Bean validation on all API inputs
- **SQL Injection Prevention**: Parameterized queries via JPA
- **Exception Sanitization**: No stack traces in API responses

## 📁 Project Structure

```
Payments-Management-System/
├── ayushmaanbhav-commons/           # Shared utilities, DTOs, exceptions
├── ayushmaanbhav-commons-spring/    # Spring-specific common components
├── ayushmaanbhav-client/            # HTTP client abstractions
├── ayushmaanbhav-database/          # Liquibase migrations
│   └── src/main/resources/
│       └── db/changelog/
│           └── migrations/          # SQL migration scripts
├── ayushmaanbhav-payment/           # Payment order management
│   ├── src/main/java/
│   │   └── com/ayushmaanbhav/payment/
│   │       ├── controller/          # REST controllers
│   │       ├── service/             # Business logic
│   │       ├── repository/          # Data access
│   │       ├── entity/              # JPA entities
│   │       └── mapper/              # Object mappers
│   └── src/test/                    # Test suite
├── ayushmaanbhav-ledger/            # Double-entry bookkeeping
├── ayushmaanbhav-ledger-commons/    # Ledger API interfaces
├── ayushmaanbhav-payment-common/    # Payment API interfaces
├── ayushmaanbhav-gateway-provider/  # Payment gateway integration
├── ayushmaanbhav-gateway-provider-common/  # Gateway abstractions
├── ayushmaanbhav-gateway-provider-setu/    # SETU implementation
├── ayushmaanbhav-event-processor/   # Async event handling
├── ayushmaanbhav-request-idempotency/  # Idempotency management
└── pom.xml                          # Parent Maven POM
```

## 🔄 Payment Flow

```
┌──────────┐     ┌─────────────┐     ┌──────────────┐     ┌───────────┐
│  Client  │────▶│   Payment   │────▶│   Gateway    │────▶│  External │
│          │     │   Service   │     │   Provider   │     │  Gateway  │
└──────────┘     └─────────────┘     └──────────────┘     └───────────┘
                        │                    │                   │
                        │  Check             │  Create           │
                        │  Idempotency       │  Payment          │
                        ▼                    ▼                   │
                 ┌─────────────┐      ┌──────────────┐          │
                 │ Idempotency │      │   Provider   │          │
                 │   Service   │      │   (SETU)     │          │
                 └─────────────┘      └──────────────┘          │
                                            │                   │
                                            │◀──────────────────┘
                                            │  Webhook Callback
                                            ▼
                 ┌─────────────┐      ┌──────────────┐
                 │   Ledger    │◀─────│    Event     │
                 │   Service   │      │  Processor   │
                 └─────────────┘      └──────────────┘
                        │
                        │  Record Transaction
                        ▼
                 ┌─────────────┐
                 │   MySQL     │
                 │  (Ledger)   │
                 └─────────────┘
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Development Guidelines

- Follow [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
- Write unit tests for all new features
- Update documentation for API changes
- Ensure all tests pass before submitting PR

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Spring Boot](https://spring.io/projects/spring-boot) - Application framework
- [Liquibase](https://www.liquibase.org/) - Database version control
- [Lombok](https://projectlombok.org/) - Boilerplate reduction
- [SETU](https://setu.co/) - Payment gateway integration

## 📞 Support

- **Documentation**: [Wiki](https://github.com/ayushmaanbhav/Payments-Management-System/wiki)
- **Issues**: [GitHub Issues](https://github.com/ayushmaanbhav/Payments-Management-System/issues)
- **Discussions**: [GitHub Discussions](https://github.com/ayushmaanbhav/Payments-Management-System/discussions)

---

<div align="center">

**[⬆ Back to Top](#-payments-management-system)**

Made with ❤️ by [Ayushmaanbhav](https://github.com/ayushmaanbhav)

</div>
