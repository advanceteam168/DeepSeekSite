# Get Document Details

This API endpoint retrieves detailed information about a document, including its complete audit trail, validation history, and processing details.

## Endpoint

```
GET /api/v1/documents/{documentId}/details
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |
| Accept | application/json |

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| documentId | string | Unique identifier of the document | Yes |

## Query Parameters

| Parameter | Type | Description | Required | Default |
|-----------|------|-------------|-----------|---------|
| includeAuditTrail | boolean | Include complete audit trail | No | false |
| includeValidation | boolean | Include validation history | No | false |
| includeProcessing | boolean | Include processing details | No | false |
| includeSignatures | boolean | Include signature details | No | false |

## Response

### Success Response (200 OK)

```json
{
  "document": {
    "id": "string",
    "type": "string",
    "version": "string",
    "number": "string",
    "status": "string",
    "createdAt": "string",
    "lastModifiedAt": "string"
  },
  "auditTrail": [
    {
      "timestamp": "string",
      "action": "string",
      "actor": {
        "tin": "string",
        "name": "string",
        "role": "string"
      },
      "details": {
        "field": "string",
        "oldValue": "string",
        "newValue": "string",
        "reason": "string"
      },
      "ipAddress": "string",
      "userAgent": "string"
    }
  ],
  "validation": {
    "history": [
      {
        "timestamp": "string",
        "version": "string",
        "result": {
          "valid": true,
          "errors": [
            {
              "code": "string",
              "message": "string",
              "severity": "string",
              "field": "string",
              "value": "string",
              "rule": "string"
            }
          ],
          "warnings": [
            {
              "code": "string",
              "message": "string",
              "field": "string",
              "value": "string",
              "rule": "string"
            }
          ]
        }
      }
    ],
    "rules": [
      {
        "code": "string",
        "description": "string",
        "severity": "string",
        "category": "string"
      }
    ]
  },
  "processing": {
    "submissions": [
      {
        "id": "string",
        "timestamp": "string",
        "status": "string",
        "steps": [
          {
            "name": "string",
            "status": "string",
            "startedAt": "string",
            "completedAt": "string",
            "duration": 0,
            "details": "string",
            "errors": [
              {
                "code": "string",
                "message": "string",
                "stack": "string"
              }
            ]
          }
        ]
      }
    ],
    "retries": [
      {
        "timestamp": "string",
        "reason": "string",
        "attempt": 0,
        "result": "string"
      }
    ]
  },
  "signatures": [
    {
      "type": "string",
      "value": "string",
      "algorithm": "string",
      "certificate": {
        "subject": "string",
        "issuer": "string",
        "validFrom": "string",
        "validTo": "string",
        "serialNumber": "string"
      },
      "timestamp": "string",
      "status": "string",
      "verification": {
        "timestamp": "string",
        "valid": true,
        "errors": [
          {
            "code": "string",
            "message": "string"
          }
        ]
      }
    }
  ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| document | object | Basic document information |
| auditTrail | array | Complete history of document changes |
| validation | object | Document validation history and rules |
| processing | object | Document processing attempts and details |
| signatures | array | Digital signature details and verification |

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
  "message": "Not authorized to view document details"
}
```

#### 404 Not Found

```json
{
  "error": "not_found",
  "message": "Document not found"
}
```

## Example

### Request

```bash
curl "https://api.myinvois.hasil.gov.my/api/v1/documents/doc-001/details?includeAuditTrail=true&includeValidation=true" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Accept: application/json"
```

### Response

```json
{
  "document": {
    "id": "doc-001",
    "type": "01",
    "version": "1.1",
    "number": "INV-2024-001",
    "status": "COMPLETED",
    "createdAt": "2024-03-14T10:30:00Z",
    "lastModifiedAt": "2024-03-14T10:30:05Z"
  },
  "auditTrail": [
    {
      "timestamp": "2024-03-14T10:30:00Z",
      "action": "CREATE",
      "actor": {
        "tin": "123456789",
        "name": "ABC Trading Sdn Bhd",
        "role": "ISSUER"
      },
      "details": {
        "field": "status",
        "oldValue": null,
        "newValue": "PENDING",
        "reason": "Document creation"
      },
      "ipAddress": "192.168.1.100",
      "userAgent": "PostmanRuntime/7.32.3"
    },
    {
      "timestamp": "2024-03-14T10:30:05Z",
      "action": "UPDATE",
      "actor": {
        "tin": "123456789",
        "name": "ABC Trading Sdn Bhd",
        "role": "ISSUER"
      },
      "details": {
        "field": "status",
        "oldValue": "PENDING",
        "newValue": "COMPLETED",
        "reason": "Processing completed"
      },
      "ipAddress": "192.168.1.100",
      "userAgent": "PostmanRuntime/7.32.3"
    }
  ],
  "validation": {
    "history": [
      {
        "timestamp": "2024-03-14T10:30:02Z",
        "version": "1.1",
        "result": {
          "valid": true,
          "errors": [],
          "warnings": [
            {
              "code": "W001",
              "message": "Amount exceeds typical range",
              "field": "totalAmount",
              "value": "1060.00",
              "rule": "amount_range_check"
            }
          ]
        }
      }
    ],
    "rules": [
      {
        "code": "W001",
        "description": "Check if amount is within typical range",
        "severity": "WARNING",
        "category": "BUSINESS_RULES"
      }
    ]
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Only document issuer and authorized auditors can view complete details
3. Audit trail includes:
   - All document changes
   - User actions
   - System operations
   - IP addresses and user agents
4. Validation history shows:
   - All validation attempts
   - Applied validation rules
   - Errors and warnings
5. Processing details include:
   - All submission attempts
   - Processing steps
   - Error details
   - Retry information
6. Signature information includes:
   - Digital signatures
   - Certificate details
   - Verification results
7. Data retention:
   - Audit trail: 7 years
   - Validation history: 1 year
   - Processing details: 90 days
8. Use query parameters to optimize response size
9. Timestamps are in UTC (ISO 8601 format)
10. IP addresses are logged for security audit 