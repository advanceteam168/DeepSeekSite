# Malaysia Standard Industrial Classification (MSIC) Codes

This document lists the standard MSIC codes used in the MyInvois System. The codes follow MSIC 2008 Ver. 1.0 based on International Standard Industrial Classification of All Economic Activities (ISIC) Rev. 4.

## Main Sections

| Section | Description | Division Codes |
|---------|-------------|----------------|
| A | Agriculture, Forestry and Fishing | 01-03 |
| B | Mining and Quarrying | 05-09 |
| C | Manufacturing | 10-33 |
| D | Electricity, Gas, Steam and Air Conditioning Supply | 35 |
| E | Water Supply; Sewerage, Waste Management and Remediation Activities | 36-39 |
| F | Construction | 41-43 |
| G | Wholesale and Retail Trade; Repair of Motor Vehicles and Motorcycles | 45-47 |
| H | Transportation and Storage | 49-53 |
| I | Accommodation and Food Service Activities | 55-56 |
| J | Information and Communication | 58-63 |
| K | Financial and Insurance/Takaful Activities | 64-66 |
| L | Real Estate Activities | 68 |
| M | Professional, Scientific and Technical Activities | 69-75 |
| N | Administrative and Support Service Activities | 77-82 |
| O | Public Administration and Defence; Compulsory Social Security | 84 |
| P | Education | 85 |
| Q | Human Health and Social Work Activities | 86-88 |
| R | Arts, Entertainment and Recreation | 90-93 |
| S | Other Service Activities | 94-96 |
| T | Activities of Households as Employers | 97-98 |
| U | Activities of Extraterritorial Organizations and Bodies | 99 |

## Selected Divisions (Examples)

### Section C: Manufacturing (10-33)

| Code | Description | Examples |
|------|-------------|----------|
| 10 | Manufacture of food products | Meat, fish, fruits, oils |
| 11 | Manufacture of beverages | Soft drinks, mineral water |
| 12 | Manufacture of tobacco products | Cigarettes, tobacco |
| 13 | Manufacture of textiles | Spinning, weaving, finishing |
| 14 | Manufacture of wearing apparel | Clothing, accessories |
| 15 | Manufacture of leather and related products | Footwear, bags |

### Section G: Wholesale and Retail Trade (45-47)

| Code | Description | Examples |
|------|-------------|----------|
| 45 | Wholesale and retail trade of motor vehicles | Cars, parts, repairs |
| 46 | Wholesale trade, except motor vehicles | B2B trading |
| 47 | Retail trade, except motor vehicles | Shops, stores |

### Section J: Information and Communication (58-63)

| Code | Description | Examples |
|------|-------------|----------|
| 58 | Publishing activities | Books, software |
| 59 | Motion picture and sound recording | Films, music |
| 60 | Broadcasting activities | TV, radio |
| 61 | Telecommunications | Telco services |
| 62 | Computer programming and consultancy | IT services |
| 63 | Information service activities | Data processing |

## Usage in API

### Business Registration Example

```json
{
  "business": {
    "msicCode": "62012",
    "msicDescription": "Software development",
    "section": "J",
    "division": "62",
    "group": "620",
    "class": "6201",
    "subclass": "62012"
  }
}
```

### Industry Classification Example

```json
{
  "industry": {
    "primary": {
      "msicCode": "62012",
      "percentage": 70
    },
    "secondary": [
      {
        "msicCode": "62020",
        "percentage": 20
      },
      {
        "msicCode": "62090",
        "percentage": 10
      }
    ]
  }
}
```

## Code Structure

### Hierarchical Levels

1. Section (1 alphabet)
2. Division (2 digits)
3. Group (3 digits)
4. Class (4 digits)
5. Subclass (5 digits)

Example: 62012
- Section: J (Information and Communication)
- Division: 62 (Computer Programming)
- Group: 620 (Computer Programming Activities)
- Class: 6201 (Computer Programming)
- Subclass: 62012 (Software Development)

## Validation Rules

1. Code Format:
   - Must be numeric
   - Length: 2-5 digits
   - Valid in MSIC 2008

2. Business Rules:
   - Primary code required
   - Multiple codes allowed
   - Percentage must total 100%
   - Compatible activities only

3. Restrictions:
   - Licensed activities
   - Regulated industries
   - Special permissions

## Notes

1. Based on MSIC 2008 Ver. 1.0
2. Aligned with ISIC Rev. 4
3. Used for:
   - Business registration
   - Tax reporting
   - Industry statistics
   - Economic analysis
4. Updated periodically
5. Managed by DOSM
6. Required for:
   - SSM registration
   - Tax registration
   - License applications
   - Government tenders
7. Special codes for:
   - Islamic finance
   - Digital economy
   - Green technology
8. Industry-specific rules
9. Cross-referenced with:
   - ISIC Rev. 4
   - NACE Rev. 2
   - NAICS 2017
10. Used in government reporting

## MSIC Codes

### A. AGRICULTURE, FORESTRY AND FISHING

#### CROPS AND ANIMAL PRODUCTION, HUNTING AND RELATED SERVICE ACTIVITIES

##### Growing of non-perennial crops

| Code | Description |
|------|-------------|
| 00000 | NOT APPLICABLE |
| 01111 | Growing of maize |
| 01112 | Growing of leguminous crops |
| 01113 | Growing of oil seeds |
| 01119 | Growing of other cereals n.e.c. |
| 01120 | Growing of paddy |
| 01131 | Growing of leafy or stem vegetables |
| 01132 | Growing of fruits bearing vegetables |
| 01133 | Growing of melons |
| 01134 | Growing of mushrooms and truffles |
| 01135 | Growing of vegetables seeds, except beet seeds |
| 01136 | Growing of other vegetables |
| 01137 | Growing of sugar beet |
| 01138 | Growing of roots, tubers, bulb or tuberous vegetables |
| 01140 | Growing of sugar cane |
| 01150 | Growing of tobacco |
| 01160 | Growing of fibre crops |
| 01191 | Growing of flowers |
| 01192 | Growing of flower seeds |
| 01193 | Growing of sago (rumbia) |
| 01199 | Growing of other non-perennial crops n.e.c. |

[See full list in JSON format](/files/MSICSubCategoryCodes.json)

## Additional Considerations

Note that when submitting the documents, taxpayers should be using the code values. 