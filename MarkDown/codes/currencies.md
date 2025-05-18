# Currency Codes

This document lists the standard currency codes used in the MyInvois System. The codes follow the ISO 4217 standard.

## Primary Currencies

| Code | Currency | Country/Region | Decimal Places | Symbol |
|------|----------|----------------|----------------|---------|
| MYR | Malaysian Ringgit | Malaysia | 2 | RM |
| USD | US Dollar | United States | 2 | $ |
| EUR | Euro | European Union | 2 | € |
| GBP | Pound Sterling | United Kingdom | 2 | £ |
| JPY | Japanese Yen | Japan | 0 | ¥ |
| CNY | Chinese Yuan | China | 2 | ¥ |

## ASEAN Currencies

| Code | Currency | Country | Decimal Places | Symbol |
|------|----------|---------|----------------|---------|
| BND | Brunei Dollar | Brunei | 2 | $ |
| KHR | Cambodian Riel | Cambodia | 2 | ៛ |
| IDR | Indonesian Rupiah | Indonesia | 2 | Rp |
| LAK | Lao Kip | Laos | 2 | ₭ |
| MMK | Myanmar Kyat | Myanmar | 2 | K |
| PHP | Philippine Peso | Philippines | 2 | ₱ |
| SGD | Singapore Dollar | Singapore | 2 | $ |
| THB | Thai Baht | Thailand | 2 | ฿ |
| VND | Vietnamese Dong | Vietnam | 0 | ₫ |

## Other Common Currencies

| Code | Currency | Country/Region | Decimal Places | Symbol |
|------|----------|----------------|----------------|---------|
| AUD | Australian Dollar | Australia | 2 | $ |
| CAD | Canadian Dollar | Canada | 2 | $ |
| CHF | Swiss Franc | Switzerland | 2 | Fr |
| HKD | Hong Kong Dollar | Hong Kong | 2 | $ |
| INR | Indian Rupee | India | 2 | ₹ |
| KRW | South Korean Won | South Korea | 0 | ₩ |
| NZD | New Zealand Dollar | New Zealand | 2 | $ |
| SAR | Saudi Riyal | Saudi Arabia | 2 | ﷼ |
| TWD | Taiwan Dollar | Taiwan | 2 | NT$ |
| AED | UAE Dirham | UAE | 2 | د.إ |

## Usage in API

### Amount Example

```json
{
  "amount": {
    "currency": "MYR",
    "value": 1000.00
  }
}
```

### Multi-currency Transaction Example

```json
{
  "invoice": {
    "currency": "USD",
    "exchangeRate": {
      "sourceCurrency": "USD",
      "targetCurrency": "MYR",
      "rate": 4.73,
      "date": "2024-03-14"
    },
    "amounts": {
      "subtotal": {
        "currency": "USD",
        "value": 1000.00,
        "localEquivalent": {
          "currency": "MYR",
          "value": 4730.00
        }
      },
      "tax": {
        "currency": "USD",
        "value": 60.00,
        "localEquivalent": {
          "currency": "MYR",
          "value": 283.80
        }
      },
      "total": {
        "currency": "USD",
        "value": 1060.00,
        "localEquivalent": {
          "currency": "MYR",
          "value": 5013.80
        }
      }
    }
  }
}
```

## Validation Rules

1. Currency codes must be:
   - 3 characters long
   - Uppercase letters only
   - Valid ISO 4217 code

2. Amount validation:
   - Must respect currency decimal places
   - No negative amounts for invoices
   - Maximum 2 decimal places for most currencies

3. Exchange rates:
   - Required for foreign currencies
   - Must be from Bank Negara Malaysia
   - Must be within 1% of official rate
   - Date must be current business day

## Notes

1. Currency codes follow ISO 4217 standard
2. MYR is the base currency for the system
3. Exchange rates updated daily from BNM
4. Some currencies have no decimal places (JPY, KRW)
5. Local equivalent in MYR required for reporting
6. Exchange rates include buy and sell rates
7. Historical rates available for past transactions
8. Currency symbols used in PDF documents
9. Amounts rounded according to currency rules
10. Special handling for zero-decimal currencies

## Exchange Rate Sources

1. Primary: Bank Negara Malaysia (BNM)
2. Secondary: European Central Bank (ECB)
3. Fallback: International Monetary Fund (IMF)

## Currency Formatting

| Currency | Format Example | Note |
|----------|----------------|------|
| MYR | RM 1,234.56 | Space after RM |
| USD | $1,234.56 | No space after $ |
| EUR | € 1.234,56 | European format |
| JPY | ¥1,235 | No decimals |
| CNY | ¥1,234.56 | Different from JPY |
| GBP | £1,234.56 | No space after £ |

## Implementation Guidelines

1. Always store amounts with maximum precision
2. Convert only for display purposes
3. Use BigDecimal for calculations
4. Apply rounding rules last
5. Include exchange rate metadata
6. Log currency conversions
7. Handle zero-decimal currencies
8. Validate against latest rates
9. Support historical rates
10. Monitor rate fluctuations 