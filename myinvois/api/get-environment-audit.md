# Get Environment Audit

This API endpoint retrieves audit logs for a specific environment in the MyInvois System.

## Endpoint

```
GET /api/v1/environments/{id}/audit
```

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| id | string | Environment identifier | Yes |

## Query Parameters

| Parameter | Type | Description | Required | Default |
|-----------|------|-------------|-----------|---------|
| from | string | Start time (ISO 8601) | No | 7 days ago |
| to | string | End time (ISO 8601) | No | Current time |
| action | string | Action type filter | No | All actions |
| user | string | User identifier filter | No | All users |
| resource | string | Resource type filter | No | All resources |
| status | string | Status filter (SUCCESS, FAILURE) | No | All statuses |
| limit | integer | Maximum number of records to return | No | 100 |
| offset | integer | Number of records to skip | No | 0 |
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
    "total": 0,
    "byAction": {
      "action": 0
    },
    "byStatus": {
      "SUCCESS": 0,
      "FAILURE": 0
    },
    "byResource": {
      "resource": 0
    },
    "byUser": {
      "user": 0
    }
  },
  "records": [
    {
      "id": "string",
      "timestamp": "string",
      "action": "string",
      "status": "string",
      "user": {
        "id": "string",
        "name": "string",
        "type": "string",
        "roles": [
          "string"
        ]
      },
      "resource": {
        "type": "string",
        "id": "string",
        "name": "string"
      },
      "request": {
        "method": "string",
        "path": "string",
        "ip": "string",
        "userAgent": "string",
        "parameters": {},
        "headers": {}
      },
      "response": {
        "statusCode": 0,
        "body": {},
        "headers": {}
      },
      "changes": [
        {
          "field": "string",
          "oldValue": {},
          "newValue": {}
        }
      ],
      "metadata": {
        "environment": "string",
        "version": "string",
        "correlationId": "string",
        "tags": [
          "string"
        ]
      }
    }
  ],
  "insights": {
    "commonActions": [
      {
        "action": "string",
        "count": 0,
        "successRate": "string"
      }
    ],
    "userActivity": [
      {
        "user": "string",
        "actions": 0,
        "resources": [
          "string"
        ]
      }
    ],
    "resourceAccess": [
      {
        "resource": "string",
        "accessCount": 0,
        "uniqueUsers": 0
      }
    ]
  }
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| timeRange | object | Time range for audit records |
| timeRange.from | string | Start time (ISO 8601) |
| timeRange.to | string | End time (ISO 8601) |
| pagination | object | Pagination information |
| pagination.limit | integer | Maximum records per page |
| pagination.offset | integer | Number of records skipped |
| pagination.total | integer | Total number of records |
| summary | object | Audit summary statistics |
| summary.total | integer | Total number of records |
| summary.byAction | object | Record count by action |
| summary.byStatus | object | Record count by status |
| summary.byResource | object | Record count by resource |
| summary.byUser | object | Record count by user |
| records | array | List of audit records |
| records[].id | string | Unique record identifier |
| records[].timestamp | string | Event timestamp |
| records[].action | string | Action performed |
| records[].status | string | Action status |
| records[].user | object | User information |
| records[].resource | object | Resource information |
| records[].request | object | Request details |
| records[].response | object | Response details |
| records[].changes | array | Field changes |
| records[].metadata | object | Additional metadata |
| insights | object | Audit analysis insights |
| insights.commonActions | array | Frequently performed actions |
| insights.userActivity | array | User activity summary |
| insights.resourceAccess | array | Resource access patterns |

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
curl -X GET "https://api.myinvois.hasil.gov.my/api/v1/environments/prod-1/audit?from=2024-03-07T00:00:00Z&to=2024-03-14T00:00:00Z&action=document_submission" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
{
  "timeRange": {
    "from": "2024-03-07T00:00:00Z",
    "to": "2024-03-14T00:00:00Z"
  },
  "pagination": {
    "limit": 100,
    "offset": 0,
    "total": 1
  },
  "summary": {
    "total": 1,
    "byAction": {
      "document_submission": 1
    },
    "byStatus": {
      "SUCCESS": 1
    },
    "byResource": {
      "document": 1
    },
    "byUser": {
      "user-123": 1
    }
  },
  "records": [
    {
      "id": "audit-001",
      "timestamp": "2024-03-14T08:30:00Z",
      "action": "document_submission",
      "status": "SUCCESS",
      "user": {
        "id": "user-123",
        "name": "John Doe",
        "type": "taxpayer",
        "roles": [
          "document_submitter"
        ]
      },
      "resource": {
        "type": "document",
        "id": "doc-456",
        "name": "Invoice-2024-001"
      },
      "request": {
        "method": "POST",
        "path": "/api/v1/documents",
        "ip": "192.168.1.100",
        "userAgent": "MyInvois-Client/1.0",
        "parameters": {
          "documentType": "invoice",
          "version": "1.1"
        },
        "headers": {
          "content-type": "application/json"
        }
      },
      "response": {
        "statusCode": 201,
        "body": {
          "documentId": "doc-456",
          "status": "submitted"
        },
        "headers": {
          "content-type": "application/json"
        }
      },
      "changes": [
        {
          "field": "status",
          "oldValue": null,
          "newValue": "submitted"
        }
      ],
      "metadata": {
        "environment": "production",
        "version": "1.5.0",
        "correlationId": "corr-789",
        "tags": [
          "invoice",
          "submission"
        ]
      }
    }
  ],
  "insights": {
    "commonActions": [
      {
        "action": "document_submission",
        "count": 150,
        "successRate": "99.33%"
      }
    ],
    "userActivity": [
      {
        "user": "user-123",
        "actions": 50,
        "resources": [
          "document",
          "notification"
        ]
      }
    ],
    "resourceAccess": [
      {
        "resource": "document",
        "accessCount": 500,
        "uniqueUsers": 25
      }
    ]
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Use query parameters to filter audit records by various criteria
3. The response includes summary statistics and access patterns
4. Each record contains detailed information about the action performed
5. Change tracking shows modifications to resources
6. Insights help identify usage patterns and potential security issues 