# Frequently Asked Questions

Answers to frequently asked questions about MyInvois system integration approach

## General

### How to retrieve and validate the accuracy of my TIN?

To facilitate the retrieval of Tax Identification Number (TIN), there are multiple channels available for taxpayers:

• Check the TIN via the [MyTax](https://mytax.hasil.gov.my/) Portal (e-Daftar or Your Profile Information)  
• Validate the TIN via the [Validate Taxpayer's TIN](https://sdk.myinvois.hasil.gov.my/einvoicingapi/01-validate-taxpayer-tin/) API before submitting e-Invoice  
• Contact the HASiL Contact Centre (03-8911 1000); or visit the nearest LHDNM offices

**For Individual TIN (with prefix IG):**

• The new prefix for individual TIN is now 'IG' (replacing 'OG' or 'SG'). For example, if the TIN is 'SG123456789', please input it as 'IG123456789'  
• The numeric character within the TIN remains unchanged, with a maximum length of 14 characters including the prefix

**For Non-Individual TIN (with prefix C, CS, D, F, FA, PT, TA, TC, TN, TR, TP, J & LE):**

• If your TIN begins with a zero '0' after the prefix, please remove any initial zeros after the prefix for successful validation. For example, if the TIN is 'C01234567890', please input it as 'C1234567890'  
• If your TIN does not end with a zero '0' (e.g., C123456789), please ensure you add an additional zero '0' at the end (e.g., C1234567890). Please note that Non-Individual TIN always ends with zero '0'

### What are the environment URLs?

| Env | Invoicing Portal | System API | Identity Service |
|-----|-----------------|------------|------------------|
| PROD | myinvois.hasil.gov.my | api.myinvois.hasil.gov.my | api.myinvois.hasil.gov.my |
| SANDBOX | preprod.myinvois.hasil.gov.my | preprod-api.myinvois.hasil.gov.my | preprod-api.myinvois.hasil.gov.my |

### What are the various document statuses within the e-Invoice workflow?

The invoice is stored in the local database with a specific status. The status is an integer value, which could be one of the following:

| Status | Value | Note |
|--------|-------|------|
| Submitted | 1 | This means the invoice has passed initial structure validations but is still pending additional validations to be completed |
| Valid | 2 | The status of a successful invoice validation |
| Invalid | 3 | The status of a submitted invoice with validation issues |
| Cancelled | 4 | The status of an invoice cancelled by the issuer |

## Application Programming Interface (API)

### How to get sample payload?

You can download the JSON and XML [Sample](/sample) payloads for various document types and scenarios from the Sample page.

### What is the Rate Limit for each API?

The rate limiting functionality implementation is using the standard HTTP rate limiting headers. Refer [here](https://sdk.myinvois.hasil.gov.my/integration-practices/#rate-limit) for the rate limit for each API. Hence, the caller should handle the rate limiting headers that are returned to them by the API and retry the call as per these headers. The header would specify the current number of calls that are rate limited and the time period the caller should wait before they make the next call.

Common observations:

1. Login Too Frequent - The system token is valid for 60 minutes. Use this token for API operations rather than generating a new one for each request.
2. High Volume Submissions – To maintain system stability, usage limits will be enforced on the Submit Documents. It is therefore advisable to submit e-Invoices in batches and in accordance with the applicable rate limits to prevent potential disruptions.

### What are the Supplier/Buyer Information Field Validation?

| No | Field | Validation Rules |
|----|-------|-----------------|
| i | Supplier/Buyer Name | 1) Cannot exceed 300 characters |
| ii | Email Address | 1) Must follow valid format (RFC 5321 & RFC 5322)<br>2) Maximum 320 characters<br>3) No space between the characters<br>4) If no email, just leave it blank |
| iii | Phone Number | 1) Minimum 8 and maximum 20 characters<br>2) Only allow optional plus (+) symbol at the front of the number<br>3) No space between the digits |
| iv | Address (Line 1-3) | 1) Maximum 150 characters for every line |
| v | City Name | 1) Maximum 50 characters |
| vi | Postal Code | 1) If country is Malaysia, maximum 5-digit numbers only<br>2) Country other than Malaysia, maximum is 50 alphanumeric characters including special characters |
| vii | SST Number | 1) Maximum 35 characters<br>2) Only allows dash (-) and semicolon (;) special characters<br>3) Up to 2 SST numbers separated by semicolon (;)<br>4) Allows 'NA' value if no SST number |
| viii | TTX Number | 1) Maximum 17 characters<br>2) Only allows dash (-) special character<br>3) Allows 'NA' value if no TTX number |

[View full FAQ documentation](https://sdk.myinvois.hasil.gov.my/faq/) 