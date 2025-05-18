# Login as Intermediary System

This API endpoint allows an intermediary system to authenticate and obtain an access token for subsequent API calls. Intermediary systems are authorized third-party service providers that can submit documents on behalf of multiple taxpayers.

## Endpoint

```
POST /api/v1/auth/login-intermediary
```

## Request Headers

| Header | Value |
|--------|-------|
| Content-Type | application/json |

## Request Body

```json
{
  "clientId": "string",
  "clientSecret": "string"
}
```

### Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| clientId | string | Intermediary system client ID | Yes |
| clientSecret | string | Intermediary system client secret | Yes |

## Response

### Success Response (200 OK)

```json
{
  "accessToken": "string",
  "tokenType": "Bearer",
  "expiresIn": 3600,
  "scope": "string"
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| accessToken | string | JWT token to be used for subsequent API calls |
| tokenType | string | Type of token (always "Bearer") |
| expiresIn | number | Token validity period in seconds |
| scope | string | Authorized scope for the intermediary system |

### Error Response (401 Unauthorized)

```json
{
  "error": "invalid_credentials",
  "message": "Invalid client ID or client secret"
}
```

## Example

### Request

```bash
curl -X POST https://api.myinvois.hasil.gov.my/api/v1/auth/login-intermediary \
  -H "Content-Type: application/json" \
  -d '{
    "clientId": "intermediary_123",
    "clientSecret": "mysecret123"
  }'
```

### Response

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "tokenType": "Bearer",
  "expiresIn": 3600,
  "scope": "submit_documents validate_tin"
}
```

## Notes

1. The access token must be included in the Authorization header for all subsequent API calls
2. The token expires after the specified expiresIn period (typically 1 hour)
3. A new token should be obtained when the current token expires
4. The scope field indicates which API operations the intermediary system is authorized to perform 