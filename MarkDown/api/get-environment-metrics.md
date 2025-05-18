# Get Environment Metrics

This API endpoint retrieves detailed performance metrics for a specific environment in the MyInvois System.

## Endpoint

```
GET /api/v1/environments/{id}/metrics
```

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| id | string | Environment identifier | Yes |

## Query Parameters

| Parameter | Type | Description | Required | Default |
|-----------|------|-------------|-----------|---------|
| from | string | Start time (ISO 8601) | No | 24 hours ago |
| to | string | End time (ISO 8601) | No | Current time |
| interval | string | Aggregation interval | No | 5m |
| metrics | array | Specific metrics to retrieve | No | All metrics |

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
    "to": "string",
    "interval": "string"
  },
  "summary": {
    "availability": "string",
    "averageResponseTime": "string",
    "totalRequests": 0,
    "errorRate": "string",
    "peakConcurrentUsers": 0
  },
  "timeSeries": {
    "requests": [
      {
        "timestamp": "string",
        "total": 0,
        "successful": 0,
        "failed": 0,
        "byEndpoint": {
          "endpoint": {
            "total": 0,
            "successful": 0,
            "failed": 0
          }
        }
      }
    ],
    "responseTime": [
      {
        "timestamp": "string",
        "average": 0,
        "p50": 0,
        "p90": 0,
        "p95": 0,
        "p99": 0
      }
    ],
    "users": [
      {
        "timestamp": "string",
        "concurrent": 0,
        "active": 0
      }
    ],
    "resources": [
      {
        "timestamp": "string",
        "cpu": {
          "usage": "string",
          "cores": 0
        },
        "memory": {
          "usage": "string",
          "total": "string"
        },
        "storage": {
          "usage": "string",
          "total": "string"
        }
      }
    ]
  },
  "endpoints": [
    {
      "path": "string",
      "method": "string",
      "metrics": {
        "totalRequests": 0,
        "successfulRequests": 0,
        "failedRequests": 0,
        "averageResponseTime": "string",
        "errorRate": "string"
      }
    }
  ],
  "errors": [
    {
      "code": "string",
      "message": "string",
      "count": 0,
      "firstSeen": "string",
      "lastSeen": "string",
      "endpoints": [
        "string"
      ]
    }
  ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| timeRange | object | Time range for metrics |
| timeRange.from | string | Start time (ISO 8601) |
| timeRange.to | string | End time (ISO 8601) |
| timeRange.interval | string | Aggregation interval |
| summary | object | Overall metrics summary |
| summary.availability | string | System availability percentage |
| summary.averageResponseTime | string | Average response time |
| summary.totalRequests | integer | Total number of requests |
| summary.errorRate | string | Overall error rate |
| summary.peakConcurrentUsers | integer | Maximum concurrent users |
| timeSeries | object | Time-series metrics data |
| timeSeries.requests | array | Request metrics over time |
| timeSeries.responseTime | array | Response time metrics over time |
| timeSeries.users | array | User metrics over time |
| timeSeries.resources | array | Resource utilization over time |
| endpoints | array | Per-endpoint metrics |
| endpoints[].path | string | Endpoint path |
| endpoints[].method | string | HTTP method |
| endpoints[].metrics | object | Endpoint-specific metrics |
| errors | array | Error statistics |
| errors[].code | string | Error code |
| errors[].message | string | Error message |
| errors[].count | integer | Number of occurrences |
| errors[].firstSeen | string | First occurrence time |
| errors[].lastSeen | string | Last occurrence time |
| errors[].endpoints | array | Affected endpoints |

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
curl -X GET "https://api.myinvois.hasil.gov.my/api/v1/environments/prod-1/metrics?from=2024-03-13T00:00:00Z&to=2024-03-14T00:00:00Z&interval=1h" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
{
  "timeRange": {
    "from": "2024-03-13T00:00:00Z",
    "to": "2024-03-14T00:00:00Z",
    "interval": "1h"
  },
  "summary": {
    "availability": "99.99%",
    "averageResponseTime": "85ms",
    "totalRequests": 1728000,
    "errorRate": "0.01%",
    "peakConcurrentUsers": 750
  },
  "timeSeries": {
    "requests": [
      {
        "timestamp": "2024-03-13T12:00:00Z",
        "total": 72000,
        "successful": 71993,
        "failed": 7,
        "byEndpoint": {
          "/api/v1/documents": {
            "total": 45000,
            "successful": 44995,
            "failed": 5
          }
        }
      }
    ],
    "responseTime": [
      {
        "timestamp": "2024-03-13T12:00:00Z",
        "average": 85,
        "p50": 75,
        "p90": 120,
        "p95": 150,
        "p99": 200
      }
    ],
    "users": [
      {
        "timestamp": "2024-03-13T12:00:00Z",
        "concurrent": 650,
        "active": 450
      }
    ],
    "resources": [
      {
        "timestamp": "2024-03-13T12:00:00Z",
        "cpu": {
          "usage": "65%",
          "cores": 32
        },
        "memory": {
          "usage": "75%",
          "total": "64GB"
        },
        "storage": {
          "usage": "45%",
          "total": "1TB"
        }
      }
    ]
  },
  "endpoints": [
    {
      "path": "/api/v1/documents",
      "method": "POST",
      "metrics": {
        "totalRequests": 1080000,
        "successfulRequests": 1079892,
        "failedRequests": 108,
        "averageResponseTime": "95ms",
        "errorRate": "0.01%"
      }
    }
  ],
  "errors": [
    {
      "code": "ERR-001",
      "message": "Rate limit exceeded",
      "count": 50,
      "firstSeen": "2024-03-13T10:15:00Z",
      "lastSeen": "2024-03-13T10:45:00Z",
      "endpoints": [
        "/api/v1/documents"
      ]
    }
  ]
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Use query parameters to specify the time range and granularity of metrics
3. The response includes both summary statistics and detailed time-series data
4. Resource utilization metrics help monitor system capacity
5. Error statistics help identify and troubleshoot issues
6. Per-endpoint metrics provide detailed performance insights 