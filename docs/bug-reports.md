# Bug Reports: ReqRes API

---

## BUG-001: Empty Data Object Accepted, Record Created with No Fields

| Field | Detail |
|---|---|
| Bug ID | BUG-001 |
| Severity | High |
| Related Test Case | TC-010 |
| Endpoint | POST /api/collections/products/records |
| Environment | https://reqres.in/api |

**Request:**
- Method: POST
- URL: https://reqres.in/api/collections/products/records?project_id={{projectId}}
- Headers: x-api-key: {{apiKey}}, X-Reqres-Env: prod, Content-Type: application/json
- Body:
{
  "data": {}
}

**Actual Response:**
- Status: 201
- Body: Record created with empty `data: {}` object and a valid UUID assigned

**Expected Response:**
- Status: 400
- Body: Error message indicating that required fields are missing

**Description:**
The API accepts an empty data object and creates a record with no fields, returning 201 Created. This means the API has no field validation on the products collection. A record with no data has no business value and should be rejected. This is a data integrity issue, the system silently stores empty records without any indication to the client that the input was incomplete.

**Standard Reference:**
APIs should validate that required fields are present before persisting data. Accepting empty payloads and returning 201 misleads the client into believing a valid resource was created.

---