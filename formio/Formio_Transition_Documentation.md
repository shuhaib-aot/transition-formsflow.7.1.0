# Form.io - Transition Documentation

## 1. Overview

**What is Form.io?**

Form.io is a form and data management platform that handles form definitions and form submissions on the server side. It's built using **Node.js**, **Express**, and uses **MongoDB** as the backend database.

In the context of the **formsflow.ai** application, we are using a customized fork of Form.io to better suit our application's needs. We've made several changes to enhance support for multi-tenancy, role-based access, and submission data handling.

## 2. Local Development Setup

### 2.1 Cloning and Installing

Run the following commands:

```bash
git clone https://github.com/AOT-Technologies/formio.git
cd formio
npm install
```

### 2.2 Environment Variables

Copy and configure the environment variables from `sample.env`:

```env
# MongoDB credentials
FORMIO_DB_USERNAME=admin
FORMIO_DB_PASSWORD=changeme
FORMIO_DB_DATABASE=formio

# Root admin user
FORMIO_ROOT_EMAIL=admin@example.com
FORMIO_ROOT_PASSWORD=changeme

# Server config
FORMIO_DEFAULT_PROJECT_URL=http://{your-ip-address}:3001
FORMIO_JWT_SECRET=--- change me now ---
FORMIO_JWT_EXPIRE=240
# Multi-tenancy and UI installation
MULTI_TENANCY_ENABLED=false
NO_INSTALL=1  #this will handle the formsflow-forms ui
```

### 2.3 Running with Docker

To run with Docker:

```bash
docker-compose up --build
```

Ensure ports and environment variables are properly mapped.

## 3. Key Concepts in Form.io

### 3.1 Roles

Form.io allows defining custom roles, e.g., `formsflow-designer`, `formsflow-reviewer`. Each role has a unique ID and can be used to define access levels in both form access and submission access.

**Example use-case:**
- `formsflow-designer` can create/edit forms but cannot submit.

Define this in:

```json
{
  "access": [
    { "type": "read", "roles": ["<role-id>"] }
  ]
}
```

### 3.2 Form

A **Form** is a collection of components (fields) that users interact with. It is JSON-driven and stored in the database.

### 3.3 Submission

A **Submission** is the actual data submitted by users when they fill and submit a form. It is stored and associated with its form.

## 4. Customizations in Our Fork

We’ve made the following customizations:

- Restricted access to the `/access` route using a custom middleware for security.
- Added a `tenantKey` to the form schema for multi-tenancy support.
- Created a `/checkpoint` endpoint to verify if the server is running.
- Added `NO_INSTALL` env variable to control UI installation.
- Included `Client.zip` directly in the repo instead of downloading it.
- Added `skip-sanitize` header to bypass schema validation on submission.
- Created `/form/:formId/metadata` to return simplified form metadata with turn some details instead of full component:
```
 {
  name: com.key,
  label: com.label,
  type: com.type
}
```
- Fixed bundle submission auth logic to return only current form’s data instead full submission, here we need to pass the formid as parameter then it will return the data against that form.
- Enhanced `/form` endpoint with `formIds` query param (handled in `handleFormlist.js`) it will return the form details against the ids in formIds query parameter.
- Introduced `displayForRole` property (used in `removeProtectedFields`, `alterCurrentSubmissionWithPrevious`) to restrict submission data access against a perticular field.
- Added `/forms/search` endpoint to filter by `path`, `name`, or `title` (in `FormListingByPathTitleName.js`) it will return the form details based on `select` query parameter.

 [postman collection] (https://documenter.getpostman.com/view/684631/formio-api-documentation/2Jvuks)

 ## How formio communicate with formsflow.ai and token handling

https://excalidraw.com/#json=3OHWDA-vZPHe8yADA1Ewr,JJIe7ZZjHrCtZVGvY3-NiA
