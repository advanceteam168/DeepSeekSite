# Get Document Type Version

This API endpoint retrieves detailed information about a specific version of a document type in the MyInvois System.

## Endpoint

```
GET /api/v1/document-types/{code}/versions/{version}
```

## Path Parameters

| Parameter | Type | Description | Required |
|-----------|------|-------------|-----------|
| code | string | Document type code | Yes |
| version | string | Version number | Yes |

## Request Headers

| Header | Value |
|--------|-------|
| Authorization | Bearer {access_token} |

## Response

### Success Response (200 OK)

```json
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
      "severity": "string",
      "details": {
        "field": "string",
        "condition": "string",
        "message": "string"
      }
    }
  ],
  "examples": [
    {
      "format": "string",
      "description": "string",
      "url": "string",
      "features": [
        "string"
      ]
    }
  ],
  "changelog": {
    "changes": [
      {
        "type": "string",
        "description": "string",
        "field": "string"
      }
    ],
    "previousVersion": "string"
  }
}
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| version | string | Version number |
| status | string | Version status (ACTIVE, DEPRECATED, RETIRED) |
| effectiveDate | string | Date when version became effective (ISO 8601) |
| endDate | string | Date when version will be retired (ISO 8601) |
| schema | object | Schema definitions for this version |
| schema.json | string | URL to JSON schema definition |
| schema.xml | string | URL to XML schema definition |
| validationRules | array | List of validation rules |
| validationRules[].code | string | Rule code |
| validationRules[].description | string | Rule description |
| validationRules[].severity | string | Rule severity (ERROR, WARNING) |
| validationRules[].details | object | Detailed rule information |
| validationRules[].details.field | string | Field path the rule applies to |
| validationRules[].details.condition | string | Validation condition |
| validationRules[].details.message | string | Error message template |
| examples | array | List of example documents |
| examples[].format | string | Format type (JSON, XML) |
| examples[].description | string | Example description |
| examples[].url | string | URL to example file |
| examples[].features | array | List of features demonstrated |
| changelog | object | Version change information |
| changelog.changes | array | List of changes from previous version |
| changelog.changes[].type | string | Change type (ADDED, MODIFIED, REMOVED) |
| changelog.changes[].description | string | Change description |
| changelog.changes[].field | string | Affected field path |
| changelog.previousVersion | string | Previous version number |

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
  "message": "Document type version not found"
}
```

## Example

### Request

```bash
curl -X GET https://api.myinvois.hasil.gov.my/api/v1/document-types/01/versions/1.1 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

### Response

```json
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
      "severity": "ERROR",
      "details": {
        "field": "invoiceNumber",
        "condition": "unique across all submitted invoices",
        "message": "Invoice number {value} has already been used"
      }
    },
    {
      "code": "INV002",
      "description": "Invoice date must not be in the future",
      "severity": "ERROR",
      "details": {
        "field": "invoiceDate",
        "condition": "date <= current date",
        "message": "Invoice date {value} cannot be in the future"
      }
    }
  ],
  "examples": [
    {
      "format": "JSON",
      "description": "Basic invoice example",
      "url": "https://sdk.myinvois.hasil.gov.my/examples/invoice-v1.1-basic.json",
      "features": [
        "basic fields",
        "single line item"
      ]
    },
    {
      "format": "XML",
      "description": "Complex invoice example",
      "url": "https://sdk.myinvois.hasil.gov.my/examples/invoice-v1.1-complex.xml",
      "features": [
        "multiple line items",
        "discounts",
        "taxes",
        "foreign currency"
      ]
    }
  ],
  "changelog": {
    "changes": [
      {
        "type": "ADDED",
        "description": "Added digital signature requirement",
        "field": "signature"
      },
      {
        "type": "MODIFIED",
        "description": "Extended invoice number length",
        "field": "invoiceNumber"
      }
    ],
    "previousVersion": "1.0"
  }
}
```

## Notes

1. This endpoint requires a valid access token obtained through the login process
2. The response includes complete technical details about the specific version
3. Schema URLs point to the formal schema definitions that can be used for validation
4. Example URLs point to downloadable sample documents
5. Validation rules include detailed information about how each rule is applied
6. The changelog helps track differences from the previous version 