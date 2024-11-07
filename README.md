
# Well Data Upload API

This API allows uploading well and wellbore data from a server to a desktop app, where users can observe, manage, and save this data as a project. Once the data is saved as a project in the app, there is no need to re-upload it unless new data is required. Below are the step-by-step instructions with notes to guide you through the process.

---

## Step 1: Authentication

Before making any requests to the API, you must authenticate using an **API key**. This API key is provided to you when you register or create an account with the service.

### Explanatory Note:
- The API key ensures that only authorized users can access and manipulate the data. If you don’t provide a valid API key, the API will respond with an **Unauthorized** error.
- Include the API key in the `Authorization` header in each request to ensure successful authentication.

### Example Request Header:
```plaintext
Authorization: Bearer <your-api-key>
```

---

## Step 2: Upload Well and Wellbore Data

This step involves sending the **well and wellbore data** from the server to the desktop application.

### Endpoint:
```
POST /data/upload
```

### Explanatory Note:
- The request body must contain information about the wells and wellbores that need to be uploaded to the desktop application.
- The data structure should include the well and wellbore properties like **well ID**, **name**, **depth**, **location**, and **status**.
- This data will then be displayed in the desktop application where you can manage it.

### Request Body (JSON):
```json
{
  "wells": [
    {
      "well_id": "WEL-001",
      "name": "Well 1",
      "location": "Site A",
      "depth": 5000,
      "status": "Active"
    },
    {
      "well_id": "WEL-002",
      "name": "Well 2",
      "location": "Site B",
      "depth": 3000,
      "status": "Inactive"
    }
  ],
  "wellbores": [
    {
      "wellbore_id": "WB-001",
      "well_id": "WEL-001",
      "depth": 1000,
      "status": "Cased",
      "orientation": "Vertical"
    },
    {
      "wellbore_id": "WB-002",
      "well_id": "WEL-002",
      "depth": 1500,
      "status": "Open",
      "orientation": "Horizontal"
    }
  ]
}
```

### Response (Success):
```json
{
  "status": "success",
  "message": "Data uploaded successfully",
  "data": {
    "wells": [
      {
        "well_id": "WEL-001",
        "status": "Active"
      },
      {
        "well_id": "WEL-002",
        "status": "Inactive"
      }
    ],
    "wellbores": [
      {
        "wellbore_id": "WB-001",
        "status": "Cased"
      },
      {
        "wellbore_id": "WB-002",
        "status": "Open"
      }
    ]
  }
}
```

### Response (Error):
```json
{
  "status": "error",
  "message": "Unable to upload data. Please try again later."
}
```

---

## Step 3: Save Data as a Project

Once the data is uploaded to the desktop application, save it as a **project**. This step allows the user to organize and retain the uploaded data for future reference, making it easier to access the data later without needing to upload it again.

### Endpoint:
```
POST /projects/save
```

### Explanatory Note:
- A **project** is a collection of data related to specific wells and wellbores that you can work with in the desktop application. By saving the data as a project, you can continue working on it locally without the need to constantly re-upload it.
- A project has a **name** and a **description** to help you organize multiple projects.

### Request Body (JSON):
```json
{
  "project_name": "Project 1",
  "description": "This is the project for observing Well 1 and Well 2.",
  "wells": [
    "WEL-001",
    "WEL-002"
  ],
  "wellbores": [
    "WB-001",
    "WB-002"
  ]
}
```

### Response (Success):
```json
{
  "status": "success",
  "message": "Project saved successfully",
  "project_id": "PRJ-001"
}
```

### Response (Error):
```json
{
  "status": "error",
  "message": "Failed to save project. Please check the data."
}
```

---

## Step 4: Retrieve Data for Existing Project

You can retrieve the data associated with an existing project to view or work with it in the desktop application.

### Endpoint:
```
GET /projects/{project_id}/data
```

### Explanatory Note:
- Once you save a project, you can access its data by sending a GET request with the **project_id**.
- The data returned will include all the wells and wellbores that are part of the project, allowing you to observe or update the data in your app.

### Response (Success):
```json
{
  "status": "success",
  "message": "Project data retrieved successfully",
  "project": {
    "project_id": "PRJ-001",
    "project_name": "Project 1",
    "wells": [
      {
        "well_id": "WEL-001",
        "name": "Well 1",
        "location": "Site A",
        "depth": 5000,
        "status": "Active"
      },
      {
        "well_id": "WEL-002",
        "name": "Well 2",
        "location": "Site B",
        "depth": 3000,
        "status": "Inactive"
      }
    ],
    "wellbores": [
      {
        "wellbore_id": "WB-001",
        "well_id": "WEL-001",
        "depth": 1000,
        "status": "Cased",
        "orientation": "Vertical"
      },
      {
        "wellbore_id": "WB-002",
        "well_id": "WEL-002",
        "depth": 1500,
        "status": "Open",
        "orientation": "Horizontal"
      }
    ]
  }
}
```

### Response (Error):
```json
{
  "status": "error",
  "message": "Project not found."
}
```

---

## Step 5: Handling Errors

If there is any issue with the API requests, you may encounter the following error codes:

| Code | Description |
|------|-------------|
| 400  | **Bad Request**: The request is invalid, often due to missing or incorrect data. |
| 401  | **Unauthorized**: The API key is missing or invalid. Ensure the key is provided and correct. |
| 404  | **Not Found**: The requested resource (e.g., project or well) does not exist. |
| 500  | **Internal Server Error**: An unexpected error occurred on the server side. |

### Explanatory Note:
- Always check the error code and message to troubleshoot and fix the issue. The **400** and **404** errors are typically caused by incorrect or missing data in the request, while **500** errors are typically server-side issues.

---

## Notes:
- **Data Persistence**: Once data is uploaded and saved as a project in the desktop app, it will persist locally. There is no need to re-upload the data unless new information is available.
- **Data Integrity**: Ensure that the data provided for wells and wellbores is valid, accurate, and complete to avoid errors during the upload process.

---

By following these steps and using the provided explanatory notes, you can effectively upload, manage, and save well and wellbore data through the API.
