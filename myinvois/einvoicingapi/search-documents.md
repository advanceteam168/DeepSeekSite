# Search Documents

This API endpoint allows searching for documents in the MyInvois System using various criteria and filters.

## Endpoint

```
GET /api/v1/documents/search
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |
| Accept | application/json |

## Query Parameters

| Parameter | Type | Description | Required | Default |
|-----------|------|-------------|-----------|---------|
| role | string | Filter by role (ISSUER, RECIPIENT) | No | Both |
| type | string | Filter by document type | No | All |
| version | string | Filter by document version | No | All |
| status | string | Filter by document status | No | All |
| number | string | Search by document number | No | - |
| fromDate | string | Start date (ISO 8601) | No | - |
| toDate | string | End date (ISO 8601) | No | - |
| minAmount | number | Minimum total amount | No | - |
| maxAmount | number | Maximum total amount | No | - |
| currency | string | Filter by currency | No | All |
| counterpartyTin | string | Filter by counterparty TIN | No | - |
| tags | array | Filter by metadata tags | No | - |
| page | integer | Page number | No | 1 |
| size | integer | Page size | No | 20 |
| sort | string | Sort field and direction | No | submittedAt:desc |

## Response

### Success Response (200 OK)

```json
{
  "content": [
    {
      "id": "string",
      "type": "string",
      "version": "string",
      "number": "string",
      "issuer": {
        "tin": "string",
        "name": "string"
      },
      "recipient": {
        "tin": "string",
        "name": "string"
      },
      "amount": {
        "currency": "string",
        "value": 0
      },
      "status": "string",
      "submittedAt": "string",
      "metadata": {
        "sourceSystem": "string",
        "correlationId": "string",
        "tags": [
          "string"
        ]
      }
    }
  ],
  "facets": {
    "types": [
      {
        "value": "string",
        "count": 0
      }
    ],
    "statuses": [
      {
        "value": "string",
        "count": 0
      }
    ],
    "currencies": [
      {
        "value": "string",
        "count": 0
      }
    ]
  },
  "pageable": {
    "page": 0,
    "size": 0,
    "totalElements": 0,
    "totalPages": 0,
    "sort": [
      {
        "field": "string",
        "direction": "string"
      }
    ]
  }
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| content | array | List of document summaries |
| facets | object | Search result aggregations |
| pageable | object | Pagination information |

### Error Responses

#### 400 Bad Request

```json
{
  "error": "invalid_request",
  "message": "Invalid search criteria",
  "details": [
    {
      "field": "fromDate",
      "message": "Must be in ISO 8601 format"
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
curl "https://api.myinvois.hasil.gov.my/api/v1/documents/search?role=ISSUER&type=01&status=COMPLETED&fromDate=2024-03-01T00:00:00Z&toDate=2024-03-31T23:59:59Z&minAmount=1000&currency=MYR&page=1&size=10&sort=submittedAt:desc" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Accept: application/json"
```

### Response

```json
{
  "content": [
    {
      "id": "doc-001",
      "type": "01",
      "version": "1.1",
      "number": "INV-2024-001",
      "issuer": {
        "tin": "123456789",
        "name": "ABC Trading Sdn Bhd"
      },
      "recipient": {
        "tin": "987654321",
        "name": "XYZ Industries Sdn Bhd"
      },
      "amount": {
        "currency": "MYR",
        "value": 1060.00
      },
      "status": "COMPLETED",
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
  ],
  "facets": {
    "types": [
      {
        "value": "01",
        "count": 1
      }
    ],
    "statuses": [
      {
        "value": "COMPLETED",
        "count": 1
      }
    ],
    "currencies": [
      {
        "value": "MYR",
        "count": 1
      }
    ]
  },
  "pageable": {
    "page": 1,
    "size": 10,
    "totalElements": 1,
    "totalPages": 1,
    "sort": [
      {
        "field": "submittedAt",
        "direction": "desc"
      }
    ]
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Search results are limited to documents the user has access to
3. Date range is limited to maximum of 31 days
4. Maximum page size is 100 records
5. Available sort fields:
   - submittedAt
   - number
   - amount
   - status
6. Sort directions: asc, desc
7. Document types:
   - 01: Invoice
   - 02: Credit Note
   - 03: Debit Note
   - 04: Refund Note
8. Status values:
   - PENDING
   - PROCESSING
   - COMPLETED
   - FAILED
   - CANCELLED
   - REJECTED
9. Facets provide count aggregations for quick filtering
10. Use metadata tags for custom categorization and filtering 