# Payment Methods

This document lists the standard payment methods supported by the MyInvois System.

## Standard Payment Methods

| Code | Method | Description | Category |
|------|--------|-------------|-----------|
| CASH | Cash | Physical cash payment | Cash |
| BANK | Bank Transfer | Direct bank transfer/wire | Electronic |
| CRED | Credit Card | Credit card payment | Card |
| DEBI | Debit Card | Debit card payment | Card |
| CHEK | Cheque | Bank cheque/check | Paper |
| DRFT | Bank Draft | Bank draft | Paper |
| LETT | Letter of Credit | Documentary credit | Trade |
| COUN | Counter Payment | Payment at counter | Cash |
| ONLN | Online Banking | Internet banking | Electronic |
| EWLT | E-Wallet | Digital wallet payment | Electronic |
| QRPY | QR Payment | QR code payment | Electronic |
| CRYP | Cryptocurrency | Digital currency payment | Electronic |
| FINT | Fintech Platform | Payment via fintech | Electronic |
| BNPL | Buy Now Pay Later | Deferred payment | Credit |
| CRED | Trade Credit | Business credit terms | Credit |

## Payment Categories

### Cash Methods

| Code | Method | Features |
|------|---------|----------|
| CASH | Cash | Physical notes and coins |
| COUN | Counter | Over-the-counter payment |

### Electronic Methods

| Code | Method | Features |
|------|---------|----------|
| BANK | Bank Transfer | GIRO, RENTAS, SWIFT |
| ONLN | Online Banking | FPX, IBG, DuitNow |
| EWLT | E-Wallet | Touch 'n Go, GrabPay, Boost |
| QRPY | QR Payment | DuitNow QR, PayNet QR |
| FINT | Fintech | Payment aggregators |

### Card Methods

| Code | Method | Features |
|------|---------|----------|
| CRED | Credit Card | Visa, Mastercard, AMEX |
| DEBI | Debit Card | MyDebit, Visa Debit |

### Paper Methods

| Code | Method | Features |
|------|---------|----------|
| CHEK | Cheque | Personal, company cheques |
| DRFT | Bank Draft | Banker's cheque |

### Credit Methods

| Code | Method | Features |
|------|---------|----------|
| BNPL | Buy Now Pay Later | Installment plans |
| CRED | Trade Credit | Payment terms |

### Trade Methods

| Code | Method | Features |
|------|---------|----------|
| LETT | Letter of Credit | International trade |

## Usage in API

### Invoice Payment Method Example

```json
{
  "invoice": {
    "paymentMethods": [
      {
        "code": "BANK",
        "details": {
          "bankName": "Maybank",
          "accountNumber": "514024123456",
          "accountName": "ABC Trading Sdn Bhd",
          "swiftCode": "MBBEMYKL"
        }
      },
      {
        "code": "QRPY",
        "details": {
          "provider": "DuitNow",
          "merchantId": "M100123456",
          "qrCode": "base64_encoded_qr_image"
        }
      }
    ]
  }
}
```

### Payment Record Example

```json
{
  "payment": {
    "invoiceId": "INV-2024-001",
    "method": "BANK",
    "amount": 1060.00,
    "currency": "MYR",
    "date": "2024-03-14",
    "reference": "MB123456789",
    "status": "COMPLETED",
    "details": {
      "bankName": "Maybank",
      "accountNumber": "514024123456",
      "transferType": "IBG"
    }
  }
}
```

## Required Information

### Bank Transfer (BANK)

| Field | Required | Description |
|-------|----------|-------------|
| bankName | Yes | Name of the bank |
| accountNumber | Yes | Bank account number |
| accountName | Yes | Account holder name |
| swiftCode | No | SWIFT/BIC code |
| bankBranch | No | Branch location |

### QR Payment (QRPY)

| Field | Required | Description |
|-------|----------|-------------|
| provider | Yes | QR provider name |
| merchantId | Yes | Merchant identifier |
| qrCode | Yes | QR code image/data |
| expiryTime | No | QR code expiry |

### E-Wallet (EWLT)

| Field | Required | Description |
|-------|----------|-------------|
| provider | Yes | Wallet provider |
| merchantId | Yes | Merchant ID |
| phoneNumber | No | Linked phone number |
| qrCode | No | QR code if applicable |

## Validation Rules

1. Method Code:
   - Must be valid code
   - Case sensitive
   - No spaces allowed

2. Amount Limits:
   - CASH: ≤ RM 10,000
   - BANK: No limit
   - CARD: Per bank limits
   - CHEK: No limit

3. Currency Rules:
   - Local methods: MYR only
   - International: Multiple currencies
   - Crypto: Specific tokens

4. Timing Rules:
   - Business hours only
   - Settlement times vary
   - Expiry for QR codes

## Notes

1. Supported methods vary by:
   - Business type
   - Transaction amount
   - Customer location
2. Settlement times:
   - Real-time: ONLN, EWLT
   - Same day: BANK
   - 1-3 days: CHEK, DRFT
3. Fee structures vary
4. Compliance requirements:
   - AML regulations
   - Payment card standards
   - Banking regulations
5. Integration requirements:
   - API endpoints
   - Security protocols
   - Testing environments
6. Reconciliation support
7. Refund capabilities
8. Dispute handling
9. Audit trail requirements
10. Reporting features 