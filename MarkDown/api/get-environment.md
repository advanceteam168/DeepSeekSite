# Get Environment

This API endpoint retrieves detailed information about a specific environment in the MyInvois System.

## Endpoint

```
GET /api/v1/environments/{id}
```

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| id | string | Environment identifier | Yes |

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |

## Response

### Success Response (200 OK)

```json
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
    },
    "storage": {
      "maxDocumentSize": "string",
      "retentionPeriod": "string"
    }
  },
  "maintenance": {
    "scheduled": false,
    "window": {
      "start": "string",
      "end": "string"
    },
    "history": [
      {
        "start": "string",
        "end": "string",
        "description": "string"
      }
    ]
  },
  "endpoints": [
    {
      "path": "string",
      "method": "string",
      "description": "string",
      "status": "string",
      "version": "string",
      "deprecated": false,
      "deprecationDate": "string"
    }
  ],
  "documentation": {
    "swagger": "string",
    "postman": "string",
    "guides": [
      {
        "title": "string",
        "url": "string",
        "version": "string"
      }
    ]
  },
  "metrics": {
    "uptime": "string",
    "lastIncident": "string",
    "availability": "string"
  },
  "support": {
    "email": "string",
    "phone": "string",
    "hours": "string",
    "timezone": "string"
  }
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
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
| limits.storage | object | Storage limitations |
| limits.storage.maxDocumentSize | string | Maximum size per document |
| limits.storage.retentionPeriod | string | Document retention period |
| maintenance | object | Maintenance information |
| maintenance.scheduled | boolean | Whether maintenance is scheduled |
| maintenance.window | object | Scheduled maintenance window |
| maintenance.window.start | string | Start time (ISO 8601) |
| maintenance.window.end | string | End time (ISO 8601) |
| maintenance.history | array | Past maintenance records |
| endpoints | array | List of available endpoints |
| endpoints[].path | string | Endpoint path |
| endpoints[].method | string | HTTP method |
| endpoints[].description | string | Endpoint description |
| endpoints[].status | string | Endpoint status |
| endpoints[].version | string | API version |
| endpoints[].deprecated | boolean | Whether endpoint is deprecated |
| endpoints[].deprecationDate | string | When endpoint will be removed |
| documentation | object | Environment documentation |
| documentation.swagger | string | Swagger/OpenAPI specification URL |
| documentation.postman | string | Postman collection URL |
| documentation.guides | array | Additional documentation |
| documentation.guides[].title | string | Guide title |
| documentation.guides[].url | string | Guide URL |
| documentation.guides[].version | string | Documentation version |
| metrics | object | Environment performance metrics |
| metrics.uptime | string | Current uptime duration |
| metrics.lastIncident | string | Last incident timestamp |
| metrics.availability | string | Availability percentage |
| support | object | Support contact information |
| support.email | string | Support email address |
| support.phone | string | Support phone number |
| support.hours | string | Support hours |
| support.timezone | string | Support timezone |

### Error Responses

#### 401 Unauthorized

```json
{
  "error": "unauthorized",
  "message": "Invalid or expired token"
}
```

#### 404 Not Found

```json
{
  "error": "not_found",
  "message": "Environment not found"
}
```

## Example

### Request

```bash
curl -X GET https://api.myinvois.hasil.gov.my/api/v1/environments/prod-1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
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
    },
    "storage": {
      "maxDocumentSize": "50MB",
      "retentionPeriod": "7 years"
    }
  },
  "maintenance": {
    "scheduled": true,
    "window": {
      "start": "2024-03-15T22:00:00Z",
      "end": "2024-03-16T02:00:00Z"
    },
    "history": [
      {
        "start": "2024-02-15T22:00:00Z",
        "end": "2024-02-16T01:00:00Z",
        "description": "System upgrade"
      }
    ]
  },
  "endpoints": [
    {
      "path": "/api/v1/documents",
      "method": "POST",
      "description": "Submit documents",
      "status": "ACTIVE",
      "version": "1.0",
      "deprecated": false
    }
  ],
  "documentation": {
    "swagger": "https://api.myinvois.hasil.gov.my/swagger.json",
    "postman": "https://api.myinvois.hasil.gov.my/myinvois-api.postman_collection.json",
    "guides": [
      {
        "title": "Getting Started",
        "url": "https://sdk.myinvois.hasil.gov.my/guides/getting-started",
        "version": "1.0"
      }
    ]
  },
  "metrics": {
    "uptime": "99.9%",
    "lastIncident": "2024-01-15T10:00:00Z",
    "availability": "99.95%"
  },
  "support": {
    "email": "support@myinvois.hasil.gov.my",
    "phone": "+60-3-8888-8888",
    "hours": "Monday-Friday, 9:00-17:00",
    "timezone": "Asia/Kuala_Lumpur"
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. The response includes comprehensive details about the specific environment
3. Storage limits and retention policies are specific to each environment
4. Maintenance history helps track past system updates
5. Performance metrics provide insight into system reliability
6. Support information helps users get assistance when needed 