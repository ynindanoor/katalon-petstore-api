# katalon-petstore-api

## Project Overview
This project focuses on API automation testing for the Swagger Petstore API (https://petstore.swagger.io/) using Katalon Studio. 
The project covers four API endpoints for pet management: creating, updating, retrieving, and deleting pet records. Positive and negative test scenarios are implemented to validate API responses, status codes, and response body data.
An end-to-end (E2E) flow is also implemented to verify the complete pet lifecycle, from creation to deletion.

## Objectives
- Automate API testing using Katalon Studio.
- Test four API endpoints with positive and negative scenarios.
- Implement at least one end-to-end testing flow.
- Use dynamic data and Global Variables instead of hardcoded values.
- Validate HTTP status codes and response bodies using assertions.
- Execute all test cases through a Test Suite.
- Generate an execution report after the Test Suite finishes.

## API Endpoints
| Method | Endpoint | Description |
|---|---|---|
| POST | `/pet` | Create a new pet |
| PUT | `/pet` | Update an existing pet |
| GET | `/pet/{petId}` | Retrieve a pet by ID |
| DELETE | `/pet/{petId}` | Delete a pet by ID |

## Test Scenarios
### 1. POST /pet — Create a Pet
| Test Case ID | Test Scenario | Type |
|---|---|---|
| TC001 | Create a pet with valid request data | Positive |
| TC005 | Create a pet with invalid or incomplete request data | Negative |

**Assertions:**
- Verify the expected HTTP status code.
- Verify the response contains the expected pet ID.
- Validate the response body against the submitted data.

### 2. PUT /pet — Update a Pet
| Test Case ID | Test Scenario | Type |
|---|---|---|
| TC003 | Update an existing pet with valid data | Positive |
| TC007 | Update a pet with invalid or incomplete request data | Negative |

**Assertions:**
- Verify the HTTP status code.
- Verify the pet ID and updated values.
- Validate the response body against the expected update.

### 3. GET /pet/{petId} — Retrieve a Pet
| Test Case ID | Test Scenario | Type |
|---|---|---|
| TC002 | Retrieve an existing pet using a valid ID | Positive |
| TC006 | Retrieve a nonexistent or invalid pet ID | Negative |

**Assertions:**
- Verify the HTTP status code.
- Verify the returned pet ID and details.
- Validate the response for nonexistent or invalid IDs.

### 4. DELETE /pet/{petId} — Delete a Pet
| Test Case ID | Test Scenario | Type |
|---|---|---|
| TC004 | Delete an existing pet | Positive |
| TC008 | Delete a nonexistent or invalid pet ID | Negative |

**Assertions:**
- Verify the HTTP status code.
- Validate the deletion response.
- Verify the deleted pet can no longer be retrieved, where supported by the API behavior.

## End-to-End (E2E) Testing
The E2E flow validates the complete pet lifecycle by chaining the four API endpoints.

**E2E Flow: POST → GET → PUT → DELETE**
1. **Create Pet:** Send a POST request to create a pet.
2. **Retrieve Pet:** Use the returned pet ID to retrieve the created pet.
3. **Update Pet:** Send a PUT request to update the pet's details.
4. **Delete Pet:** Send a DELETE request using the same pet ID.

The pet ID is extracted dynamically from the POST response and stored in `GlobalVariable.petId`. This value is reused in subsequent requests without hardcoding the ID.

## Global Variables and Dynamic Data
The project uses the following Global Variables in Katalon Studio.

| Global Variable | Purpose |
|---|---|
| `GlobalVariable.baseUrl` | Stores the Swagger Petstore API base URL |
| `GlobalVariable.petId` | Stores the dynamically extracted pet ID |

### 1. baseUrl
The `baseUrl` variable stores the API base URL and is configured in the Katalon execution profile.

Example usage:

```groovy
def requestUrl = GlobalVariable.baseUrl + '/pet'
```

This avoids repeating the base URL throughout the test scripts.

### 2. petId
The `petId` variable stores the ID returned by the POST `/pet` response.

Example:

```groovy
def responseBody = new groovy.json.JsonSlurper()
    .parseText(response.getResponseText())

GlobalVariable.petId = responseBody.id
```

The extracted ID is reused for subsequent requests.

For example, the GET request URL can be constructed as follows:

```groovy
def requestUrl = GlobalVariable.baseUrl +
    '/pet/' + GlobalVariable.petId
```

The same `GlobalVariable.petId` is used to identify the pet during the E2E flow.

**Note:** The pet ID is assigned dynamically from the API response. If the response already returns an Integer, it can be assigned directly without using `Integer.parseInt()`.

## Assertions and Response Validation

Assertions are used to validate both HTTP status codes and response body content.
The validation includes:

- Expected HTTP status codes.
- Pet ID matches the created pet.
- Pet name and status match the expected values.
- Updated values are returned after a PUT request.
- Deletion response matches the API behavior.
- A subsequent GET request confirms the deletion behavior, where applicable.

Example status code assertion:

```groovy
assert response.getStatusCode() == expectedStatusCode
```

The expected status code should be configured according to the endpoint and test scenario.

## Test Suite Execution

All test cases are organized into a Katalon Test Suite.
The Test Suite is used to:

- Execute positive and negative test cases.
- Run the E2E pet lifecycle flow.
- Collect test execution statuses.
- Record failures and execution details.
- Generate an execution report.

## How to Run the Tests
1. Clone the repository from GitHub.
2. Open the project in Katalon Studio.
3. Configure `baseUrl` and `petId` in the execution profile.
4. Verify the API request objects and test scripts.
5. Run the Test Suite.
6. Review the execution results and generated report.

The `baseUrl` should contain the API base URL. The `petId` value is populated dynamically when the relevant test executes.

## Limitations
- The project uses a publicly available demo API.
- Pet records may be shared with other users and modified by external requests.
- Negative test results depend on the API's validation and error-handling behavior.
- Dynamic IDs reduce conflicts but do not guarantee data isolation.

## Conclusion
This project demonstrates API automation testing using Katalon Studio, covering positive and negative scenarios, dynamic data, Global Variables, response assertions, and end-to-end testing.
The dynamic `petId` enables the test flow to reuse the pet created during execution, while the Test Suite provides a centralized way to run tests and review results.

## Disclaimer
This project is intended for educational and demonstration purposes only. It uses the publicly available Swagger Petstore demo API.
