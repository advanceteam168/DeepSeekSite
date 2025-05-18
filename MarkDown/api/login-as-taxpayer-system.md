# Login as Taxpayer System

This API endpoint allows a taxpayer system to authenticate and obtain an access token for subsequent API calls.

## Endpoint

```
POST /api/v1/auth/login-taxpayer
```

## Request Headers

| Header | Value |
|--------|-------|
| Content-Type | application/json |

## Request Body

```json
{
  "tin": "string",
  "password": "string"
}
```

### Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| tin | string | Taxpayer Identification Number | Yes |
| password | string | Taxpayer system password | Yes |

## Response

### Success Response (200 OK)

```json
{
  "accessToken": "string",
  "tokenType": "Bearer",
  "expiresIn": 3600
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| accessToken | string | JWT token to be used for subsequent API calls |
| tokenType | string | Type of token (always "Bearer") |
| expiresIn | number | Token validity period in seconds |

### Error Response (401 Unauthorized)

```json
{
  "error": "invalid_credentials",
  "message": "Invalid TIN or password"
}
```

## Example

### Request

```bash
curl -X POST https://api.myinvois.hasil.gov.my/api/v1/auth/login-taxpayer \
  -H "Content-Type: application/json" \
  -d '{
    "tin": "123456789",
    "password": "mypassword123"
  }'
```

### Response

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "tokenType": "Bearer",
  "expiresIn": 3600
}
```

## Notes

1. The access token must be included in the Authorization header for all subsequent API calls
2. The token expires after the specified expiresIn period (typically 1 hour)
3. A new token should be obtained when the current token expires 