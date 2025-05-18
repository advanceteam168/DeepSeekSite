# Classification Codes

This document lists the standard classification codes used in the MyInvois System for categorizing businesses and transactions.

## Business Classification Codes

### Industry Sectors

| Code | Description | Category |
|------|-------------|-----------|
| A | Agriculture, Forestry and Fishing | Primary |
| B | Mining and Quarrying | Primary |
| C | Manufacturing | Secondary |
| D | Electricity, Gas, Steam and Air Conditioning Supply | Secondary |
| E | Water Supply, Sewerage and Waste Management | Secondary |
| F | Construction | Secondary |
| G | Wholesale and Retail Trade | Tertiary |
| H | Transportation and Storage | Tertiary |
| I | Accommodation and Food Service Activities | Tertiary |
| J | Information and Communication | Tertiary |
| K | Financial and Insurance Activities | Tertiary |
| L | Real Estate Activities | Tertiary |
| M | Professional, Scientific and Technical Activities | Tertiary |
| N | Administrative and Support Service Activities | Tertiary |
| O | Public Administration and Defence | Tertiary |
| P | Education | Tertiary |
| Q | Human Health and Social Work Activities | Tertiary |
| R | Arts, Entertainment and Recreation | Tertiary |
| S | Other Service Activities | Tertiary |
| T | Activities of Households as Employers | Tertiary |
| U | Activities of Extraterritorial Organizations | Tertiary |

### Business Size

| Code | Description | Annual Revenue (MYR) | Employees |
|------|-------------|---------------------|-----------|
| MI | Micro Enterprise | < 300,000 | < 5 |
| SE | Small Enterprise | 300,000 - 15M | 5 - 75 |
| ME | Medium Enterprise | 15M - 50M | 76 - 200 |
| LE | Large Enterprise | > 50M | > 200 |

### Business Type

| Code | Description | Registration Prefix |
|------|-------------|-------------------|
| ROB | Sole Proprietorship | SA |
| ROC | Company | MY |
| LLP | Limited Liability Partnership | LLP |
| BIZ | Business | BIZ |
| SOC | Society | PPM |
| OTH | Others | - |

## Transaction Classification Codes

### Transaction Types

| Code | Description | Category |
|------|-------------|-----------|
| SALE | Sales of Goods | Goods |
| SERV | Services | Services |
| RENT | Rental | Services |
| ROYA | Royalties | Services |
| COMM | Commission | Services |
| INTR | Interest | Financial |
| DIVD | Dividend | Financial |
| OTHR | Others | Others |

### Payment Terms

| Code | Description | Days |
|------|-------------|------|
| IMM | Immediate | 0 |
| COD | Cash on Delivery | 0 |
| NET7 | Net 7 Days | 7 |
| NET14 | Net 14 Days | 14 |
| NET30 | Net 30 Days | 30 |
| NET45 | Net 45 Days | 45 |
| NET60 | Net 60 Days | 60 |
| NET90 | Net 90 Days | 90 |

### Document Purpose

| Code | Description | Usage |
|------|-------------|-------|
| ORIG | Original | First issuance |
| COPY | Copy | Duplicate of original |
| REPL | Replacement | Replaces original |
| CORR | Correction | Corrects errors |
| CANC | Cancellation | Cancels original |

## Usage in API

### Request Example

```json
{
  "business": {
    "industryCode": "C",
    "sizeCode": "SE",
    "typeCode": "ROC"
  },
  "transaction": {
    "typeCode": "SALE",
    "paymentTerms": "NET30",
    "purpose": "ORIG"
  }
}
```

### Response Example

```json
{
  "business": {
    "industryCode": "C",
    "industryName": "Manufacturing",
    "industryCategory": "Secondary",
    "sizeCode": "SE",
    "sizeName": "Small Enterprise",
    "typeCode": "ROC",
    "typeName": "Company"
  },
  "transaction": {
    "typeCode": "SALE",
    "typeName": "Sales of Goods",
    "typeCategory": "Goods",
    "paymentTerms": "NET30",
    "paymentDays": 30,
    "purpose": "ORIG",
    "purposeName": "Original"
  }
}
```

## Notes

1. Industry sectors follow the Malaysia Standard Industrial Classification (MSIC) 2008
2. Business size criteria based on SME Corp Malaysia guidelines
3. Business type codes aligned with SSM registration types
4. Transaction types support GST/SST reporting requirements
5. Payment terms are calendar days from invoice date
6. Document purpose codes used in e-Invoice header
7. All codes are case-sensitive
8. Industry codes are single alphabets (A-U)
9. Size codes are two characters
10. Transaction codes are four characters

## List

| Code | Description |
|------|-------------|
| 001 | Breastfeeding equipment |
| 002 | Child care centres and kindergartens fees |
| 003 | Computer, smartphone or tablet |
| 004 | Consolidated e-Invoice |
| 005 | Construction materials (as specified under Fourth Schedule of the Lembaga Pembangunan Industri Pembinaan Malaysia Act 1994) |
| 006 | Disbursement |
| 007 | Donation |
| 008 | e-Commerce - e-Invoice to buyer / purchaser |
| 009 | e-Commerce - Self-billed e-Invoice to seller, logistics, etc. |
| 010 | Education fees |
| 011 | Goods on consignment (Consignor) |
| 012 | Goods on consignment (Consignee) |
| 013 | Gym membership |
| 014 | Insurance - Education and medical benefits |
| 015 | Insurance - Takaful or life insurance |
| 016 | Interest and financing expenses |
| 017 | Internet subscription |
| 018 | Land and building |
| 019 | Medical examination for learning disabilities and early intervention or rehabilitation treatments of learning disabilities |
| 020 | Medical examination or vaccination expenses |
| 021 | Medical expenses for serious diseases |
| 022 | Others |
| 023 | Petroleum operations (as defined in Petroleum (Income Tax) Act 1967) |
| 024 | Private retirement scheme or deferred annuity scheme |
| 025 | Motor vehicle |
| 026 | Subscription of books / journals / magazines / newspapers / other similar publications |
| 027 | Reimbursement |
| 028 | Rental of motor vehicle |
| 029 | EV charging facilities (Installation, rental, sale / purchase or subscription fees) |
| 030 | Repair and maintenance |
| 031 | Research and development |
| 032 | Foreign income |
| 033 | Self-billed - Betting and gaming |
| 034 | Self-billed - Importation of goods |
| 035 | Self-billed - Importation of services |
| 036 | Self-billed - Others |
| 037 | Self-billed - Monetary payment to agents, dealers or distributors |
| 038 | Sports equipment, rental / entry fees for sports facilities, registration in sports competition or sports training fees imposed by associations / sports clubs / companies registered with the Sports Commissioner or Companies Commission of Malaysia and carrying out sports activities as listed under the Sports Development Act 1997 |
| 039 | Supporting equipment for disabled person |
| 040 | Voluntary contribution to approved provident fund |
| 041 | Dental examination or treatment |
| 042 | Fertility treatment |
| 043 | Treatment and home care nursing, daycare centres and residential care centers |
| 044 | Vouchers, gift cards, loyalty points, etc |
| 045 | Self-billed - Non-monetary payment to agents, dealers or distributors |

## File Download

[Download JSON file](/files/ClassificationCodes.json) containing codes of all classifications.

## Additional Considerations

Note that when submitting the documents, taxpayers should be using the code values. 