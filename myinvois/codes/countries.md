# Country Codes

This document lists the standard country codes used in the MyInvois System. The codes follow the ISO 3166-1 alpha-2 standard.

## ASEAN Countries

| Code | Country | Currency | Dial Code |
|------|---------|----------|-----------|
| BN | Brunei Darussalam | BND | +673 |
| KH | Cambodia | KHR | +855 |
| ID | Indonesia | IDR | +62 |
| LA | Lao People's Democratic Republic | LAK | +856 |
| MY | Malaysia | MYR | +60 |
| MM | Myanmar | MMK | +95 |
| PH | Philippines | PHP | +63 |
| SG | Singapore | SGD | +65 |
| TH | Thailand | THB | +66 |
| VN | Viet Nam | VND | +84 |

## Major Trading Partners

| Code | Country | Currency | Dial Code |
|------|---------|----------|-----------|
| AU | Australia | AUD | +61 |
| CN | China | CNY | +86 |
| DE | Germany | EUR | +49 |
| GB | United Kingdom | GBP | +44 |
| HK | Hong Kong | HKD | +852 |
| IN | India | INR | +91 |
| JP | Japan | JPY | +81 |
| KR | Korea, Republic of | KRW | +82 |
| NZ | New Zealand | NZD | +64 |
| TW | Taiwan | TWD | +886 |
| US | United States | USD | +1 |

## Other Common Countries

| Code | Country | Currency | Dial Code |
|------|---------|----------|-----------|
| AE | United Arab Emirates | AED | +971 |
| CA | Canada | CAD | +1 |
| CH | Switzerland | CHF | +41 |
| ES | Spain | EUR | +34 |
| FR | France | EUR | +33 |
| IT | Italy | EUR | +39 |
| NL | Netherlands | EUR | +31 |
| RU | Russian Federation | RUB | +7 |
| SA | Saudi Arabia | SAR | +966 |
| ZA | South Africa | ZAR | +27 |

## Usage in API

### Address Example

```json
{
  "address": {
    "line1": "123 Jalan Business",
    "line2": "Taman Enterprise",
    "city": "Kuala Lumpur",
    "state": "WP Kuala Lumpur",
    "postcode": "50000",
    "country": "MY"
  }
}
```

### International Transaction Example

```json
{
  "seller": {
    "country": "MY",
    "currency": "MYR"
  },
  "buyer": {
    "country": "SG",
    "currency": "SGD"
  },
  "exchangeRate": {
    "sourceCurrency": "MYR",
    "targetCurrency": "SGD",
    "rate": 0.31,
    "date": "2024-03-14"
  }
}
```

## Validation Rules

1. Country codes must be:
   - 2 characters long
   - Uppercase letters only
   - Valid ISO 3166-1 alpha-2 code

2. Address validation:
   - Country code required for all addresses
   - Must match postal format of country
   - State/province required for certain countries

3. Transaction validation:
   - Cross-border transactions require exchange rates
   - Currency must match country's official currency
   - Special rules for ASEAN countries

## Notes

1. Country codes follow ISO 3166-1 alpha-2 standard
2. Currency codes follow ISO 4217 standard
3. Dial codes follow ITU-T E.164 standard
4. ASEAN countries have preferential treatment
5. Exchange rates updated daily from Bank Negara Malaysia
6. Address formats validated by country
7. Postal code formats validated by country
8. State/province required for MY, US, CA, AU
9. Special handling for sanctioned countries
10. EU countries use common currency (EUR)

## File Download

[Download JSON file](/files/CountryCodes.json) containing definition of all the country codes.

## Additional Considerations

Note that when submitting the documents, taxpayers should be using the code values. 