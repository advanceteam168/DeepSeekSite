# Search Taxpayer TIN

This API endpoint allows searching for taxpayers in the MyInvois System using various criteria.

## Endpoint

```
GET /api/v1/taxpayers/search
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |
| Accept | application/json |

## Query Parameters

| Parameter | Type | Description | Required | Default |
|-----------|------|-------------|-----------|---------|
| tin | string | Search by TIN (partial match) | No | - |
| name | string | Search by business name | No | - |
| state | string | Filter by state code | No | - |
| businessType | string | Filter by business type | No | - |
| status | string | Filter by registration status | No | ACTIVE |
| page | integer | Page number | No | 1 |
| size | integer | Page size | No | 20 |
| sort | string | Sort field and direction | No | name:asc |

## Response

### Success Response (200 OK)

```json
{
  "content": [
    {
      "tin": "string",
      "name": "string",
      "status": "string",
      "registrationDate": "string",
      "businessType": "string",
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
      },
      "metadata": {
        "industry": "string",
        "size": "string",
        "registrationNumber": "string"
      }
    }
  ],
  "facets": {
    "states": [
      {
        "value": "string",
        "count": 0
      }
    ],
    "businessTypes": [
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
| content | array | List of taxpayer information |
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
      "field": "tin",
      "message": "TIN must be at least 3 characters"
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
curl "https://api.myinvois.hasil.gov.my/api/v1/taxpayers/search?name=Trading&state=WP&businessType=COMPANY&status=ACTIVE&page=1&size=10&sort=name:asc" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Accept: application/json"
```

### Response

```json
{
  "content": [
    {
      "tin": "123456789",
      "name": "ABC Trading Sdn Bhd",
      "status": "ACTIVE",
      "registrationDate": "2024-01-01T00:00:00Z",
      "businessType": "COMPANY",
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
      },
      "metadata": {
        "industry": "Wholesale Trade",
        "size": "SME",
        "registrationNumber": "201901000123"
      }
    }
  ],
  "facets": {
    "states": [
      {
        "value": "WP",
        "count": 1
      }
    ],
    "businessTypes": [
      {
        "value": "COMPANY",
        "count": 1
      }
    ],
    "statuses": [
      {
        "value": "ACTIVE",
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
        "field": "name",
        "direction": "asc"
      }
    ]
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. Search is case-insensitive and supports partial matches
3. Maximum page size is 100 records
4. Available sort fields:
   - tin
   - name
   - registrationDate
   - state
5. Sort directions: asc, desc
6. Business types:
   - COMPANY
   - ENTERPRISE
   - PARTNERSHIP
   - SOLE_PROPRIETOR
   - OTHERS
7. Status values:
   - ACTIVE
   - SUSPENDED
   - DEREGISTERED
   - PENDING
8. State codes follow Malaysian state standards
9. Results include basic business information
10. Use facets for quick filtering and analytics 