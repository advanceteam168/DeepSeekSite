# State Codes

This document lists the standard state codes used in the MyInvois System for Malaysian states and federal territories.

## States and Federal Territories

| Code | State/Territory | Capital | Region |
|------|----------------|---------|---------|
| JHR | Johor | Johor Bahru | Peninsular |
| KDH | Kedah | Alor Setar | Peninsular |
| KTN | Kelantan | Kota Bharu | Peninsular |
| MLK | Melaka | Melaka City | Peninsular |
| NSN | Negeri Sembilan | Seremban | Peninsular |
| PHG | Pahang | Kuantan | Peninsular |
| PNG | Pulau Pinang | George Town | Peninsular |
| PRK | Perak | Ipoh | Peninsular |
| PLS | Perlis | Kangar | Peninsular |
| SBH | Sabah | Kota Kinabalu | Borneo |
| SWK | Sarawak | Kuching | Borneo |
| SGR | Selangor | Shah Alam | Peninsular |
| TRG | Terengganu | Kuala Terengganu | Peninsular |
| KUL | W.P. Kuala Lumpur | Kuala Lumpur | Peninsular |
| LBN | W.P. Labuan | Victoria | Borneo |
| PJY | W.P. Putrajaya | Putrajaya | Peninsular |

## Regional Grouping

### Peninsular Malaysia (West Malaysia)

| Region | States |
|--------|---------|
| Northern | Perlis, Kedah, Pulau Pinang, Perak |
| Central | Selangor, W.P. Kuala Lumpur, W.P. Putrajaya |
| Southern | Negeri Sembilan, Melaka, Johor |
| East Coast | Kelantan, Terengganu, Pahang |

### East Malaysia (Borneo)

| Region | States |
|--------|---------|
| Borneo | Sabah, Sarawak, W.P. Labuan |

## Usage in API

### Address Example

```json
{
  "address": {
    "line1": "123 Jalan Business",
    "line2": "Taman Enterprise",
    "city": "Shah Alam",
    "state": "SGR",
    "postcode": "40150",
    "country": "MY"
  }
}
```

### Business Registration Example

```json
{
  "business": {
    "name": "ABC Trading Sdn Bhd",
    "registrationNumber": "202401001234",
    "primaryAddress": {
      "state": "KUL",
      "region": "CENTRAL"
    },
    "branches": [
      {
        "state": "PNG",
        "region": "NORTHERN"
      },
      {
        "state": "JHR",
        "region": "SOUTHERN"
      }
    ]
  }
}
```

## Postal Code Ranges

| State | Range | Format |
|-------|-------|--------|
| JHR | 79000-86000 | 5 digits |
| KDH | 05000-09810 | 5 digits |
| KTN | 15000-18500 | 5 digits |
| MLK | 75000-78000 | 5 digits |
| NSN | 70000-73500 | 5 digits |
| PHG | 25000-28800 | 5 digits |
| PNG | 10000-14400 | 5 digits |
| PRK | 30000-36810 | 5 digits |
| PLS | 01000-02800 | 5 digits |
| SBH | 88000-91000 | 5 digits |
| SWK | 93000-98850 | 5 digits |
| SGR | 40000-48300 | 5 digits |
| TRG | 20000-24300 | 5 digits |
| KUL | 50000-60000 | 5 digits |
| LBN | 87000-87033 | 5 digits |
| PJY | 62000-62988 | 5 digits |

## Validation Rules

1. State Code:
   - Must be 3 characters
   - Uppercase letters only
   - Valid code from list

2. Address Rules:
   - State code required
   - Must match postcode range
   - City must be in state
   - Valid combinations only

3. Business Rules:
   - Primary state required
   - Multiple states allowed
   - Regional grouping valid

## Notes

1. State codes are 3 letters
2. Federal territories prefixed with W.P.
3. Used in:
   - Postal addresses
   - Tax documents
   - Business registration
   - Government forms
4. Regional grouping for:
   - Tax jurisdictions
   - Business reporting
   - Statistical analysis
5. Special handling for:
   - Federal territories
   - Special economic zones
   - Free trade zones
6. Integration with:
   - Postal systems
   - Government agencies
   - Tax authorities
7. Address validation:
   - Format by state
   - City-state match
   - Postcode ranges
8. Business rules:
   - Branch registration
   - Tax registration
   - Licensing
9. Reporting requirements:
   - State-level statistics
   - Regional analysis
   - Tax jurisdiction
10. System features:
    - Address validation
    - Postcode lookup
    - City suggestions 