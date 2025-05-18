# Validate Taxpayer TIN

This API endpoint validates a Taxpayer Identification Number (TIN) to ensure it exists and is active in the MyInvois System.

## Endpoint

```
POST /api/v1/taxpayers/validate
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |
| Content-Type | application/json |

## Request Body

```json
{
  "tin": "string"
}
```

### Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| tin | string | Taxpayer Identification Number to validate | Yes |

## Response

### Success Response (200 OK)

```json
{
  "valid": true,
  "taxpayer": {
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
    }
  }
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| valid | boolean | Whether the TIN is valid |
| taxpayer | object | Taxpayer information (only returned if valid is true) |
| taxpayer.tin | string | Taxpayer Identification Number |
| taxpayer.name | string | Registered business name |
| taxpayer.status | string | Current status (ACTIVE, SUSPENDED, etc.) |
| taxpayer.registrationDate | string | MyInvois registration date (ISO 8601) |
| taxpayer.businessType | string | Type of business |
| taxpayer.address | object | Registered business address |

### Error Responses

#### 400 Bad Request

```json
{
  "error": "invalid_request",
  "message": "Invalid TIN format"
}
```

#### 401 Unauthorized

```json
{
  "error": "unauthorized",
  "message": "Invalid or expired token"
}
```

#### 404 Not Found

```json
{
  "error": "not_found",
  "message": "TIN not found"
}
```

## Example

### Request

```bash
curl -X POST https://api.myinvois.hasil.gov.my/api/v1/taxpayers/validate \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json" \
  -d '{
    "tin": "123456789"
  }'
```

### Response

```json
{
  "valid": true,
  "taxpayer": {
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
    }
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. The TIN format must follow the standard Malaysian tax identification format
3. Only active TINs registered with MyInvois can be validated
4. The response includes basic business information if the TIN is valid
5. Use this endpoint before submitting documents to ensure the counterparty is registered 