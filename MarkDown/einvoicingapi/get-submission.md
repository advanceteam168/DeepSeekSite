# Get Submission Status

This API endpoint retrieves the status and details of a document submission in the MyInvois System.

## Endpoint

```
GET /api/v1/submissions/{submissionId}
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| submissionId | string | Unique identifier of the submission | Yes |

## Response

### Success Response (200 OK)

```json
{
  "id": "string",
  "status": "string",
  "document": {
    "id": "string",
    "type": "string",
    "version": "string",
    "number": "string"
  },
  "validation": {
    "valid": true,
    "errors": [
      {
        "code": "string",
        "message": "string",
        "severity": "string",
        "field": "string"
      }
    ],
    "warnings": [
      {
        "code": "string",
        "message": "string",
        "field": "string"
      }
    ]
  },
  "processing": {
    "startedAt": "string",
    "completedAt": "string",
    "duration": 0,
    "steps": [
      {
        "name": "string",
        "status": "string",
        "startedAt": "string",
        "completedAt": "string",
        "duration": 0,
        "details": "string"
      }
    ]
  },
  "submitter": {
    "tin": "string",
    "name": "string"
  },
  "submittedAt": "string",
  "metadata": {
    "sourceSystem": "string",
    "correlationId": "string",
    "tags": [
      "string"
    ]
  }
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| id | string | Submission identifier |
| status | string | Overall submission status |
| document | object | Submitted document information |
| validation | object | Document validation results |
| processing | object | Processing details and steps |
| submitter | object | Submission requester information |
| submittedAt | string | Submission timestamp |
| metadata | object | Additional submission metadata |

### Error Responses

#### 401 Unauthorized

```json
{
  "error": "unauthorized",
  "message": "Invalid or expired token"
}
```

#### 403 Forbidden

```json
{
  "error": "forbidden",
  "message": "Not authorized to view this submission"
}
```

#### 404 Not Found

```json
{
  "error": "not_found",
  "message": "Submission not found"
}
```

## Example

### Request

```bash
curl "https://api.myinvois.hasil.gov.my/api/v1/submissions/sub-001" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
{
  "id": "sub-001",
  "status": "COMPLETED",
  "document": {
    "id": "doc-001",
    "type": "01",
    "version": "1.1",
    "number": "INV-2024-001"
  },
  "validation": {
    "valid": true,
    "errors": [],
    "warnings": [
      {
        "code": "W001",
        "message": "Amount exceeds typical range",
        "field": "totalAmount"
      }
    ]
  },
  "processing": {
    "startedAt": "2024-03-14T10:30:00Z",
    "completedAt": "2024-03-14T10:30:05Z",
    "duration": 5000,
    "steps": [
      {
        "name": "validation",
        "status": "COMPLETED",
        "startedAt": "2024-03-14T10:30:00Z",
        "completedAt": "2024-03-14T10:30:02Z",
        "duration": 2000,
        "details": "Document validation successful"
      },
      {
        "name": "storage",
        "status": "COMPLETED",
        "startedAt": "2024-03-14T10:30:02Z",
        "completedAt": "2024-03-14T10:30:03Z",
        "duration": 1000,
        "details": "Document stored successfully"
      },
      {
        "name": "notification",
        "status": "COMPLETED",
        "startedAt": "2024-03-14T10:30:03Z",
        "completedAt": "2024-03-14T10:30:05Z",
        "duration": 2000,
        "details": "Notifications sent to all parties"
      }
    ]
  },
  "submitter": {
    "tin": "123456789",
    "name": "ABC Trading Sdn Bhd"
  },
  "submittedAt": "2024-03-14T10:30:00Z",
  "metadata": {
    "sourceSystem": "ERP-001",
    "correlationId": "erp-inv-001",
    "tags": [
      "march-2024",
      "regular-customer"
    ]
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Only the document issuer can view their submission details
3. Submission status values:
   - PENDING: Submission received but not yet processed
   - PROCESSING: Document is being processed
   - COMPLETED: Processing completed successfully
   - FAILED: Processing failed due to errors
4. Processing steps may vary based on document type and validation rules
5. Validation warnings do not prevent successful submission
6. Processing duration is provided in milliseconds
7. Use the correlationId in metadata to track submissions across systems
8. Historical submission data is retained for 7 years
9. Processing steps are recorded for audit and troubleshooting
10. Real-time status updates are available through the notification system 