# REST (Representational State Transfer)

## 1. Simple Definition

Imagine you have a bookstore and customers request books using simple, standardized questions: "Show me all books" (GET), "Add a new book" (POST), "Update this book's price" (PUT), "Remove this book" (DELETE). Rest is a predictable, consistent way to ask for and share information. **REST is a set of simple rules for how software systems should communicate over the internet, using standard actions (GET, POST, PUT, DELETE) on standard resources (like customers, products, or data tables) instead of custom, complex instructions.**

---

## 2. Technical Definition (Data Engineering Context)

REST is an **architectural style for designing web APIs** that uses HTTP methods (GET, POST, PUT, DELETE) and resource-based URLs to perform operations on data. In data engineering, REST APIs provide **stateless, scalable data access** where each request contains all information needed, enabling independent scaling of servers, caching at multiple levels, and predictable interaction patterns. REST is the foundation for **self-service data platforms, microservices communication, and real-time data consumption**.

---

## 3. Real-World Analogy

**Scenario: Library Evolution**

- **Old library (no REST):** To get a book, you write a detailed custom letter to the librarian. "Please find me the red book about cooking that was published in 2019." Every request is unique and confusing. Librarians must interpret each letter differently.

- **Modern library (with REST):** You use a standardized form:
  - **GET /books?category=cooking&year=2019** → "Show me all cooking books from 2019"
  - **POST /books** → "Add a new book to the library"
  - **PUT /books/123** → "Update book ID 123's information"
  - **DELETE /books/456** → "Remove book ID 456"

**Mapping to Data Engineering:**
- **Library books** = Data resources (customers, transactions, inventory)
- **GET, POST, PUT, DELETE** = Standard operations on data
- **Book ID (123)** = Resource identifier (primary key)
- **Query parameters** = Filters (?category=cooking&year=2019)
- **Librarian** = Backend server/API
- **Standardized form** = Predictable, scalable communication

Without REST, every consumer writes custom code. With REST, everyone uses the same simple patterns.

---

## 4. Why This Concept Is Important in Data Engineering

- **Predictability:** All APIs work the same way. Once you understand one, others are intuitive. Reduces learning curve for new tools.

- **Scalability:** REST is stateless—each request is independent. Servers don't need to remember previous requests, so you can easily add more servers.

- **Caching:** REST uses HTTP caching standards. Responses can be cached at CDNs, intermediate servers, and browsers, reducing database load significantly.

- **Self-Service Data Access:** Engineers and analysts query data APIs without manual extraction. No bottleneck on central data team.

- **Decoupling:** Applications depend on REST contracts, not database internals. You can change implementation without breaking consumers.

- **Microservices Communication:** Data pipelines built as microservices use REST to communicate. Easy to understand, debug, and integrate.

- **Real-time Analytics:** REST enables dashboards, reports, and applications to fetch fresh data on-demand instead of waiting for batch jobs.

- **Integration Simplicity:** REST works with standard HTTP—any language, any tool can consume it. No special libraries needed.

---

## 5. Tools & Technologies Used

| **Category** | **Tool/Technology** | **Purpose** |
|---|---|---|
| **REST Frameworks** | Flask, FastAPI, Django REST, Spring Boot | Build REST APIs for data services |
| **API Gateways** | Kong, AWS API Gateway, Nginx | Route, authenticate, and rate-limit REST traffic |
| **Client Libraries** | Requests (Python), curl, Postman | Consume REST APIs from various languages |
| **Documentation** | OpenAPI/Swagger, Postman collections, ReDoc | Auto-generate and share REST API specs |
| **Testing** | Pytest, Jest, Insomnia, RestAssured | Test REST endpoints for correctness |
| **Caching Layer** | Redis, Varnish, CDN | Cache REST responses for performance |
| **Monitoring** | Datadog, New Relic, Prometheus | Track REST API latency, errors, usage |
| **Versioning** | Git, semantic versioning strategies | Manage API versions while maintaining compatibility |
| **Data Virtualization** | Denodo, Informatica, Presto | Expose REST APIs over multiple data sources |
| **Orchestration** | Apache Airflow, Dagster | Trigger and chain REST API calls in pipelines |

---

## 6. One-Line Interview Summary

*"REST is an architectural pattern that uses standard HTTP methods (GET, POST, PUT, DELETE) on resource-based URLs to enable predictable, scalable, and cacheable data access without requiring complex custom instructions."*

---

## Quick Tips for Interviews

- Know the HTTP methods: "GET retrieves data, POST creates new data, PUT updates existing data, DELETE removes data. This consistency makes APIs intuitive and predictable."

- Resource-based thinking: "REST is about resources (nouns), not actions (verbs). Say `/customers/123` (GET to retrieve, PUT to update), not `/getCustomer` or `/updateCustomer`."

- Stateless design: "Each REST request contains all info needed—no server memory of previous requests. This enables scaling—add servers without session sync."

- Status codes matter: "Use 200 (success), 400 (bad request), 401 (unauthorized), 404 (not found), 500 (server error). Proper codes help clients handle errors correctly."

- Real example: "We built a REST API for customer data. GET /customers returned all customers, GET /customers/123 returned one customer, POST /customers created new ones. Analysts queried it directly, eliminating manual data requests."

- Caching advantage: "REST responses with proper cache headers can be cached by CDNs, proxies, and browsers—often reducing database load by 70%+ for read-heavy workloads."

- Pagination is crucial: "Don't return 1 million records in one response. Use pagination: GET /customers?page=1&limit=100. Prevents timeouts and crashes."

- Filtering and sorting: "Support query parameters for filtering (GET /sales?region=US&date=2024-01-01) and sorting (GET /sales?sort=revenue:desc) so clients get exactly what they need."

- Versioning strategy: "Use URL versioning (/v1/customers vs /v2/customers) or header versioning to maintain backward compatibility without breaking existing clients."

- Error responses: "Return consistent error format: {\"error\": \"User not found\", \"code\": \"NOT_FOUND\"}. Helps clients understand what went wrong."

- HATEOAS (advanced): "Include links in responses to guide clients to related resources: {\"id\": 123, \"name\": \"John\", \"_links\": {\"orders\": \"/customers/123/orders\"}}. This makes APIs self-discoverable."
