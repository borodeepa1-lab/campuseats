# CampusEats — Assignment 4
## Order Service — Part A: REST Resource Design

## 1. Existing SOAP Operations

The existing Order Service design contains these operations:

1. `addToCart()`
2. `placeOrder()`
3. `getOrderStatus()`
4. `cancelOrder()`

These operations come from the existing CampusEats service design.

---

## 2. REST Resource Mapping

The REST design represents durable resources with nouns rather than SOAP-style operation names.

| Old SOAP Operation | REST Resource | REST Endpoint |
|---|---|---|
| `placeOrder()` | orders | `POST /orders` |
| `getOrderStatus()` | orders | `GET /orders/{orderId}` |
| Filter orders | orders | `GET /orders?status={status}` |
| `cancelOrder()` | order cancellation | `POST /orders/{orderId}/cancellation` |

### Final Assignment 4 Endpoints

| Method | URL | What it does | Success Code | Failure Codes |
|---|---|---|---|---|
| POST | `/orders` | Creates an order from a cart | 201 | 400, 404, 409, 422 |
| GET | `/orders/{orderId}` | Returns one order and its status | 200 | 404 |
| GET | `/orders?status={status}` | Returns orders filtered by status | 200 | 400 |
| POST | `/orders/{orderId}/cancellation` | Cancels an existing order | 200 | 404, 409 |

### Notes on the status codes

- `201 Created` is used when a new order is successfully created. The response must include a `Location` header pointing to the new order.
- `200 OK` is used when an existing order is successfully read or cancelled.
- `400 Bad Request` is used for a malformed request or invalid query input.
- `404 Not Found` is used when the requested order does not exist.
- `409 Conflict` is used when the requested state change conflicts with the current order state.
- `422 Unprocessable Entity` is available for a syntactically valid request that the Order Service refuses because of a domain rule.

---

## 3. A5 — Difficult REST Mapping

The `cancelOrder()` operation was the least comfortable operation to map from SOAP to REST because it changes the state of an existing order. We represented it as `POST /orders/{orderId}/cancellation`, treating cancellation as a sub-resource associated with the order. We rejected `POST /cancelOrder` because the URL would represent an operation rather than a resource. This also satisfies the assignment requirement for a state-changing sub-resource.

---

## 4. Relationship to the Existing Order Service

The Order Service is the central service for placing orders. Its existing design owns carts, cart items, orders, and order items. The `placeOrder()` operation takes the user's cart, delivery address, payment method, and optional coupon and turns the cart into a persisted order.

The REST design keeps this same service boundary rather than moving order data into another service.

---

# Part B — To Be Added

This section will be completed by the other team members.

## OpenAPI

- Final `openapi.yaml`
- OpenAPI validation evidence

## Part C — Implementation

- Implementation details
- Validation
- Error handling
- Idempotency
- Tests

## Part D — Network

- Outbound service call
- Timeout and retry strategy
- Fallback when the dependency is unavailable

---

# Assignment Questions

## Question 1 — WSDL vs OpenAPI

To be completed after the final `openapi.yaml` is available.

- Count the lines in the Assignment 3 WSDL.
- Count the lines in `openapi.yaml`.
- Explain what makes up the difference.
- Name two things the WSDL declared that OpenAPI does not need.

## Question 2 — SOAP Fault

To be completed using the actual Assignment 3 SOAP fault and the final REST error response.

## Question 3 — UDDI

To be completed using the team's Assignment 3 UDDI work.

Discuss which of `publish`, `find`, and `bind` still exist in the REST setup and what took over the job of the ones that disappeared.

## Question 4 — XML Schema Validation

To be completed by the implementation member.

Name the `validate()` function used by the REST service and give one failure that would get through without it.

## Question 5 — SOAP vs REST

To be completed by the team.

Identify one part of the service where SOAP would still be preferred and state the exact guarantee being purchased.
