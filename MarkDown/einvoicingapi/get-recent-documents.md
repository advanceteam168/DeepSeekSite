# Get Recent Documents

This API endpoint retrieves a list of recently submitted or received documents in the MyInvois System.

## Endpoint

```
GET /api/v1/documents/recent
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |

## Query Parameters

| Parameter | Type | Description | Required | Default |
|-----------|------|-------------|-----------|---------|
| role | string | Filter by role (ISSUER, RECIPIENT) | No | Both |
| type | string | Filter by document type | No | All |
| status | string | Filter by document status | No | All |
| fromDate | string | Start date (ISO 8601) | No | Last 7 days |
| toDate | string | End date (ISO 8601) | No | Current date |
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
      "processedAt": "string",
      "metadata": {
        "sourceSystem": "string",
        "correlationId": "string",
        "tags": [
          "string"
        ]
      }
    }
  ],
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
| content[].id | string | Document identifier |
| content[].type | string | Document type code |
| content[].version | string | Document type version |
| content[].number | string | Document number |
| content[].issuer | object | Document issuer information |
| content[].recipient | object | Document recipient information |
| content[].amount | object | Document total amount |
| content[].status | string | Document status |
| content[].submittedAt | string | Submission timestamp |
| content[].processedAt | string | Processing completion timestamp |
| content[].metadata | object | Additional document metadata |
| pageable | object | Pagination information |

### Error Responses

#### 400 Bad Request

```json
{
  "error": "invalid_request",
  "message": "Invalid date format",
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
curl "https://api.myinvois.hasil.gov.my/api/v1/documents/recent?role=ISSUER&type=01&status=COMPLETED&fromDate=2024-03-07T00:00:00Z&toDate=2024-03-14T23:59:59Z&page=1&size=10&sort=submittedAt:desc" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
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
        "value": 1000.00
      },
      "status": "COMPLETED",
      "submittedAt": "2024-03-14T10:30:00Z",
      "processedAt": "2024-03-14T10:30:05Z",
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
2. Date range is limited to maximum of 31 days
3. Maximum page size is 100 records
4. Results are sorted by submission date in descending order by default
5. Document content is not included in the response, use get-document endpoint for full content
6. Response includes both submitted and received documents by default
7. Use role parameter to filter documents by issuer/recipient role
8. Status values: PENDING, PROCESSING, COMPLETED, FAILED, CANCELLED, REJECTED
9. Document types: 01 (Invoice), 02 (Credit Note), 03 (Debit Note), etc.
10. Metadata and tags can be used for custom filtering and organization 