# QR Code Generation

This API endpoint generates a QR code for a document in the MyInvois System. The QR code can be used for quick verification of document authenticity.

## Endpoint

```
GET /api/v1/documents/{documentId}/qr-code
```

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |
| Accept | image/png, image/svg+xml |

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| documentId | string | Unique identifier of the document | Yes |

## Query Parameters

| Parameter | Type | Description | Required | Default |
|-----------|------|-------------|-----------|---------|
| format | string | QR code format (PNG, SVG) | No | PNG |
| size | integer | QR code size in pixels | No | 256 |
| margin | integer | QR code margin in pixels | No | 4 |
| errorCorrection | string | Error correction level (L, M, Q, H) | No | M |
| color | string | QR code color (hex) | No | #000000 |
| backgroundColor | string | Background color (hex) | No | #FFFFFF |
| logo | string | URL of logo to embed (base64) | No | - |
| logoSize | integer | Logo size as percentage of QR code | No | 20 |

## Response

### Success Response (200 OK)

Returns the QR code image in the requested format (PNG or SVG).

Headers:
```
Content-Type: image/png
Content-Length: <length>
Cache-Control: public, max-age=86400
```

### Error Responses

#### 400 Bad Request

```json
{
  "error": "invalid_request",
  "message": "Invalid QR code parameters",
  "details": [
    {
      "field": "size",
      "message": "Size must be between 128 and 1024 pixels"
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

#### 403 Forbidden

```json
{
  "error": "forbidden",
  "message": "Not authorized to access this document"
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
curl "https://api.myinvois.hasil.gov.my/api/v1/documents/doc-001/qr-code?format=PNG&size=512&errorCorrection=H" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Accept: image/png" \
  --output qr-code.png
```

### QR Code Content

The QR code contains a JSON payload with essential document information:

```json
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
  "hash": "sha256-H4sIAAAAAAAEAO1X227bOBD9FYF...",
  "verifyUrl": "https://myinvois.hasil.gov.my/verify/doc-001"
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. QR code format options:
   - PNG: Default, best for general use
   - SVG: Scalable format, best for printing
3. Error correction levels:
   - L (Low): 7% recovery
   - M (Medium): 15% recovery
   - Q (Quarter): 25% recovery
   - H (High): 30% recovery
4. Size constraints:
   - Minimum: 128x128 pixels
   - Maximum: 1024x1024 pixels
   - Default: 256x256 pixels
5. Logo requirements:
   - Must be square image
   - Supported formats: PNG, JPEG
   - Maximum file size: 50KB
6. QR code content includes:
   - Document identifier
   - Basic document information
   - Document hash for verification
   - Verification URL
7. QR codes are cached for 24 hours
8. High error correction recommended for printed documents
9. Use light background colors for better scanning
10. QR code verification works offline with MyInvois mobile app 