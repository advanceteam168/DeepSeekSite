# Reject Document

This API endpoint allows a document recipient to reject a document they have received in the MyInvois System.

## Endpoint

```
POST /api/v1/documents/{documentId}/reject
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |
| Content-Type | application/json |

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| documentId | string | Unique identifier of the document to reject | Yes |

## Request Body

```json
{
  "reason": "string",
  "remarks": "string",
  "signature": {
    "value": "string",
    "algorithm": "string",
    "certificate": "string"
  },
  "attachments": [
    {
      "name": "string",
      "type": "string",
      "content": "string"
    }
  ]
}
```

### Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| reason | string | Rejection reason code | Yes |
| remarks | string | Additional remarks about rejection | No |
| signature | object | Digital signature for rejection | Yes |
| attachments | array | Supporting documents for rejection | No |

## Response

### Success Response (200 OK)

```json
{
  "id": "string",
  "documentId": "string",
  "status": "string",
  "reason": "string",
  "remarks": "string",
  "requestedBy": {
    "tin": "string",
    "name": "string"
  },
  "requestedAt": "string",
  "processedAt": "string",
  "attachments": [
    {
      "id": "string",
      "name": "string",
      "type": "string",
      "size": 0
    }
  ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| id | string | Unique rejection request identifier |
| documentId | string | Document identifier |
| status | string | Rejection status |
| reason | string | Rejection reason code |
| remarks | string | Additional remarks |
| requestedBy | object | Rejection requester information |
| requestedAt | string | Request timestamp |
| processedAt | string | Processing completion timestamp |
| attachments | array | List of supporting documents |

### Error Responses

#### 400 Bad Request

```json
{
  "error": "invalid_request",
  "message": "Invalid rejection reason",
  "details": [
    {
      "field": "reason",
      "message": "Reason code must be valid"
    }
  ]
}
```

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
  "message": "Not authorized to reject this document"
}
```

#### 404 Not Found

```json
{
  "error": "not_found",
  "message": "Document not found"
}
```

#### 409 Conflict

```json
{
  "error": "conflict",
  "message": "Document already rejected"
}
```

## Example

### Request

```bash
curl -X POST https://api.myinvois.hasil.gov.my/api/v1/documents/doc-001/reject \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "reason": "02",
    "remarks": "Incorrect item quantities",
    "signature": {
      "value": "MEUCIQDx6POWBWr...",
      "algorithm": "SHA256withRSA",
      "certificate": "MIID6zCCA..."
    },
    "attachments": [
      {
        "name": "delivery-note.pdf",
        "type": "application/pdf",
        "content": "JVBERi0xLjcKCjEgMCBvYmo..."
      }
    ]
  }'
```

### Response

```json
{
  "id": "rej-001",
  "documentId": "doc-001",
  "status": "COMPLETED",
  "reason": "02",
  "remarks": "Incorrect item quantities",
  "requestedBy": {
    "tin": "987654321",
    "name": "XYZ Industries Sdn Bhd"
  },
  "requestedAt": "2024-03-14T11:30:00Z",
  "processedAt": "2024-03-14T11:30:05Z",
  "attachments": [
    {
      "id": "att-001",
      "name": "delivery-note.pdf",
      "type": "application/pdf",
      "size": 245678
    }
  ]
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Only the document recipient can reject documents
3. Documents can only be rejected within the allowed timeframe (typically 7 days)
4. Rejected documents cannot be un-rejected
5. A digital signature is required to prove rejection authorization
6. Both parties (issuer and recipient) will be notified of the rejection
7. Rejection reason codes must be from the standard code list
8. Supporting documents can be attached to justify the rejection
9. Maximum attachment size is 10MB per file
10. Supported attachment formats: PDF, JPG, PNG 