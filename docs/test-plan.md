# Test Plan: ReqRes API

## 1. Objective

Validate the functional correctness, authentication behavior, and negative
handling of the ReqRes REST API across all available endpoints and HTTP
methods using a real persistent data environment.

## 2. Scope

### In Scope
- All 6 operations across 2 endpoint paths
- API key authentication via `x-api-key` header
- Positive, negative, boundary, and authorization test cases
- Response status codes, response body structure, and field-level assertions
- Data persistence validation (created records retrievable after creation)

### Out of Scope
- UI testing
- Performance or load testing
- Database-level validation
- App-user session auth / Bearer token flow (covered in later projects)
- Automated code-based testing (covered in Phase 5)

## 3. Test Types

| Type | Description |
|---|---|
| Functional | Does each endpoint return the correct response and data for valid input? |
| Negative | Does the API handle missing fields, wrong data types, and invalid IDs correctly? |
| Authentication | Does the API correctly reject requests with missing or invalid API keys? |
| Boundary | Do edge-case values (non-existent UUIDs, malformed IDs, empty bodies) behave correctly? |

## 4. Tools

| Tool | Purpose |
|---|---|
| Postman | Manual test execution and scripting |
| Newman | CLI-based collection runner |
| GitHub Actions | CI/CD pipeline to run Newman on push |
| Newman HTMLextra | HTML report generation |

## 5. Entry Criteria

- ReqRes API is reachable (verified via GET /api/collections returning 200)
- Valid API key is available and stored as an environment variable
- Postman collection and environment file are created with base URL and project_id variables set
- Test cases are written before execution begins

## 6. Exit Criteria

- All planned test cases have been executed
- All unexpected responses are documented as bug reports with full request/response evidence
- Postman collection and environment exported and pushed to GitHub
- Newman runs successfully via GitHub Actions on push
- HTML report generated and uploaded as a CI artifact
- README and test-cases.md are complete

## 7. Risks and Assumptions

| Risk | Mitigation |
|---|---|
| API key must be kept out of the public repo | Store as GitHub Actions secret and Postman environment variable; clear current value before export |
| Records created during testing persist in the shared project | Clean up created records at the end of each test run using DELETE |
| Record IDs are UUIDs generated at creation time | Save created record ID dynamically via post-response script; do not hardcode |
| Rate limit of 250 requests/day on free tier | Keep test suite lean; do not run collection runner repeatedly in short intervals |