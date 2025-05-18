# Cancel Document

This API endpoint allows cancellation of a previously submitted document in the MyInvois System.

## Endpoint

```
POST /api/v1/documents/{documentId}/cancel
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |
| Content-Type | application/json |

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| documentId | string | Unique identifier of the document to cancel | Yes |

## Request Body

```json
{
  "reason": "string",
  "remarks": "string",
  "signature": {
    "value": "string",
    "algorithm": "string",
    "certificate": "string"
  }
}
```

### Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| reason | string | Cancellation reason code | Yes |
| remarks | string | Additional remarks about cancellation | No |
| signature | object | Digital signature for cancellation | Yes |

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
  "processedAt": "string"
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| id | string | Unique cancellation request identifier |
| documentId | string | Document identifier |
| status | string | Cancellation status |
| reason | string | Cancellation reason code |
| remarks | string | Additional remarks |
| requestedBy | object | Cancellation requester information |
| requestedAt | string | Request timestamp |
| processedAt | string | Processing completion timestamp |

### Error Responses

#### 400 Bad Request

```json
{
  "error": "invalid_request",
  "message": "Invalid cancellation reason",
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
  "message": "Not authorized to cancel this document"
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
  "message": "Document already cancelled"
}
```

## Example

### Request

```bash
curl -X POST https://api.myinvois.hasil.gov.my/api/v1/documents/doc-001/cancel \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "reason": "01",
    "remarks": "Customer requested cancellation",
    "signature": {
      "value": "MEUCIQDx6POWBWr...",
      "algorithm": "SHA256withRSA",
      "certificate": "MIID6zCCA..."
    }
  }'
```

### Response

```json
{
  "id": "can-001",
  "documentId": "doc-001",
  "status": "COMPLETED",
  "reason": "01",
  "remarks": "Customer requested cancellation",
  "requestedBy": {
    "tin": "123456789",
    "name": "ABC Trading Sdn Bhd"
  },
  "requestedAt": "2024-03-14T10:30:00Z",
  "processedAt": "2024-03-14T10:30:05Z"
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Only the document issuer can cancel their own documents
3. Documents can only be cancelled within the allowed timeframe (typically 7 days)
4. Cancelled documents cannot be uncancelled
5. A digital signature is required to prove cancellation authorization
6. Both parties (issuer and recipient) will be notified of the cancellation
7. Cancellation reason codes must be from the standard code list 