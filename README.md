# ReqRes API Testing

[![ReqRes API Tests](https://github.com/haris-yousaf/reqres-api-testing/actions/workflows/newman.yml/badge.svg)](https://github.com/haris-yousaf/reqres-api-testing/actions/workflows/newman.yml)

Manual and automated API test suite for the [ReqRes API](https://reqres.in) - a real persistent REST API for QA practice, covering full CRUD operations and API key authentication on a live products collection.

---

## Project Summary

| Item | Detail |
|---|---|
| API Under Test | ReqRes (`reqres.in/api`) |
| Collection Tested | Products |
| Total Test Cases | 17 |
| Total Assertions | 45 |
| Bugs Found | 1 |
| Auth Types Tested | API Key (`x-api-key` header) |
| CI/CD | GitHub Actions via Newman |

---

## What Was Tested

| Method | Endpoint | Operation |
|---|---|---|
| GET | /collections | List all collections |
| GET | /collections/products/records | List all records |
| GET | /collections/products/records/:id | Get record by UUID |
| POST | /collections/products/records | Create record |
| PUT | /collections/products/records/:id | Full update record |
| DELETE | /collections/products/records/:id | Delete record |

### Test Types Covered

- Functional: valid input returns correct response, data persists and is retrievable after creation
- Negative: non-existent UUIDs, invalid ID formats, empty request bodies
- Authentication: missing API key (401), invalid API key (403), auth required on all write operations
- Boundary: nil UUID format, empty data object, malformed ID strings

---

## Bugs Found

| Bug ID | Endpoint | Issue | Severity |
|---|---|---|---|
| BUG-001 | POST /collections/products/records | Empty data object accepted, record created with no fields instead of returning 400 | High |

Full bug report with request/response evidence is in [`docs/bug-reports.md`](docs/bug-reports.md).

---

## Repository Structure

```
reqres-api-testing/
├── .github/
│   └── workflows/
│       └── newman.yml          # CI/CD pipeline
├── postman/
│   ├── reqres-collection.json
│   └── reqres-environment.json
├── docs/
│   ├── test-plan.md            # Scope, tools, entry/exit criteria
│   ├── test-cases.md           # All 17 test cases with expected results
│   └── bug-reports.md          # 1 bug with full request/response evidence
├── reports/
│   └── newman-report.html      # Sample HTML report from last run
└── README.md
```

---

## How to Run Locally

**Prerequisites:** Node.js, Newman, newman-reporter-htmlextra

```bash
npm install -g newman newman-reporter-htmlextra
```

**Run the collection:**
```bash
newman run postman/reqres-collection.json \
  -e postman/reqres-environment.json \
  --env-var "x-api-key=YOUR_API_KEY" \
  --env-var "projectId=YOUR_PROJECT_ID" \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/newman-report.html
```

Get your free API key and project ID at [app.reqres.in](https://app.reqres.in).

---

## CI/CD

Tests run automatically on every push via GitHub Actions. The API key and project ID are stored as GitHub Actions secrets and injected at runtime via `--env-var`, keeping credentials out of the repository entirely.

To view the latest run: go to the **Actions** tab and download the `newman-report` artifact.

---

## Key Differences from Project 1

| Area | Restful Booker | ReqRes |
|---|---|---|
| Auth mechanism | Token via Cookie header | API Key via x-api-key header |
| Record IDs | Integer | UUID string |
| Data persistence | Shared demo, resets periodically | Real persistent storage per project |
| Credential handling | Token generated at runtime | API key stored as GitHub Actions secret |

---

## Documentation

- [Test Plan](docs/test-plan.md)
- [Test Cases](docs/test-cases.md)
- [Bug Reports](docs/bug-reports.md)
