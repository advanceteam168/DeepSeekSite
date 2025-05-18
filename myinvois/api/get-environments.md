# Get Environments

This API endpoint retrieves information about available MyInvois System environments.

## Endpoint

```
GET /api/v1/environments
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |

## Response

### Success Response (200 OK)

```json
{
  "environments": [
    {
      "id": "string",
      "name": "string",
      "type": "string",
      "baseUrl": "string",
      "status": "string",
      "features": [
        "string"
      ],
      "limits": {
        "maxRequestSize": "string",
        "maxDocumentsPerRequest": 0,
        "rateLimit": {
          "requestsPerMinute": 0,
          "burstSize": 0
        }
      },
      "maintenance": {
        "scheduled": false,
        "window": {
          "start": "string",
          "end": "string"
        }
      },
      "endpoints": [
        {
          "path": "string",
          "method": "string",
          "description": "string",
          "status": "string"
        }
      ],
      "documentation": {
        "swagger": "string",
        "postman": "string",
        "guides": [
          {
            "title": "string",
            "url": "string"
          }
        ]
      }
    }
  ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| environments | array | List of available environments |
| id | string | Unique environment identifier |
| name | string | Environment name |
| type | string | Environment type (PRODUCTION, SANDBOX) |
| baseUrl | string | Base URL for API endpoints |
| status | string | Current status (ACTIVE, MAINTENANCE, DEGRADED) |
| features | array | List of supported features |
| limits | object | Environment limitations |
| limits.maxRequestSize | string | Maximum request size (e.g., "10MB") |
| limits.maxDocumentsPerRequest | integer | Maximum documents per batch request |
| limits.rateLimit | object | Rate limiting configuration |
| limits.rateLimit.requestsPerMinute | integer | Maximum requests per minute |
| limits.rateLimit.burstSize | integer | Maximum burst size |
| maintenance | object | Maintenance information |
| maintenance.scheduled | boolean | Whether maintenance is scheduled |
| maintenance.window | object | Scheduled maintenance window |
| maintenance.window.start | string | Start time (ISO 8601) |
| maintenance.window.end | string | End time (ISO 8601) |
| endpoints | array | List of available endpoints |
| endpoints[].path | string | Endpoint path |
| endpoints[].method | string | HTTP method |
| endpoints[].description | string | Endpoint description |
| endpoints[].status | string | Endpoint status |
| documentation | object | Environment documentation |
| documentation.swagger | string | Swagger/OpenAPI specification URL |
| documentation.postman | string | Postman collection URL |
| documentation.guides | array | Additional documentation |
| documentation.guides[].title | string | Guide title |
| documentation.guides[].url | string | Guide URL |

### Error Response (401 Unauthorized)

```json
{
  "error": "unauthorized",
  "message": "Invalid or expired token"
}
```

## Example

### Request

```bash
curl -X GET https://api.myinvois.hasil.gov.my/api/v1/environments \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
{
  "environments": [
    {
      "id": "prod-1",
      "name": "Production",
      "type": "PRODUCTION",
      "baseUrl": "https://api.myinvois.hasil.gov.my",
      "status": "ACTIVE",
      "features": [
        "document-submission",
        "document-validation",
        "digital-signatures",
        "batch-processing"
      ],
      "limits": {
        "maxRequestSize": "10MB",
        "maxDocumentsPerRequest": 1000,
        "rateLimit": {
          "requestsPerMinute": 60,
          "burstSize": 100
        }
      },
      "maintenance": {
        "scheduled": true,
        "window": {
          "start": "2024-03-15T22:00:00Z",
          "end": "2024-03-16T02:00:00Z"
        }
      },
      "endpoints": [
        {
          "path": "/api/v1/documents",
          "method": "POST",
          "description": "Submit documents",
          "status": "ACTIVE"
        }
      ],
      "documentation": {
        "swagger": "https://api.myinvois.hasil.gov.my/swagger.json",
        "postman": "https://api.myinvois.hasil.gov.my/myinvois-api.postman_collection.json",
        "guides": [
          {
            "title": "Getting Started",
            "url": "https://sdk.myinvois.hasil.gov.my/guides/getting-started"
          }
        ]
      }
    },
    {
      "id": "sandbox-1",
      "name": "Sandbox",
      "type": "SANDBOX",
      "baseUrl": "https://sandbox-api.myinvois.hasil.gov.my",
      "status": "ACTIVE",
      "features": [
        "document-submission",
        "document-validation",
        "digital-signatures",
        "batch-processing",
        "test-data-generation"
      ],
      "limits": {
        "maxRequestSize": "10MB",
        "maxDocumentsPerRequest": 100,
        "rateLimit": {
          "requestsPerMinute": 120,
          "burstSize": 200
        }
      },
      "maintenance": {
        "scheduled": false
      },
      "endpoints": [
        {
          "path": "/api/v1/documents",
          "method": "POST",
          "description": "Submit documents",
          "status": "ACTIVE"
        }
      ],
      "documentation": {
        "swagger": "https://sandbox-api.myinvois.hasil.gov.my/swagger.json",
        "postman": "https://sandbox-api.myinvois.hasil.gov.my/myinvois-api.postman_collection.json",
        "guides": [
          {
            "title": "Sandbox Guide",
            "url": "https://sdk.myinvois.hasil.gov.my/guides/sandbox"
          }
        ]
      }
    }
  ]
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. The response includes information about all available environments (Production and Sandbox)
3. Each environment has its own base URL, features, and limitations
4. Documentation links provide access to detailed API specifications and guides
5. Rate limits and request size limits are specific to each environment
6. The sandbox environment typically has more relaxed limits and additional testing features 