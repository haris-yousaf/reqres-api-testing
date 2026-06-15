# Test Cases: ReqRes API

## Endpoint Coverage Summary

| Method | Endpoint | Operation |
|---|---|---|
| GET | /collections | List collections |
| GET | /collections/products/records | List all records |
| GET | /collections/products/records/:id | Get single record |
| POST | /collections/products/records | Create record |
| PUT | /collections/products/records/:id | Update record |
| DELETE | /collections/products/records/:id | Delete record |

---

## Required Headers (All Requests)

| Header | Value |
|---|---|
| x-api-key | {{apiKey}} |
| X-Reqres-Env | prod |

Content-Type: application/json is additionally required for POST and PUT.

---

## TC-001: List Collections

| Field | Detail |
|---|---|
| Test Case ID | TC-001 |
| Endpoint | GET /collections |
| Description | Returns all collections for the project |
| Pre-conditions | Valid API key |
| Query Params | project_id={{projectId}} |
| Request Body | None |
| Expected Status | 200 |
| Expected Response | Array containing at least one collection object with a `name` field |
| Type | Functional |

---

## TC-002: List All Records

| Field | Detail |
|---|---|
| Test Case ID | TC-002 |
| Endpoint | GET /collections/products/records |
| Description | Returns all records in the products collection |
| Pre-conditions | Valid API key |
| Query Params | project_id={{projectId}} |
| Request Body | None |
| Expected Status | 200 |
| Expected Response | Object containing `data` array and `meta` object with pagination info |
| Type | Functional |

---

## TC-003: Create Record with Valid Data

| Field | Detail |
|---|---|
| Test Case ID | TC-003 |
| Endpoint | POST /collections/products/records |
| Description | Valid request body creates a new record and returns it with a UUID |
| Pre-conditions | Valid API key |
| Query Params | project_id={{projectId}} |
| Request Body | See below |
| Expected Status | 201 |
| Expected Response | Object containing `data` with an `id` (UUID string) and all submitted fields |
| Type | Functional |

Request body:
```json
{
  "data": {
    "name": "Wireless Headphones",
    "price": 59.99,
    "category": "Electronics",
    "in_stock": true
  }
}
```

---

## TC-004: Get Record by Valid ID

| Field | Detail |
|---|---|
| Test Case ID | TC-004 |
| Endpoint | GET /collections/products/records/:id |
| Description | Returns the full record object for a valid existing UUID |
| Pre-conditions | Valid API key, record created in TC-003 |
| Query Params | project_id={{projectId}} |
| Request Body | None |
| Expected Status | 200 |
| Expected Response | Object containing `data` with all fields matching what was created |
| Type | Functional |

---

## TC-005: Update Record with Valid Data

| Field | Detail |
|---|---|
| Test Case ID | TC-005 |
| Endpoint | PUT /collections/products/records/:id |
| Description | Valid update replaces the record with new data |
| Pre-conditions | Valid API key, record created in TC-003 |
| Query Params | project_id={{projectId}} |
| Request Body | See below |
| Expected Status | 200 |
| Expected Response | Object containing `data` with all updated field values |
| Type | Functional |

Request body:
```json
{
  "data": {
    "name": "Wireless Headphones Pro",
    "price": 89.99,
    "category": "Electronics",
    "in_stock": false
  }
}
```

---

## TC-006: Delete Record with Valid Auth

| Field | Detail |
|---|---|
| Test Case ID | TC-006 |
| Endpoint | DELETE /collections/products/records/:id |
| Description | Authenticated delete removes the record |
| Pre-conditions | Valid API key, record created in TC-003 |
| Query Params | project_id={{projectId}} |
| Request Body | None |
| Expected Status | 204 |
| Expected Response | Empty body |
| Type | Functional / Auth |

---

## TC-007: Get Record After Deletion

| Field | Detail |
|---|---|
| Test Case ID | TC-007 |
| Endpoint | GET /collections/products/records/:id |
| Description | Fetching a deleted record returns 404 |
| Pre-conditions | Record deleted in TC-006 |
| Query Params | project_id={{projectId}} |
| Request Body | None |
| Expected Status | 404 |
| Expected Response | Error response indicating record not found |
| Type | Negative |

---

## TC-008: Get Record by Non-Existent ID

| Field | Detail |
|---|---|
| Test Case ID | TC-008 |
| Endpoint | GET /collections/products/records/:id |
| Description | Valid UUID format but non-existent record returns 404 |
| Pre-conditions | Valid API key |
| URL | /records/00000000-0000-0000-0000-000000000000 |
| Query Params | project_id={{projectId}} |
| Request Body | None |
| Expected Status | 404 |
| Expected Response | Error response indicating record not found |
| Type | Negative |

