# Self-Billed Debit Note v1.1

v1.1 of the Self-Billed Debit Note document type structure is the current version that includes signature validation. This version supersedes v1.0 and includes all the same data structure with additional signature validation requirements.

## Overview

Self-Billed Debit Note is a document type supported by the MyInvois System. It is used when the buyer creates a debit note on behalf of the supplier to adjust previously issued self-billed invoices with the Tax Authority.

### Guidance on constructing UBL

In the below Data Structure, if the UBL Schema mapping says:

`/ ubl:DebitNote / cbc:DebitNoteTypeCode / @listVersionID`

which means that it needs to go under DebitNote element as a DebitNoteTypeCode element with an attribute listVersionId.

For Example - The UBL XML for DebitNoteTypeCode will look like this:

```xml
<DebitNote>
    <cbc:DebitNoteTypeCode listVersionID="1.1">13</cbc:DebitNoteTypeCode>
</DebitNote>
```

## Data Structure

### Core

The data structure of the self-billed debit note v1.1 document consists of a single `document` element that contains these additional elements. Note that this description defines the data structure while actual attribute naming could be different for JSON and XML data structures.

| ELEMENT | Field TYPE | DESCRIPTION | VALUE EXAMPLE | UBL Schema Mapping | Mandatory | Number of Chars | Cardinality |
|---------|------------|-------------|---------------|-------------------|-----------|----------------|-------------|
| Supplier | [Supplier](#supplier) | Structure representing the supplier information. | | / ubl:DebitNote / cac:AccountingSupplierParty | Mandatory | | |
| Buyer | [Buyer](#buyer) | Structure representing the buyer information. | | / ubl:DebitNote / cac:AccountingCustomerParty | Mandatory | | |
| e-Invoice Version | xsd:normalizedString | Current e-Invoice version (e.g., 1.0, 2.0, etc.) | 1.1 | / ubl:DebitNote / cbc:DebitNoteTypeCode / @listVersionID | Mandatory | 5 | [1-1] |

## Digital Signature Requirements

v1.1 includes mandatory digital signature validation. The signature must be:
1. Created using a valid digital certificate from an approved Certificate Authority
2. Applied to the entire document content
3. Included in the UBL structure according to the UBL 2.1 signature specifications

## Sample

- [Self-Billed Debit Note 1.1 Sample XML](/files/sdksamples/1.1-SelfBilledDebitNote-Sample.xml)
- [Self-Billed Debit Note 1.1 Sample JSON](/files/sdksamples/1.1-SelfBilledDebitNote-Sample.json) 
- [Self-Billed Debit Note 1.1 Foreign Currency Sample XML](/files/sdksamples/1.1-SelfBilledDebitNote-ForeignCurrency-Sample.xml)
- [Self-Billed Debit Note 1.1 Foreign Currency Sample JSON](/files/sdksamples/1.1-SelfBilledDebitNote-ForeignCurrency-Sample.json)
- [Self-Billed Debit Note 1.1 with Multi Line Item Sample XML](/files/sdksamples/1.1-SelfBilledDebitNote-MultiLineItem-Sample.xml)
- [Self-Billed Debit Note 1.1 with Multi Line Item Sample JSON](/files/sdksamples/1.1-SelfBilledDebitNote-MultiLineItem-Sample.json)

Note: Please note that the above sample is to provide guidance on UBL 2.1 XML only and taxpayers should not rely on the example provided. It is recommended for taxpayers to follow the UBL 2.1 structure. For JSON format, taxpayers should follow the UBL schema in a similar way. 