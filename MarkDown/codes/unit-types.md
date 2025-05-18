# Unit Types

This document lists the standard unit types used in the MyInvois System.

## Quantity Units

### Count Units

| Code | Description | Symbol | Category |
|------|-------------|--------|-----------|
| PCS | Pieces | pcs | Count |
| UNT | Units | unit | Count |
| DOZ | Dozen | doz | Count |
| BOX | Box | box | Count |
| CTN | Carton | ctn | Count |
| SET | Set | set | Count |
| PAK | Pack | pak | Count |
| LOT | Lot | lot | Count |

### Length Units

| Code | Description | Symbol | Category |
|------|-------------|--------|-----------|
| MM | Millimeter | mm | Length |
| CM | Centimeter | cm | Length |
| M | Meter | m | Length |
| KM | Kilometer | km | Length |
| IN | Inch | in | Length |
| FT | Foot | ft | Length |
| YD | Yard | yd | Length |

### Weight Units

| Code | Description | Symbol | Category |
|------|-------------|--------|-----------|
| MG | Milligram | mg | Weight |
| G | Gram | g | Weight |
| KG | Kilogram | kg | Weight |
| MT | Metric Ton | mt | Weight |
| OZ | Ounce | oz | Weight |
| LB | Pound | lb | Weight |

### Volume Units

| Code | Description | Symbol | Category |
|------|-------------|--------|-----------|
| ML | Milliliter | ml | Volume |
| L | Liter | l | Volume |
| M3 | Cubic Meter | m³ | Volume |
| GAL | Gallon | gal | Volume |
| BBL | Barrel | bbl | Volume |

### Area Units

| Code | Description | Symbol | Category |
|------|-------------|--------|-----------|
| MM2 | Square Millimeter | mm² | Area |
| CM2 | Square Centimeter | cm² | Area |
| M2 | Square Meter | m² | Area |
| KM2 | Square Kilometer | km² | Area |
| HA | Hectare | ha | Area |
| AC | Acre | ac | Area |

### Time Units

| Code | Description | Symbol | Category |
|------|-------------|--------|-----------|
| SEC | Second | sec | Time |
| MIN | Minute | min | Time |
| HR | Hour | hr | Time |
| DAY | Day | day | Time |
| WK | Week | wk | Time |
| MON | Month | mon | Time |
| YR | Year | yr | Time |

## Usage in API

### Line Item Example

```json
{
  "item": {
    "description": "Office Paper A4",
    "quantity": {
      "value": 10,
      "unit": "BOX"
    },
    "subQuantity": {
      "value": 500,
      "unit": "PCS",
      "conversion": 50
    },
    "unitPrice": {
      "value": 100.00,
      "unit": "BOX"
    }
  }
}
```

### Product Definition Example

```json
{
  "product": {
    "code": "PAPER-A4",
    "description": "A4 Paper 80gsm",
    "unitOfMeasure": {
      "primary": {
        "code": "BOX",
        "description": "Box of 500 sheets"
      },
      "secondary": {
        "code": "PCS",
        "description": "Individual sheets",
        "conversion": 500
      }
    },
    "packaging": {
      "dimensions": {
        "length": {
          "value": 30,
          "unit": "CM"
        },
        "width": {
          "value": 21,
          "unit": "CM"
        },
        "height": {
          "value": 25,
          "unit": "CM"
        }
      },
      "weight": {
        "value": 2.5,
        "unit": "KG"
      }
    }
  }
}
```

## Unit Conversions

### Common Conversions

| From | To | Factor |
|------|-----|--------|
| MM | CM | 0.1 |
| CM | M | 0.01 |
| M | KM | 0.001 |
| MG | G | 0.001 |
| G | KG | 0.001 |
| KG | MT | 0.001 |
| ML | L | 0.001 |
| MM2 | CM2 | 0.01 |
| CM2 | M2 | 0.0001 |
| M2 | HA | 0.0001 |

### Time Conversions

| From | To | Factor |
|------|-----|--------|
| SEC | MIN | 1/60 |
| MIN | HR | 1/60 |
| HR | DAY | 1/24 |
| DAY | WK | 1/7 |
| DAY | MON | 1/30 |
| MON | YR | 1/12 |

## Validation Rules

1. Unit Code:
   - Must be valid code
   - Case sensitive
   - Category appropriate

2. Quantity Rules:
   - Non-negative values
   - Decimal places by unit
   - Conversion accuracy

3. Combinations:
   - Compatible units
   - Valid conversions
   - Business rules

## Notes

1. Unit Selection:
   - Industry standard
   - Common usage
   - Local practice

2. Precision:
   - Length: 3 decimals
   - Weight: 3 decimals
   - Volume: 3 decimals
   - Count: 0 decimals

3. Conversions:
   - Base units
   - Standard factors
   - Rounding rules

4. Display:
   - Local format
   - Symbol position
   - Decimal separator

5. Validation:
   - Range checks
   - Unit compatibility
   - Business logic

6. Special Cases:
   - Bulk quantities
   - Mixed units
   - Custom units

7. Documentation:
   - Unit description
   - Usage context
   - Examples

8. System Features:
   - Auto-conversion
   - Unit suggestion
   - Validation

9. Integration:
   - ERP systems
   - Inventory
   - Shipping

10. Updates:
    - New units
    - Conversion factors
    - Industry changes 