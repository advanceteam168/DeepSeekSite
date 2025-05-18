# Invoice v1.1

v1.1 of the Invoice document type structure is the current version that includes signature validation. This version supersedes v1.0 and includes all the same data structure with additional signature validation requirements.

## Overview

Invoice is the main document type supported by the MyInvois System. It is used to submit issued invoices with the Tax Authority.

### Guidance on constructing UBL

In the below Data Structure, if the UBL Schema mapping says:

`/ ubl:Invoice / cbc:InvoiceTypeCode / @listVersionID`

which means that it needs to go under Invoice element as a InvoiceTypeCode element with an attribute listVersionId.

For Example - The UBL XML for InvoiceTypeCode will look like this:

```xml
<Invoice>
    <cbc:InvoiceTypeCode listVersionID="1.1">01</cbc:InvoiceTypeCode>
</Invoice>
```

## Data Structure

### Core

The data structure of the invoice v1.1 document consists of a single `document` element that contains these additional elements. Note that this description defines the data structure while actual attribute naming could be different for JSON and XML data structures.

| ELEMENT | Field TYPE | DESCRIPTION | VALUE EXAMPLE | UBL Schema Mapping | Mandatory | Number of Chars | Cardinality |
|---------|------------|-------------|---------------|-------------------|-----------|----------------|-------------|
| Supplier | [Supplier](#supplier) | Structure representing the supplier information. | | / ubl:Invoice / cac:AccountingSupplierParty | Mandatory | | |
| Buyer | [Buyer](#buyer) | Structure representing the buyer information. | | / ubl:Invoice / cac:AccountingCustomerParty | Mandatory | | |
| e-Invoice Version | xsd:normalizedString | Current e-Invoice version (e.g., 1.0, 2.0, etc.) | 1.1 | / ubl:Invoice / cbc:InvoiceTypeCode / @listVersionID | Mandatory | 5 | [1-1] |

## Digital Signature Requirements

v1.1 includes mandatory digital signature validation. The signature must be:
1. Created using a valid digital certificate from an approved Certificate Authority
2. Applied to the entire document content
3. Included in the UBL structure according to the UBL 2.1 signature specifications

## Sample

- [Invoice 1.1 Sample XML](/files/sdksamples/1.1-Invoice-Sample.xml)
- [Invoice 1.1 Sample JSON](/files/sdksamples/1.1-Invoice-Sample.json) 
- [Invoice 1.1 Foreign Currency Sample XML](/files/sdksamples/1.1-Invoice-ForeignCurrency-Sample.xml)
- [Invoice 1.1 Foreign Currency Sample JSON](/files/sdksamples/1.1-Invoice-ForeignCurrency-Sample.json)
- [Invoice 1.1 with Multi Line Item Sample XML](/files/sdksamples/1.1-Invoice-MultiLineItem-Sample.xml)
- [Invoice 1.1 with Multi Line Item Sample JSON](/files/sdksamples/1.1-Invoice-MultiLineItem-Sample.json)
- [Invoice 1.1 Consolidated Sample XML](/files/sdksamples/1.1-Invoice-Consolidated-Sample.xml)
- [Invoice 1.1 Consolidated Sample JSON](/files/sdksamples/1.1-Invoice-Consolidated-Sample.json)

Note: Please note that the above sample is to provide guidance on UBL 2.1 XML only and taxpayers should not rely on the example provided. It is recommended for taxpayers to follow the UBL 2.1 structure. For JSON format, taxpayers should follow the UBL schema in a similar way. 