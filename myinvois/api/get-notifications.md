# Get Notifications

This API endpoint retrieves system notifications and announcements from the MyInvois System.

## Endpoint

```
GET /api/v1/notifications
```

## Query Parameters

| Parameter | Type | Description | Required | Default |
|-----------|------|-------------|-----------|---------|
| type | string | Filter by notification type (SYSTEM, MAINTENANCE, DOCUMENT) | No | All types |
| status | string | Filter by status (ACTIVE, EXPIRED) | No | ACTIVE |
| from | string | Start date (ISO 8601) | No | 30 days ago |
| to | string | End date (ISO 8601) | No | Current date |
| page | integer | Page number for pagination | No | 1 |
| size | integer | Number of items per page | No | 20 |

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |

## Response

### Success Response (200 OK)

```json
{
  "notifications": [
    {
      "id": "string",
      "type": "string",
      "title": "string",
      "message": "string",
      "severity": "string",
      "startDate": "string",
      "endDate": "string",
      "status": "string",
      "links": [
        {
          "title": "string",
          "url": "string"
        }
      ],
      "metadata": {
        "affectedServices": [
          "string"
        ],
        "maintenanceWindow": {
          "start": "string",
          "end": "string"
        }
      }
    }
  ],
  "pagination": {
    "page": 0,
    "size": 0,
    "totalItems": 0,
    "totalPages": 0
  }
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| notifications | array | List of notifications |
| id | string | Unique notification identifier |
| type | string | Notification type (SYSTEM, MAINTENANCE, DOCUMENT) |
| title | string | Notification title |
| message | string | Detailed notification message |
| severity | string | Notification severity (INFO, WARNING, CRITICAL) |
| startDate | string | Date when notification becomes active (ISO 8601) |
| endDate | string | Date when notification expires (ISO 8601) |
| status | string | Current status (ACTIVE, EXPIRED) |
| links | array | Related links |
| links[].title | string | Link title |
| links[].url | string | Link URL |
| metadata | object | Additional notification metadata |
| metadata.affectedServices | array | List of affected services |
| metadata.maintenanceWindow | object | Maintenance time window |
| pagination | object | Pagination information |
| pagination.page | integer | Current page number |
| pagination.size | integer | Items per page |
| pagination.totalItems | integer | Total number of items |
| pagination.totalPages | integer | Total number of pages |

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
curl -X GET "https://api.myinvois.hasil.gov.my/api/v1/notifications?type=MAINTENANCE&status=ACTIVE" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
{
  "notifications": [
    {
      "id": "maint-2024-001",
      "type": "MAINTENANCE",
      "title": "Scheduled System Maintenance",
      "message": "The MyInvois System will undergo scheduled maintenance. During this period, the system will be unavailable.",
      "severity": "WARNING",
      "startDate": "2024-03-15T22:00:00Z",
      "endDate": "2024-03-16T02:00:00Z",
      "status": "ACTIVE",
      "links": [
        {
          "title": "Maintenance Schedule",
          "url": "https://myinvois.hasil.gov.my/maintenance-schedule"
        }
      ],
      "metadata": {
        "affectedServices": [
          "Document Submission",
          "Document Retrieval",
          "User Portal"
        ],
        "maintenanceWindow": {
          "start": "2024-03-15T22:00:00Z",
          "end": "2024-03-16T02:00:00Z"
        }
      }
    }
  ],
  "pagination": {
    "page": 1,
    "size": 20,
    "totalItems": 1,
    "totalPages": 1
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Use query parameters to filter notifications based on type, status, and date range
3. Notifications are paginated to manage large result sets
4. The metadata field contains additional information specific to the notification type
5. Active notifications are those whose endDate has not yet passed 