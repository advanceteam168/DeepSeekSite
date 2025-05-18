# Get Document

This API endpoint retrieves a specific document by its ID from the MyInvois System.

## Endpoint

```
GET /api/v1/documents/{documentId}
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
| format | string | Response format (JSON, XML, PDF) | No | JSON |
| includeMetadata | boolean | Include document metadata | No | false |
| includeHistory | boolean | Include document history | No | false |

## Response

### Success Response (200 OK)

```json
{
  "id": "string",
  "type": "string",
  "version": "string",
  "number": "string",
  "issuer": {
    "tin": "string",
    "name": "string",
    "address": {
      "line1": "string",
      "line2": "string",
      "city": "string",
      "state": "string",
      "postcode": "string",
      "country": "string"
    },
    "contact": {
      "name": "string",
      "email": "string",
      "phone": "string"
    }
  },
  "recipient": {
    "tin": "string",
    "name": "string",
    "address": {
      "line1": "string",
      "line2": "string",
      "city": "string",
      "state": "string",
      "postcode": "string",
      "country": "string"
    },
    "contact": {
      "name": "string",
      "email": "string",
      "phone": "string"
    }
  },
  "content": {
    "issueDate": "string",
    "dueDate": "string",
    "currency": "string",
    "items": [
      {
        "code": "string",
        "description": "string",
        "quantity": 0,
        "unitPrice": 0,
        "taxRate": 0,
        "taxAmount": 0,
        "totalAmount": 0
      }
    ],
    "summary": {
      "totalBeforeTax": 0,
      "totalTaxAmount": 0,
      "totalAmount": 0
    }
  },
  "status": "string",
  "signature": {
    "value": "string",
    "algorithm": "string",
    "certificate": "string",
    "timestamp": "string"
  },
  "metadata": {
    "sourceSystem": "string",
    "correlationId": "string",
    "tags": [
      "string"
    ]
  },
  "history": [
    {
      "action": "string",
      "status": "string",
      "timestamp": "string",
      "actor": {
        "tin": "string",
        "name": "string"
      },
      "remarks": "string"
    }
  ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| id | string | Document identifier |
| type | string | Document type code |
| version | string | Document type version |
| number | string | Document number |
| issuer | object | Document issuer information |
| recipient | object | Document recipient information |
| content | object | Document content and line items |
| status | string | Document status |
| signature | object | Digital signature information |
| metadata | object | Additional document metadata |
| history | array | Document action history |

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
  "message": "Not authorized to view this document"
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
curl "https://api.myinvois.hasil.gov.my/api/v1/documents/doc-001?format=JSON&includeMetadata=true&includeHistory=true" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Accept: application/json"
```

### Response

```json
{
  "id": "doc-001",
  "type": "01",
  "version": "1.1",
  "number": "INV-2024-001",
  "issuer": {
    "tin": "123456789",
    "name": "ABC Trading Sdn Bhd",
    "address": {
      "line1": "123 Jalan Business",
      "line2": "Taman Enterprise",
      "city": "Kuala Lumpur",
      "state": "WP Kuala Lumpur",
      "postcode": "50000",
      "country": "MY"
    },
    "contact": {
      "name": "John Doe",
      "email": "john.doe@abctrading.com",
      "phone": "+60312345678"
    }
  },
  "recipient": {
    "tin": "987654321",
    "name": "XYZ Industries Sdn Bhd",
    "address": {
      "line1": "456 Jalan Manufacturing",
      "line2": "Kawasan Industri",
      "city": "Shah Alam",
      "state": "Selangor",
      "postcode": "40000",
      "country": "MY"
    },
    "contact": {
      "name": "Jane Smith",
      "email": "jane.smith@xyzind.com",
      "phone": "+60387654321"
    }
  },
  "content": {
    "issueDate": "2024-03-14",
    "dueDate": "2024-04-13",
    "currency": "MYR",
    "items": [
      {
        "code": "ITEM-001",
        "description": "Office Supplies",
        "quantity": 10,
        "unitPrice": 100.00,
        "taxRate": 6,
        "taxAmount": 60.00,
        "totalAmount": 1060.00
      }
    ],
    "summary": {
      "totalBeforeTax": 1000.00,
      "totalTaxAmount": 60.00,
      "totalAmount": 1060.00
    }
  },
  "status": "COMPLETED",
  "signature": {
    "value": "MEUCIQDx6POWBWr...",
    "algorithm": "SHA256withRSA",
    "certificate": "MIID6zCCA...",
    "timestamp": "2024-03-14T10:30:00Z"
  },
  "metadata": {
    "sourceSystem": "ERP-001",
    "correlationId": "erp-inv-001",
    "tags": [
      "march-2024",
      "regular-customer"
    ]
  },
  "history": [
    {
      "action": "SUBMIT",
      "status": "COMPLETED",
      "timestamp": "2024-03-14T10:30:00Z",
      "actor": {
        "tin": "123456789",
        "name": "ABC Trading Sdn Bhd"
      },
      "remarks": "Document submitted successfully"
    }
  ]
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Only document issuer and recipient can view the document
3. Available formats:
   - JSON: Default structured format
   - XML: Alternative structured format
   - PDF: Human-readable format
4. Document content structure varies by document type and version
5. Digital signature can be used to verify document authenticity
6. History includes all actions performed on the document
7. Metadata can be used for integration with external systems
8. All monetary values are in the specified currency
9. Tax calculations follow Malaysian GST/SST rules
10. Contact information is used for notifications 