# Serverless API Using Azure Functions

## Project Overview

This project demonstrates how to build a **serverless REST API** using **Azure Functions** and an **HTTP Trigger**. The API returns employee information in JSON format without requiring any server management.

The solution is hosted on Azure Functions and can be invoked through an HTTP GET request.

---

## Features

- Serverless architecture
- HTTP-triggered API endpoint
- Returns employee details in JSON format
- Auto-scaling with Azure Functions
- Pay-per-use execution model
- Simple and lightweight implementation

---

## Technologies Used

- Microsoft Azure
- Azure Functions
- Node.js
- JavaScript
- HTTP Trigger
- JSON

---

## Project Architecture

```text
Client/Browser/Postman
          │
          ▼
     HTTP Request
          │
          ▼
   Azure Function App
          │
          ▼
      HelloAPI
          │
          ▼
     JSON Response
```

---

## Function Code

```javascript
module.exports = async function (context, req) {
    const employee = {
        id: 101,
        name: "Neha",
        designation: "SharePoint Technical Analyst"
    };

    context.res = {
        status: 200,
        headers: {
            "Content-Type": "application/json"
        },
        body: employee
    };
};
```

---

## API Endpoint

### Request

```http
GET /api/HelloAPI
```

### Sample Response

```json
{
  "id": 101,
  "name": "Neha",
  "designation": "SharePoint Technical Analyst"
}
```

---

## Deployment Steps

1. Create a Function App in Azure Portal.
2. Select the appropriate runtime stack (Node.js).
3. Create a new HTTP Trigger Function named `HelloAPI`.
4. Replace the default function code with the provided code.
5. Save and deploy the function.
6. Test the API using Azure Portal, Browser, or Postman.
7. Obtain the Function URL and invoke the endpoint.

---

## Testing

### Method

```text
GET
```

### Using Azure Portal

1. Open the Function App.
2. Navigate to `HelloAPI`.
3. Click **Test/Run**.
4. Select **GET** method.
5. Click **Run**.

### Expected Output

```json
{
  "id": 101,
  "name": "Neha",
  "designation": "SharePoint Technical Analyst"
}
```

---

## Benefits of Serverless Computing

- No server management required
- Automatic scaling
- Reduced operational overhead
- Cost-effective pay-per-execution model
- Faster deployment and development

---

## Future Enhancements

- Connect with Azure SQL Database
- Integrate Azure Cosmos DB
- Add CRUD operations
- Implement Authentication and Authorization
- Add API Management
- Add Application Insights monitoring

---

## Author

**Neha Methwani**  
SharePoint Technical Analyst

---

## License

This project is created for learning and demonstration purposes.
