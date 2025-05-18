# e-Invoice APIs

Definition of the API that taxpayer systems can leverage to deal with the actual Invoice documents.

## Validate Taxpayer's TIN

This API allows taxpayer's ERP system to validate specific Tax Identification Number (TIN) before adding this number to an invoice and issuing the invoice.

[Read more](/einvoicingapi/01-validate-taxpayer-tin/)

## Submit Documents

This API allows taxpayer to submit one or more signed documents to MyInvois System.

[Read more](/einvoicingapi/02-submit-documents/)

## Cancel Document

This API allows issuer to cancel previously issued document, either self-induced cancellation or by accepting a rejection request made by the buyer.

[Read more](/einvoicingapi/03-cancel-document/)

## Reject Document

This API allows a buyer that received an invoice to reject it and request the supplier to cancel it.

[Read more](/einvoicingapi/04-reject-document/)

## Get Recent Documents

This API allows taxpayer's systems to search the documents sent or received which are available on the MyInvois System using various filters. This API will only return documents that are issued within the last 31 days.

[Read more](/einvoicingapi/05-get-recent-documents/)

## Get Submission

This API returns information on documents submitted during a single submission by taxpayer.

[Read more](/einvoicingapi/06-get-submission/)

## Get Document

This API allows taxpayers to retrieve document source in XML or JSON format along with the additional tax authority metadata.

[Read more](/einvoicingapi/07-get-document/)

## Get Document Details

This API allows taxpayers to retrieve a single document's full details including validation results.

[Read more](/einvoicingapi/08-get-document-details/)

## Search Documents

This API allows taxpayer's systems to search the documents sent or received which are available on the MyInvois System using various filters.

[Read more](/einvoicingapi/09-search-documents/)

## Search Taxpayer's TIN

This API allows taxpayer's ERP system to search for a specific Tax Identification Number (TIN) using the supported search parameters. The available search parameters are either the Taxpayer Name or ID Type and ID Value, or all three parameters combined. If all parameters are provided then the search would use an AND operator to make sure the result found matches all search parameters provided.

[Read more](/einvoicingapi/10-search-taxpayer-tin/)

## Taxpayer's QR Code

This API allows taxpayer's ERP system to search and retrieve the information for a specific Taxpayer based on the Base64 formatted string obtained from scanning the respective QR code.

[Read more](/einvoicingapi/11-qr-code/) 