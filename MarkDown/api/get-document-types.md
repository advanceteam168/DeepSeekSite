# Get Document Types

This API endpoint retrieves a list of all supported document types in the MyInvois System.

## Endpoint

```
GET /api/v1/document-types
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |

## Response

### Success Response (200 OK)

```json
{
  "documentTypes": [
    {
      "code": "string",
      "name": "string",
      "description": "string",
      "versions": [
        {
          "version": "string",
          "status": "string",
          "effectiveDate": "string",
          "endDate": "string"
        }
      ]
    }
  ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| documentTypes | array | List of document types |
| code | string | Document type code |
| name | string | Document type name |
| description | string | Document type description |
| versions | array | List of versions for this document type |
| version | string | Version number |
| status | string | Version status (ACTIVE, DEPRECATED, RETIRED) |
| effectiveDate | string | Date when version became effective (ISO 8601) |
| endDate | string | Date when version will be retired (ISO 8601) |

### Error Response (401 Unauthorized)

```json
{
  "error": "unauthorized",
  "message": "Invalid or expired token"
}
```

## Example

### Request

```bash
curl -X GET https://api.myinvois.hasil.gov.my/api/v1/document-types \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
{
  "documentTypes": [
    {
      "code": "01",
      "name": "Invoice",
      "description": "Standard invoice document",
      "versions": [
        {
          "version": "1.0",
          "status": "DEPRECATED",
          "effectiveDate": "2023-01-01T00:00:00Z",
          "endDate": "2024-12-31T23:59:59Z"
        },
        {
          "version": "1.1",
          "status": "ACTIVE",
          "effectiveDate": "2024-01-01T00:00:00Z",
          "endDate": null
        }
      ]
    },
    {
      "code": "02",
      "name": "Credit Note",
      "description": "Credit note document",
      "versions": [
        {
          "version": "1.0",
          "status": "DEPRECATED",
          "effectiveDate": "2023-01-01T00:00:00Z",
          "endDate": "2024-12-31T23:59:59Z"
        },
        {
          "version": "1.1",
          "status": "ACTIVE",
          "effectiveDate": "2024-01-01T00:00:00Z",
          "endDate": null
        }
      ]
    }
  ]
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. The list includes all document types regardless of their version status
3. Document types may have multiple versions with different statuses
4. The endDate field will be null for active versions that have no planned retirement date 