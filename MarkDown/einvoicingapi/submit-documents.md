# Submit Documents

This API endpoint allows submission of one or more documents (invoices, credit notes, debit notes, etc.) to the MyInvois System.

## Endpoint

```
POST /api/v1/documents
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |
| Content-Type | application/json |

## Request Body

```json
{
  "documents": [
    {
      "type": "string",
      "version": "string",
      "content": {},
      "format": "string",
      "signature": {
        "value": "string",
        "algorithm": "string",
        "certificate": "string"
      },
      "metadata": {
        "sourceSystem": "string",
        "correlationId": "string",
        "tags": [
          "string"
        ]
      }
    }
  ],
  "options": {
    "validateOnly": false,
    "async": false,
    "notifyParties": true
  }
}
```

### Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| documents | array | List of documents to submit | Yes |
| documents[].type | string | Document type code | Yes |
| documents[].version | string | Document type version | Yes |
| documents[].content | object | Document content (JSON or XML) | Yes |
| documents[].format | string | Content format (JSON, XML) | Yes |
| documents[].signature | object | Digital signature information | Yes |
| documents[].metadata | object | Additional document metadata | No |
| options | object | Submission options | No |
| options.validateOnly | boolean | Only validate without submitting | No |
| options.async | boolean | Process asynchronously | No |
| options.notifyParties | boolean | Send notifications to parties | No |

## Response

### Success Response (200 OK)

```json
{
  "submissions": [
    {
      "id": "string",
      "status": "string",
      "document": {
        "id": "string",
        "type": "string",
        "version": "string"
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
        ]
      },
      "timestamp": "string",
      "processingTime": 0
    }
  ],
  "summary": {
    "total": 0,
    "successful": 0,
    "failed": 0,
    "pending": 0
  }
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| submissions | array | List of submission results |
| submissions[].id | string | Unique submission identifier |
| submissions[].status | string | Submission status |
| submissions[].document | object | Submitted document information |
| submissions[].validation | object | Validation results |
| submissions[].timestamp | string | Submission timestamp |
| submissions[].processingTime | number | Processing time in milliseconds |
| summary | object | Submission batch summary |

### Error Responses

#### 400 Bad Request

```json
{
  "error": "invalid_request",
  "message": "Invalid document format",
  "details": [
    {
      "field": "string",
      "message": "string"
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

## Example

### Request

```bash
curl -X POST https://api.myinvois.hasil.gov.my/api/v1/documents \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "documents": [
      {
        "type": "01",
        "version": "1.1",
        "content": {
          "invoiceNumber": "INV-2024-001",
          "invoiceDate": "2024-03-14",
          "supplier": {
            "tin": "123456789"
          },
          "buyer": {
            "tin": "987654321"
          }
        },
        "format": "JSON",
        "signature": {
          "value": "MEUCIQDx6POWBWr...",
          "algorithm": "SHA256withRSA",
          "certificate": "MIID6zCCA..."
        }
      }
    ],
    "options": {
      "validateOnly": false,
      "async": false,
      "notifyParties": true
    }
  }'
```

### Response

```json
{
  "submissions": [
    {
      "id": "sub-001",
      "status": "COMPLETED",
      "document": {
        "id": "doc-001",
        "type": "01",
        "version": "1.1"
      },
      "validation": {
        "valid": true,
        "errors": []
      },
      "timestamp": "2024-03-14T08:30:00Z",
      "processingTime": 1500
    }
  ],
  "summary": {
    "total": 1,
    "successful": 1,
    "failed": 0,
    "pending": 0
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Documents must conform to their respective type and version schemas
3. Digital signatures are mandatory for all document submissions
4. Batch submissions are limited to 1000 documents per request
5. Use validateOnly option to test document validity without submission
6. Async processing is recommended for large batch submissions 