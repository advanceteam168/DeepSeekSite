# Get Environment Alerts

This API endpoint retrieves alerts and warnings for a specific environment in the MyInvois System.

## Endpoint

```
GET /api/v1/environments/{id}/alerts
```

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| id | string | Environment identifier | Yes |

## Query Parameters

| Parameter | Type | Description | Required | Default |
|-----------|------|-------------|-----------|---------|
| status | string | Alert status filter (ACTIVE, RESOLVED, ACKNOWLEDGED) | No | ACTIVE |
| severity | string | Alert severity filter (INFO, WARNING, CRITICAL) | No | All severities |
| type | string | Alert type filter | No | All types |
| from | string | Start time (ISO 8601) | No | 24 hours ago |
| to | string | End time (ISO 8601) | No | Current time |
| limit | integer | Maximum number of alerts to return | No | 100 |
| offset | integer | Number of alerts to skip | No | 0 |

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
    "bySeverity": {
      "INFO": 0,
      "WARNING": 0,
      "CRITICAL": 0
    },
    "byStatus": {
      "ACTIVE": 0,
      "RESOLVED": 0,
      "ACKNOWLEDGED": 0
    },
    "byType": {
      "type": 0
    }
  },
  "alerts": [
    {
      "id": "string",
      "type": "string",
      "title": "string",
      "description": "string",
      "severity": "string",
      "status": "string",
      "createdAt": "string",
      "updatedAt": "string",
      "resolvedAt": "string",
      "source": {
        "type": "string",
        "name": "string",
        "component": "string"
      },
      "metrics": {
        "threshold": 0,
        "value": 0,
        "unit": "string"
      },
      "affectedResources": [
        {
          "type": "string",
          "id": "string",
          "name": "string"
        }
      ],
      "actions": [
        {
          "type": "string",
          "status": "string",
          "timestamp": "string",
          "description": "string",
          "user": "string"
        }
      ],
      "notifications": [
        {
          "type": "string",
          "recipient": "string",
          "status": "string",
          "timestamp": "string"
        }
      ],
      "metadata": {
        "priority": "string",
        "category": "string",
        "tags": [
          "string"
        ]
      }
    }
  ],
  "insights": {
    "commonPatterns": [
      {
        "pattern": "string",
        "count": 0,
        "severity": "string",
        "resources": [
          "string"
        ]
      }
    ],
    "trends": {
      "daily": [
        {
          "date": "string",
          "count": 0,
          "bySeverity": {
            "severity": 0
          }
        }
      ],
      "weekly": {
        "trend": "string",
        "percentage": 0
      }
    }
  }
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| timeRange | object | Time range for alerts |
| timeRange.from | string | Start time (ISO 8601) |
| timeRange.to | string | End time (ISO 8601) |
| pagination | object | Pagination information |
| pagination.limit | integer | Maximum alerts per page |
| pagination.offset | integer | Number of alerts skipped |
| pagination.total | integer | Total number of alerts |
| summary | object | Alert summary statistics |
| summary.total | integer | Total number of alerts |
| summary.bySeverity | object | Alert count by severity |
| summary.byStatus | object | Alert count by status |
| summary.byType | object | Alert count by type |
| alerts | array | List of alerts |
| alerts[].id | string | Unique alert identifier |
| alerts[].type | string | Alert type |
| alerts[].title | string | Alert title |
| alerts[].description | string | Detailed description |
| alerts[].severity | string | Alert severity |
| alerts[].status | string | Current status |
| alerts[].createdAt | string | Creation timestamp |
| alerts[].updatedAt | string | Last update timestamp |
| alerts[].resolvedAt | string | Resolution timestamp |
| alerts[].source | object | Alert source information |
| alerts[].metrics | object | Related metrics |
| alerts[].affectedResources | array | Affected resources |
| alerts[].actions | array | Alert actions history |
| alerts[].notifications | array | Notification history |
| alerts[].metadata | object | Additional metadata |
| insights | object | Alert analysis insights |
| insights.commonPatterns | array | Common alert patterns |
| insights.trends | object | Alert trend analysis |

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
curl -X GET "https://api.myinvois.hasil.gov.my/api/v1/environments/prod-1/alerts?status=ACTIVE&severity=CRITICAL" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
{
  "timeRange": {
    "from": "2024-03-13T08:00:00Z",
    "to": "2024-03-14T08:00:00Z"
  },
  "pagination": {
    "limit": 100,
    "offset": 0,
    "total": 1
  },
  "summary": {
    "total": 1,
    "bySeverity": {
      "CRITICAL": 1
    },
    "byStatus": {
      "ACTIVE": 1
    },
    "byType": {
      "resource_utilization": 1
    }
  },
  "alerts": [
    {
      "id": "alert-001",
      "type": "resource_utilization",
      "title": "High CPU Usage",
      "description": "CPU utilization has exceeded 90% threshold for more than 5 minutes",
      "severity": "CRITICAL",
      "status": "ACTIVE",
      "createdAt": "2024-03-14T07:55:00Z",
      "updatedAt": "2024-03-14T08:00:00Z",
      "source": {
        "type": "monitoring",
        "name": "system-metrics",
        "component": "document-service"
      },
      "metrics": {
        "threshold": 90,
        "value": 95,
        "unit": "percentage"
      },
      "affectedResources": [
        {
          "type": "service",
          "id": "doc-srv-01",
          "name": "Document Processing Service"
        }
      ],
      "actions": [
        {
          "type": "notification",
          "status": "completed",
          "timestamp": "2024-03-14T07:55:00Z",
          "description": "Alert notification sent to on-call team",
          "user": "system"
        }
      ],
      "notifications": [
        {
          "type": "email",
          "recipient": "oncall@myinvois.hasil.gov.my",
          "status": "sent",
          "timestamp": "2024-03-14T07:55:00Z"
        }
      ],
      "metadata": {
        "priority": "P1",
        "category": "performance",
        "tags": [
          "cpu",
          "resource",
          "document-service"
        ]
      }
    }
  ],
  "insights": {
    "commonPatterns": [
      {
        "pattern": "High CPU Usage",
        "count": 3,
        "severity": "CRITICAL",
        "resources": [
          "document-service"
        ]
      }
    ],
    "trends": {
      "daily": [
        {
          "date": "2024-03-14",
          "count": 1,
          "bySeverity": {
            "CRITICAL": 1
          }
        }
      ],
      "weekly": {
        "trend": "increasing",
        "percentage": 50
      }
    }
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Use query parameters to filter alerts by status, severity, and type
3. The response includes summary statistics and trend analysis
4. Each alert contains detailed information about the issue and affected resources
5. Action history tracks responses to alerts
6. Insights help identify patterns and trends in system issues 