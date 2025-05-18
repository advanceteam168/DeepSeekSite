# Get Environment Logs

This API endpoint retrieves system logs for a specific environment in the MyInvois System.

## Endpoint

```
GET /api/v1/environments/{id}/logs
```

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| id | string | Environment identifier | Yes |

## Query Parameters

| Parameter | Type | Description | Required | Default |
|-----------|------|-------------|-----------|---------|
| from | string | Start time (ISO 8601) | No | 1 hour ago |
| to | string | End time (ISO 8601) | No | Current time |
| level | string | Log level filter (DEBUG, INFO, WARN, ERROR) | No | All levels |
| service | string | Service filter | No | All services |
| search | string | Text search query | No | None |
| limit | integer | Maximum number of logs to return | No | 100 |
| offset | integer | Number of logs to skip | No | 0 |
| sort | string | Sort order (timestamp_asc, timestamp_desc) | No | timestamp_desc |

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |

## Response

### Success Response (200 OK)

```json
{
  "timeRange": {
    "from": "string",
    "to": "string"
  },
  "pagination": {
    "limit": 0,
    "offset": 0,
    "total": 0
  },
  "summary": {
    "totalLogs": 0,
    "byLevel": {
      "DEBUG": 0,
      "INFO": 0,
      "WARN": 0,
      "ERROR": 0
    },
    "byService": {
      "service": 0
    }
  },
  "logs": [
    {
      "id": "string",
      "timestamp": "string",
      "level": "string",
      "service": "string",
      "message": "string",
      "context": {
        "requestId": "string",
        "userId": "string",
        "endpoint": "string",
        "method": "string",
        "statusCode": 0,
        "duration": 0
      },
      "metadata": {
        "host": "string",
        "environment": "string",
        "version": "string"
      },
      "error": {
        "code": "string",
        "message": "string",
        "stack": "string",
        "details": {}
      },
      "tags": [
        "string"
      ]
    }
  ],
  "patterns": [
    {
      "pattern": "string",
      "count": 0,
      "firstSeen": "string",
      "lastSeen": "string",
      "services": [
        "string"
      ],
      "levels": [
        "string"
      ]
    }
  ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| timeRange | object | Time range for logs |
| timeRange.from | string | Start time (ISO 8601) |
| timeRange.to | string | End time (ISO 8601) |
| pagination | object | Pagination information |
| pagination.limit | integer | Maximum logs per page |
| pagination.offset | integer | Number of logs skipped |
| pagination.total | integer | Total number of logs |
| summary | object | Log summary statistics |
| summary.totalLogs | integer | Total number of logs |
| summary.byLevel | object | Log count by level |
| summary.byService | object | Log count by service |
| logs | array | List of log entries |
| logs[].id | string | Unique log identifier |
| logs[].timestamp | string | Log timestamp (ISO 8601) |
| logs[].level | string | Log level |
| logs[].service | string | Service name |
| logs[].message | string | Log message |
| logs[].context | object | Request context |
| logs[].context.requestId | string | Unique request identifier |
| logs[].context.userId | string | User identifier |
| logs[].context.endpoint | string | API endpoint |
| logs[].context.method | string | HTTP method |
| logs[].context.statusCode | integer | HTTP status code |
| logs[].context.duration | integer | Request duration (ms) |
| logs[].metadata | object | Additional metadata |
| logs[].metadata.host | string | Server hostname |
| logs[].metadata.environment | string | Environment name |
| logs[].metadata.version | string | Service version |
| logs[].error | object | Error information |
| logs[].error.code | string | Error code |
| logs[].error.message | string | Error message |
| logs[].error.stack | string | Stack trace |
| logs[].error.details | object | Additional error details |
| logs[].tags | array | Log tags |
| patterns | array | Common log patterns |
| patterns[].pattern | string | Log message pattern |
| patterns[].count | integer | Pattern occurrence count |
| patterns[].firstSeen | string | First occurrence time |
| patterns[].lastSeen | string | Last occurrence time |
| patterns[].services | array | Services with pattern |
| patterns[].levels | array | Log levels with pattern |

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
curl -X GET "https://api.myinvois.hasil.gov.my/api/v1/environments/prod-1/logs?from=2024-03-14T07:00:00Z&to=2024-03-14T08:00:00Z&level=ERROR&limit=10" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
{
  "timeRange": {
    "from": "2024-03-14T07:00:00Z",
    "to": "2024-03-14T08:00:00Z"
  },
  "pagination": {
    "limit": 10,
    "offset": 0,
    "total": 3
  },
  "summary": {
    "totalLogs": 3,
    "byLevel": {
      "ERROR": 3
    },
    "byService": {
      "document-service": 2,
      "api-gateway": 1
    }
  },
  "logs": [
    {
      "id": "log-001",
      "timestamp": "2024-03-14T07:15:30Z",
      "level": "ERROR",
      "service": "document-service",
      "message": "Failed to process document: Invalid signature",
      "context": {
        "requestId": "req-001",
        "userId": "user-123",
        "endpoint": "/api/v1/documents",
        "method": "POST",
        "statusCode": 400,
        "duration": 250
      },
      "metadata": {
        "host": "doc-srv-01",
        "environment": "production",
        "version": "1.5.0"
      },
      "error": {
        "code": "DOC-001",
        "message": "Document signature validation failed",
        "stack": "Error: Document signature validation failed\n    at validateSignature (/app/services/document.js:150:23)\n    at processDocument (/app/services/document.js:75:12)",
        "details": {
          "documentId": "doc-123",
          "signatureAlgorithm": "SHA256"
        }
      },
      "tags": [
        "document-processing",
        "signature-validation"
      ]
    }
  ],
  "patterns": [
    {
      "pattern": "Failed to process document: %s",
      "count": 2,
      "firstSeen": "2024-03-14T07:15:30Z",
      "lastSeen": "2024-03-14T07:45:12Z",
      "services": [
        "document-service"
      ],
      "levels": [
        "ERROR"
      ]
    }
  ]
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Use query parameters to filter and paginate logs
3. The response includes summary statistics and common patterns
4. Log entries contain detailed context and error information
5. Pattern analysis helps identify recurring issues
6. Logs are retained according to the environment's retention policy 