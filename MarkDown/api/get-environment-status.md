# Get Environment Status

This API endpoint retrieves real-time status information about a specific environment in the MyInvois System.

## Endpoint

```
GET /api/v1/environments/{id}/status
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
  "status": "string",
  "timestamp": "string",
  "components": [
    {
      "name": "string",
      "type": "string",
      "status": "string",
      "message": "string",
      "metrics": {
        "latency": "string",
        "successRate": "string",
        "errorRate": "string"
      }
    }
  ],
  "incidents": {
    "active": [
      {
        "id": "string",
        "title": "string",
        "severity": "string",
        "startTime": "string",
        "lastUpdate": "string",
        "affectedComponents": [
          "string"
        ],
        "updates": [
          {
            "timestamp": "string",
            "status": "string",
            "message": "string"
          }
        ]
      }
    ],
    "resolved": [
      {
        "id": "string",
        "title": "string",
        "severity": "string",
        "startTime": "string",
        "resolvedTime": "string",
        "affectedComponents": [
          "string"
        ],
        "resolution": "string"
      }
    ]
  },
  "metrics": {
    "uptime": {
      "last24Hours": "string",
      "last7Days": "string",
      "last30Days": "string"
    },
    "performance": {
      "averageResponseTime": "string",
      "p95ResponseTime": "string",
      "p99ResponseTime": "string"
    },
    "traffic": {
      "requestsPerMinute": 0,
      "activeUsers": 0
    }
  },
  "maintenance": {
    "ongoing": {
      "startTime": "string",
      "endTime": "string",
      "progress": "string",
      "status": "string"
    },
    "upcoming": [
      {
        "startTime": "string",
        "endTime": "string",
        "description": "string",
        "impact": "string"
      }
    ]
  }
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| status | string | Overall environment status (OPERATIONAL, DEGRADED, MAINTENANCE, DOWN) |
| timestamp | string | Status report timestamp (ISO 8601) |
| components | array | Status of individual system components |
| components[].name | string | Component name |
| components[].type | string | Component type |
| components[].status | string | Component status |
| components[].message | string | Status message |
| components[].metrics | object | Component performance metrics |
| components[].metrics.latency | string | Average latency |
| components[].metrics.successRate | string | Success rate percentage |
| components[].metrics.errorRate | string | Error rate percentage |
| incidents | object | System incidents information |
| incidents.active | array | Currently active incidents |
| incidents.active[].id | string | Incident identifier |
| incidents.active[].title | string | Incident title |
| incidents.active[].severity | string | Incident severity |
| incidents.active[].startTime | string | Incident start time |
| incidents.active[].lastUpdate | string | Last update time |
| incidents.active[].affectedComponents | array | Affected component names |
| incidents.active[].updates | array | Incident updates |
| incidents.resolved | array | Recently resolved incidents |
| metrics | object | System performance metrics |
| metrics.uptime | object | Uptime statistics |
| metrics.performance | object | Performance statistics |
| metrics.traffic | object | Traffic statistics |
| maintenance | object | Maintenance information |
| maintenance.ongoing | object | Currently ongoing maintenance |
| maintenance.upcoming | array | Scheduled maintenance windows |

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
curl -X GET https://api.myinvois.hasil.gov.my/api/v1/environments/prod-1/status \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
{
  "status": "OPERATIONAL",
  "timestamp": "2024-03-14T08:30:00Z",
  "components": [
    {
      "name": "API Gateway",
      "type": "SERVICE",
      "status": "OPERATIONAL",
      "message": "All systems operational",
      "metrics": {
        "latency": "45ms",
        "successRate": "99.99%",
        "errorRate": "0.01%"
      }
    },
    {
      "name": "Document Processing",
      "type": "SERVICE",
      "status": "OPERATIONAL",
      "message": "Processing within normal parameters",
      "metrics": {
        "latency": "120ms",
        "successRate": "99.95%",
        "errorRate": "0.05%"
      }
    }
  ],
  "incidents": {
    "active": [],
    "resolved": [
      {
        "id": "INC-2024-001",
        "title": "Increased API Latency",
        "severity": "MINOR",
        "startTime": "2024-03-13T15:00:00Z",
        "resolvedTime": "2024-03-13T16:30:00Z",
        "affectedComponents": [
          "API Gateway"
        ],
        "resolution": "Scaled up API Gateway instances to handle increased load"
      }
    ]
  },
  "metrics": {
    "uptime": {
      "last24Hours": "100%",
      "last7Days": "99.99%",
      "last30Days": "99.95%"
    },
    "performance": {
      "averageResponseTime": "85ms",
      "p95ResponseTime": "150ms",
      "p99ResponseTime": "200ms"
    },
    "traffic": {
      "requestsPerMinute": 1200,
      "activeUsers": 450
    }
  },
  "maintenance": {
    "ongoing": null,
    "upcoming": [
      {
        "startTime": "2024-03-15T22:00:00Z",
        "endTime": "2024-03-16T02:00:00Z",
        "description": "Scheduled system upgrade",
        "impact": "System will be unavailable"
      }
    ]
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. The response provides real-time status information about the environment
3. Component status helps identify specific service issues
4. Incident tracking provides visibility into current and past issues
5. Performance metrics help monitor system health
6. Maintenance information keeps users informed about planned downtime