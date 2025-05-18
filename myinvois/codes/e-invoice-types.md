# e-Invoice Types

This document lists the standard e-invoice types supported by the MyInvois System.

## Standard Document Types

| Code | Document Type | Description | Version | Direction |
|------|--------------|-------------|---------|-----------|
| 01 | Invoice | Standard tax invoice | 1.1 | Outbound |
| 02 | Credit Note | Credit adjustment to invoice | 1.1 | Outbound |
| 03 | Debit Note | Debit adjustment to invoice | 1.1 | Outbound |
| 04 | Refund Note | Refund document | 1.1 | Outbound |
| 11 | Self-Billed Invoice | Invoice issued by buyer | 1.1 | Inbound |
| 12 | Self-Billed Credit Note | Credit note issued by buyer | 1.1 | Inbound |
| 13 | Self-Billed Debit Note | Debit note issued by buyer | 1.1 | Inbound |
| 14 | Self-Billed Refund Note | Refund note issued by buyer | 1.1 | Inbound |

## Document Categories

### Standard Invoices (01)

- Regular sales invoice
- Tax invoice
- Commercial invoice
- Simplified tax invoice
- Detailed tax invoice

### Credit Notes (02)

- Return of goods
- Price adjustment
- Discount
- Cancellation
- Correction

### Debit Notes (03)

- Additional charges
- Price adjustment
- Tax adjustment
- Correction
- Penalty

### Refund Notes (04)

- Payment refund
- Deposit refund
- Credit balance refund
- Overpayment refund
- Service cancellation

### Self-Billed Documents (11-14)

- Recipient-created tax invoice (RCTI)
- Buyer-created tax invoice (BCTI)
- Self-billed adjustments
- Margin scheme documents

## Document Properties

### Common Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| type | string | Yes | Document type code |
| version | string | Yes | Document version |
| number | string | Yes | Document number |
| issueDate | date | Yes | Issue date |
| dueDate | date | No | Payment due date |
| currency | string | Yes | Currency code |
| language | string | No | Document language |

### Party Information

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| issuer | object | Yes | Document issuer details |
| recipient | object | Yes | Document recipient details |
| buyer | object | No | Buyer details if different |
| seller | object | No | Seller details if different |
| taxRepresentative | object | No | Tax representative details |

### Amount Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| lineAmounts | array | Yes | Line item amounts |
| taxableAmount | number | Yes | Amount subject to tax |
| taxAmount | number | Yes | Total tax amount |
| totalAmount | number | Yes | Total document amount |
| discountAmount | number | No | Total discounts |
| chargeAmount | number | No | Additional charges |

## Usage in API

### Document Creation Example

```json
{
  "document": {
    "type": "01",
    "version": "1.1",
    "number": "INV-2024-001",
    "issueDate": "2024-03-14",
    "dueDate": "2024-04-13",
    "currency": "MYR",
    "language": "en",
    "issuer": {
      "tin": "123456789",
      "name": "ABC Trading Sdn Bhd"
    },
    "recipient": {
      "tin": "987654321",
      "name": "XYZ Industries Sdn Bhd"
    },
    "items": [
      {
        "number": 1,
        "description": "Office Supplies",
        "quantity": 10,
        "unitPrice": 100.00,
        "taxRate": 6,
        "taxAmount": 60.00,
        "totalAmount": 1060.00
      }
    ],
    "summary": {
      "totalBeforeTax": 1000.00,
      "totalTaxAmount": 60.00,
      "totalAmount": 1060.00
    }
  }
}
```

### Document Reference Example

```json
{
  "document": {
    "type": "02",
    "version": "1.1",
    "number": "CN-2024-001",
    "references": [
      {
        "type": "ORIG",
        "number": "INV-2024-001",
        "date": "2024-03-14"
      }
    ]
  }
}
```

## Notes

1. Document types are two-digit codes
2. Version follows semantic versioning
3. Self-billed documents require approval
4. Reference documents are mandatory for:
   - Credit notes
   - Debit notes
   - Refund notes
5. Document numbers must be unique
6. Issue date cannot be future date
7. Due date must be after issue date
8. Language defaults to English
9. Amounts must balance at line and header level
10. Tax calculations must be accurate

## Validation Rules

### Document Level

1. Type code must be valid
2. Version must be supported
3. Number format by document type
4. Date validations
5. Currency must be active

### Party Level

1. Valid TIN numbers
2. Required party details
3. Address validation
4. Contact information
5. Tax registration status

### Amount Level

1. Line amount calculations
2. Tax amount calculations
3. Total amount validations
4. Currency decimal places
5. Negative amount rules

## Business Rules

1. Credit limits
2. Payment terms
3. Tax regulations
4. Industry requirements
5. Compliance checks

## File Download

[Download JSON file](/files/EInvoiceTypes.json) containing codes of all the e-invoice types.

## Additional Considerations

Note that when submitting the documents, taxpayers should be using the code values. 