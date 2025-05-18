# Get Notification

This API endpoint retrieves detailed information about a specific notification from the MyInvois System.

## Endpoint

```
GET /api/v1/notifications/{id}
```

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| id | string | Notification identifier | Yes |

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |

## Response

### Success Response (200 OK)

```json
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
    },
    "documentTypes": [
      {
        "code": "string",
        "version": "string"
      }
    ],
    "regions": [
      "string"
    ]
  },
  "history": [
    {
      "timestamp": "string",
      "event": "string",
      "details": "string"
    }
  ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
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
| metadata.documentTypes | array | Affected document types |
| metadata.documentTypes[].code | string | Document type code |
| metadata.documentTypes[].version | string | Document type version |
| metadata.regions | array | Affected geographical regions |
| history | array | Notification history |
| history[].timestamp | string | Event timestamp (ISO 8601) |
| history[].event | string | Event type |
| history[].details | string | Event details |

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
  "message": "Notification not found"
}
```

## Example

### Request

```bash
curl -X GET https://api.myinvois.hasil.gov.my/api/v1/notifications/maint-2024-001 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
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
    },
    {
      "title": "System Status",
      "url": "https://status.myinvois.hasil.gov.my"
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
    },
    "regions": [
      "ALL"
    ]
  },
  "history": [
    {
      "timestamp": "2024-03-01T10:00:00Z",
      "event": "CREATED",
      "details": "Maintenance notification created"
    },
    {
      "timestamp": "2024-03-10T15:30:00Z",
      "event": "UPDATED",
      "details": "Updated maintenance window"
    }
  ]
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. The response includes complete details about the notification, including its history
3. The metadata field contains additional information specific to the notification type
4. Links provide related resources and documentation
5. History tracks changes and updates to the notification 