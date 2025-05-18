# Get Document Type

This API endpoint retrieves detailed information about a specific document type in the MyInvois System.

## Endpoint

```
GET /api/v1/document-types/{code}
```

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| code | string | Document type code | Yes |

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |

## Response

### Success Response (200 OK)

```json
{
  "code": "string",
  "name": "string",
  "description": "string",
  "versions": [
    {
      "version": "string",
      "status": "string",
      "effectiveDate": "string",
      "endDate": "string",
      "schema": {
        "json": "string",
        "xml": "string"
      },
      "validationRules": [
        {
          "code": "string",
          "description": "string",
          "severity": "string"
        }
      ]
    }
  ],
  "examples": [
    {
      "version": "string",
      "format": "string",
      "description": "string",
      "url": "string"
    }
  ]
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| code | string | Document type code |
| name | string | Document type name |
| description | string | Document type description |
| versions | array | List of versions for this document type |
| version | string | Version number |
| status | string | Version status (ACTIVE, DEPRECATED, RETIRED) |
| effectiveDate | string | Date when version became effective (ISO 8601) |
| endDate | string | Date when version will be retired (ISO 8601) |
| schema | object | Schema definitions for the document type |
| schema.json | string | URL to JSON schema definition |
| schema.xml | string | URL to XML schema definition |
| validationRules | array | List of validation rules |
| validationRules[].code | string | Rule code |
| validationRules[].description | string | Rule description |
| validationRules[].severity | string | Rule severity (ERROR, WARNING) |
| examples | array | List of example documents |
| examples[].version | string | Version number |
| examples[].format | string | Format type (JSON, XML) |
| examples[].description | string | Example description |
| examples[].url | string | URL to example file |

### Error Responses

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
  "message": "Document type not found"
}
```

## Example

### Request

```bash
curl -X GET https://api.myinvois.hasil.gov.my/api/v1/document-types/01 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
{
  "code": "01",
  "name": "Invoice",
  "description": "Standard invoice document",
  "versions": [
    {
      "version": "1.1",
      "status": "ACTIVE",
      "effectiveDate": "2024-01-01T00:00:00Z",
      "endDate": null,
      "schema": {
        "json": "https://sdk.myinvois.hasil.gov.my/schemas/invoice-v1.1.json",
        "xml": "https://sdk.myinvois.hasil.gov.my/schemas/invoice-v1.1.xsd"
      },
      "validationRules": [
        {
          "code": "INV001",
          "description": "Invoice number must be unique",
          "severity": "ERROR"
        },
        {
          "code": "INV002",
          "description": "Invoice date must not be in the future",
          "severity": "ERROR"
        }
      ]
    }
  ],
  "examples": [
    {
      "version": "1.1",
      "format": "JSON",
      "description": "Basic invoice example",
      "url": "https://sdk.myinvois.hasil.gov.my/examples/invoice-v1.1-basic.json"
    },
    {
      "version": "1.1",
      "format": "XML",
      "description": "Basic invoice example",
      "url": "https://sdk.myinvois.hasil.gov.my/examples/invoice-v1.1-basic.xml"
    }
  ]
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. The response includes detailed information about the document type, including all versions
3. Schema URLs point to the formal schema definitions that can be used for validation
4. Example URLs point to downloadable sample documents
5. Validation rules list all the business rules that apply to this document type 