---

## TC-009: Get Record by Invalid ID Format

| Field | Detail |
|---|---|
| Test Case ID | TC-009 |
| Endpoint | GET /collections/products/records/:id |
| Description | Non-UUID string as ID returns 404 or 400 |
| Pre-conditions | Valid API key |
| URL | /records/invalid-id-format |
| Query Params | project_id={{projectId}} |
| Request Body | None |
| Expected Status | 400 or 404 |
| Expected Response | Error response |
| Type | Negative / Boundary |

---

## TC-010: Create Record with Missing Required Field

| Field | Detail |
|---|---|
| Test Case ID | TC-010 |
| Endpoint | POST /collections/products/records |
| Description | Empty data object returns an error |
| Pre-conditions | Valid API key |
| Query Params | project_id={{projectId}} |
| Request Body | `{ "data": {} }` |
| Expected Status | 400 |
| Actual Status | 201 |
| Expected Response | Error response indicating bad request |
| Actual Response | Record created with empty `data: {}` object and a valid UUID assigned
| Result | Fail - see BUG-001
| Type | Negative |

---

## TC-011: Update Record with Non-Existent ID

| Field | Detail |
|---|---|
| Test Case ID | TC-011 |
| Endpoint | PUT /collections/products/records/:id |
| Description | Updating a non-existent UUID returns 404 |
| Pre-conditions | Valid API key |
| URL | /records/00000000-0000-0000-0000-000000000000 |
| Query Params | project_id={{projectId}} |
| Request Body | Valid update body |
| Expected Status | 404 |
| Expected Response | Error response indicating record not found |
| Type | Negative |

---

## TC-012: Delete Record with Non-Existent ID

| Field | Detail |
|---|---|
| Test Case ID | TC-012 |
| Endpoint | DELETE /collections/products/records/:id |
| Description | Deleting a non-existent UUID returns 404 |
| Pre-conditions | Valid API key |
| URL | /records/00000000-0000-0000-0000-000000000000 |
| Query Params | project_id={{projectId}} |
| Request Body | None |
| Expected Status | 404 |
| Expected Response | Error response indicating record not found |
| Type | Negative |

---

## TC-013: List Records with Missing API Key

| Field | Detail |
|---|---|
| Test Case ID | TC-013 |
| Endpoint | GET /collections/products/records |
| Description | Request without x-api-key header is rejected |
| Pre-conditions | None |
| Query Params | project_id={{projectId}} |
| Headers | x-api-key omitted, X-Reqres-Env: prod |
| Request Body | None |
| Expected Status | 401 |
| Expected Response | Error response indicating missing API key |
| Type | Auth / Negative |

---

## TC-014: List Records with Invalid API Key

| Field | Detail |
|---|---|
| Test Case ID | TC-014 |
| Endpoint | GET /collections/products/records |
| Description | Request with wrong API key value is rejected |
| Pre-conditions | None |
| Query Params | project_id={{projectId}} |
| Headers | x-api-key: invalidkey123, X-Reqres-Env: prod |
| Request Body | None |
| Expected Status | 401 or 403 |
| Expected Response | Error response indicating invalid auth |
| Type | Auth / Negative |

---

## TC-015: Create Record with No Auth

| Field | Detail |
|---|---|
| Test Case ID | TC-015 |
| Endpoint | POST /collections/products/records |
| Description | Create request without API key is rejected |
| Pre-conditions | None |
| Query Params | project_id={{projectId}} |
| Headers | x-api-key omitted |
| Request Body | Valid create body |
| Expected Status | 401 or 403 |
| Expected Response | Error response indicating missing auth |
| Type | Auth / Negative |

---

## TC-016: Update Record with No Auth

| Field | Detail |
|---|---|
| Test Case ID | TC-016 |
| Endpoint | PUT /collections/products/records/:id |
| Description | Update request without API key is rejected |
| Pre-conditions | A valid record ID exists |
| Query Params | project_id={{projectId}} |
| Headers | x-api-key omitted |
| Request Body | Valid update body |
| Expected Status | 401 or 403 |
| Expected Response | Error response indicating missing auth |
| Type | Auth / Negative |

---

## TC-017: Delete Record with No Auth

| Field | Detail |
|---|---|
| Test Case ID | TC-017 |
| Endpoint | DELETE /collections/products/records/:id |
| Description | Delete request without API key is rejected |
| Pre-conditions | A valid record ID exists |
| Query Params | project_id={{projectId}} |
| Headers | x-api-key omitted |
| Request Body | None |
| Expected Status | 401 or 403 |
| Expected Response | Error response indicating missing auth |
| Type | Auth / Negative |