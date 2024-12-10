
# **Backend User Stories for Model Inventory Project**

This document contains user stories for the backend implementation of the **Model Inventory Project**, covering all aspects as per the design document.

---

## **Epic: Core Model Management**

### **Story 1: Create Model Info API**
**Description:**  
As a backend developer, I want to implement CRUD APIs for managing `model_info` so that users can create, retrieve, update, and delete model data.

**Acceptance Criteria:**
- `POST /api/models`: Creates a new model, validating mandatory fields (e.g., `model_name`, `model_jira_id`).
- `GET /api/models`: Retrieves all models with optional filters (`framework_id`, `model_name`, `lob_id`).
- `GET /api/models/{id}`: Retrieves details of a specific model by ID.
- `PUT /api/models/{id}`: Updates an existing model.
- `DELETE /api/models/{id}`: Deletes a model and cascades deletions for related metadata and roles.

**Priority:** High  

---

### **Story 2: Implement Metadata API**
**Description:**  
As a backend developer, I want to implement APIs for managing metadata so that extended model information can be stored and retrieved.

**Acceptance Criteria:**
- `POST /api/metadata`: Adds or updates metadata for a specific model.
- `GET /api/metadata/{modelId}`: Retrieves metadata for a specific model.
- Validations ensure `modelId` exists before metadata operations are performed.

**Priority:** High  

---

### **Story 3: Implement Model Roles API**
**Description:**  
As a backend developer, I want to implement APIs for managing roles associated with a model so that ownership and responsibilities are tracked.

**Acceptance Criteria:**
- `POST /api/model-roles/{modelId}`: Adds roles (e.g., `model_owner`, `model_sponsor`) to a model.
- `GET /api/model-roles/{modelId}`: Retrieves all roles associated with a model.
- Validation ensures the model exists before assigning roles.

**Priority:** Medium  

---

## **Epic: Supporting Features**

### **Story 4: Create Vendor Management API**
**Description:**  
As a backend developer, I want to implement APIs for managing vendors so that vendor information can be added, updated, and retrieved.

**Acceptance Criteria:**
- `POST /api/vendors`: Creates a new vendor.
- `GET /api/vendors`: Retrieves all vendors.
- Ensures vendor names are unique.

**Priority:** Medium  

---

### **Story 5: Add Lookup APIs**
**Description:**  
As a backend developer, I want to implement APIs for retrieving lookup data so that dropdown options (e.g., frameworks, deployment types) can be fetched dynamically.

**Acceptance Criteria:**
- `GET /api/lookups/frameworks`: Retrieves all frameworks.
- `GET /api/lookups/deployment-types`: Retrieves all deployment types.
- `GET /api/lookups/run-frequencies`: Retrieves all run frequencies.
- Caching is implemented to optimize performance for frequently accessed lookup data.

**Priority:** Medium  

---

### **Story 6: Implement Infrastructure API**
**Description:**  
As a backend developer, I want to implement APIs for managing infrastructure details so that model deployment configurations can be stored and retrieved.

**Acceptance Criteria:**
- `POST /api/infrastructure`: Adds infrastructure details (e.g., `namespace_name`, `cluster_name`).
- `GET /api/infrastructure/{id}`: Retrieves infrastructure details for a model.
- Validation ensures unique infrastructure identifiers (e.g., `namespace_name`).

**Priority:** Medium  

---

## **Epic: Security and Secrets Management**

### **Story 7: Integrate OKTA for Authentication**
**Description:**  
As a backend developer, I want to integrate OKTA for authentication so that APIs are secured, and only authorized users can access them.

**Acceptance Criteria:**
- Validates tokens using OKTA for all APIs.
- Unauthorized requests return a `401 Unauthorized` response.
- Secure routes and scopes are defined in API configurations.

**Priority:** High  

---

### **Story 8: Fetch Secrets from Vault**
**Description:**  
As a backend developer, I want the application to securely retrieve secrets (e.g., database credentials, OKTA keys) from Vault so that sensitive information is protected.

**Acceptance Criteria:**
- Secrets like database credentials, API keys, and client secrets are fetched from Vault during startup.
- Vault integration uses secure tokens to retrieve secrets dynamically.

**Priority:** High  

---

## **Epic: Error Handling and Validation**

### **Story 9: Implement Global Error Handling**
**Description:**  
As a backend developer, I want to implement a global error handler so that consistent error responses are returned for all APIs.

**Acceptance Criteria:**
- Handles exceptions like `EntityNotFoundException`, `ValidationException`, and database errors centrally.
- Error responses include:
  - `timestamp`, `status`, `error`, `message`, and `path`.
- Example response for a missing entity:
  ```json
  {
      "timestamp": "2024-11-19T12:00:00",
      "status": 404,
      "error": "Not Found",
      "message": "Vendor not found with ID: 1",
      "path": "/api/vendors/1"
  }
  ```

**Priority:** High  

---

### **Story 10: Add Validation for API Inputs**
**Description:**  
As a backend developer, I want to add validation for API inputs so that only valid data is accepted by the system.

**Acceptance Criteria:**
- Use Hibernate Validator annotations (`@NotNull`, `@Size`, `@Pattern`) for validating fields in request DTOs.
- Example validations:
  - `model_name`: Must not be null or empty.
  - `vendor_name`: Maximum length of 100 characters.
  - `run_frequency_id`: Must be a valid ID in the `run_frequency_info` table.
- Invalid requests return a `400 Bad Request` with validation errors.

**Priority:** High  

---

## **Epic: Performance and Optimization**

### **Story 11: Implement Query Optimization for Model Retrieval**
**Description:**  
As a backend developer, I want to optimize queries for retrieving models so that API performance is improved for large datasets.

**Acceptance Criteria:**
- Indexes are created for frequently queried columns like `model_name`, `framework_id`, and `deployment_date`.
- Pagination and filtering are implemented in `GET /api/models`.
- Query execution times are measured to ensure sub-second responses.

**Priority:** Medium  

---

## **Epic: Documentation and Monitoring**

### **Story 12: Add Swagger Documentation**
**Description:**  
As a backend developer, I want to document all APIs using Swagger so that developers can easily understand and test the endpoints.

**Acceptance Criteria:**
- Swagger documentation is generated for all APIs.
- API descriptions, parameter details, and example responses are included.
- Swagger UI is hosted at `/swagger-ui`.

**Priority:** Low  

---

### **Story 13: Add Application Health Check API**
**Description:**  
As a backend developer, I want to implement a health check API so that the application readiness and liveness can be monitored.

**Acceptance Criteria:**
- `GET /api/health`:
  - Returns `200 OK` if the application is running.
  - Includes checks for database connection and Vault availability.
- Example response:
  ```json
  {
      "status": "UP",
      "dbConnection": "UP",
      "vault": "UP"
  }
  ```

**Priority:** Medium  

---

## **Summary**
| Epic                       | High Priority                                     | Medium Priority                                   | Low Priority                |
|----------------------------|--------------------------------------------------|-------------------------------------------------|-----------------------------|
| **Core Model Management**  | Model Info API, Metadata API                     | Model Roles API                                 | -                           |
| **Supporting Features**    | -                                                | Vendor API, Lookup APIs, Infrastructure API     | -                           |
| **Security and Secrets**   | OKTA Authentication, Vault Integration           | -                                               | -                           |
| **Error Handling**         | Global Error Handling, Input Validation          | -                                               | -                           |
| **Performance**            | -                                                | Query Optimization                              | -                           |
| **Documentation**          | -                                                | -                                               | Swagger Documentation        |
| **Monitoring**             | -                                                | Health Check API                                | -                           |

