# Tax Types

This document lists the standard tax types used in the MyInvois System.

## Sales and Service Tax (SST)

### Sales Tax

| Code | Description | Rate | Category |
|------|-------------|------|-----------|
| ST00 | Sales Tax Exempt | 0% | Exempt |
| ST05 | Sales Tax 5% | 5% | Standard |
| ST10 | Sales Tax 10% | 10% | Standard |
| STSP | Sales Tax Special Rate | Variable | Special |
| STEX | Sales Tax Export | 0% | Export |

### Service Tax

| Code | Description | Rate | Category |
|------|-------------|------|-----------|
| SV00 | Service Tax Exempt | 0% | Exempt |
| SV06 | Service Tax 6% | 6% | Standard |
| SVSP | Service Tax Special Rate | Variable | Special |
| SVEX | Service Tax Export | 0% | Export |

## Tourism Tax

| Code | Description | Rate | Category |
|------|-------------|------|-----------|
| TT00 | Tourism Tax Exempt | 0% | Exempt |
| TT10 | Tourism Tax RM10 | RM10/room/night | Standard |
| TTSP | Tourism Tax Special Rate | Variable | Special |

## Withholding Tax

| Code | Description | Rate | Category |
|------|-------------|------|-----------|
| WT00 | Withholding Tax Exempt | 0% | Exempt |
| WT10 | Withholding Tax 10% | 10% | Standard |
| WT15 | Withholding Tax 15% | 15% | Standard |
| WTSP | Withholding Tax Special Rate | Variable | Special |

## Usage in API

### Invoice Line Item Example

```json
{
  "item": {
    "description": "Professional Services",
    "quantity": 1,
    "unitPrice": 1000.00,
    "taxes": [
      {
        "type": "SV06",
        "rate": 6,
        "amount": 60.00,
        "category": "Standard"
      }
    ],
    "totalBeforeTax": 1000.00,
    "totalTaxAmount": 60.00,
    "totalAmount": 1060.00
  }
}
```

### Multiple Tax Example

```json
{
  "item": {
    "description": "Hotel Room",
    "quantity": 2,
    "unitPrice": 500.00,
    "taxes": [
      {
        "type": "SV06",
        "rate": 6,
        "amount": 60.00,
        "category": "Standard"
      },
      {
        "type": "TT10",
        "rate": "FLAT",
        "amount": 20.00,
        "category": "Standard"
      }
    ],
    "totalBeforeTax": 1000.00,
    "totalTaxAmount": 80.00,
    "totalAmount": 1080.00
  }
}
```

## Tax Categories

### Standard Rate

- Fixed percentage rates
- Statutory rates
- Generally applicable

### Zero Rate

- 0% tax rate
- Full input tax credit
- Export-related

### Exempt

- No tax charged
- No input tax credit
- Special categories

### Special Rate

- Variable rates
- Industry-specific
- Requires approval

## Validation Rules

1. Tax Code:
   - Must be valid code
   - Case sensitive
   - Matches category

2. Rate Rules:
   - Within valid range
   - Matches code type
   - Special approvals

3. Amount Calculation:
   - Accurate computation
   - Rounding rules
   - Multiple tax handling

## Tax Computation

### Basic Formula

```
Tax Amount = Taxable Amount × Tax Rate
```

### Multiple Taxes

```
Total Tax = Sum of Individual Taxes
Total Amount = Amount Before Tax + Total Tax
```

### Special Cases

1. Flat Rate Taxes:
   - Tourism Tax
   - Fixed amounts
   - Per unit basis

2. Compound Taxes:
   - Tax on tax
   - Sequential calculation
   - Order matters

## Notes

1. Tax Types:
   - Based on Malaysian law
   - Regular updates
   - Industry-specific

2. Documentation:
   - Tax code required
   - Rate justification
   - Exemption proof

3. Calculations:
   - 2 decimal places
   - Round up/down rules
   - Currency specific

4. Compliance:
   - RMCD regulations
   - Industry guidelines
   - Audit requirements

5. Special Handling:
   - Mixed supplies
   - Composite supplies
   - Export supplies

6. System Features:
   - Auto-calculation
   - Validation checks
   - Audit trails

7. Business Rules:
   - Registration status
   - Industry codes
   - Special schemes

8. Reporting:
   - Tax returns
   - Audit files
   - Statistics

9. Integration:
   - Tax authorities
   - Accounting systems
   - Payment systems

10. Updates:
    - Rate changes
    - New tax types
    - Policy changes 