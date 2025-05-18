# Digital Signature Documentation

This guide describes the signature elements, creation process, and validation requirements for MyInvois documents.

## Introduction

Documents submitted to MyInvois must be digitally signed following UBL 2.1 standards:
- XML documents: [Universal Business Language Version 2.1](https://docs.oasis-open.org/ubl/UBL-2.1.html)
- JSON documents: [UBL 2.1 JSON Alternative Representation Version 2.0](https://docs.oasis-open.org/ubl/UBL-2.1-JSON/v2.0/cnd01/UBL-2.1-JSON-v2.0-cnd01.html)

> **Important**: Submitters must use a valid digital certificate issued by a Malaysian certificate authority (CA). See [List of Certification Authorities](https://www.mcmc.gov.my/en/sectors/digital-signature/list-of-licensees).

## Digital Signing Certificate Profile

### Certificate Distinguished Name Requirements

| Display Name | OID | Expected Content | Example |
|--------------|-----|------------------|---------|
| Common name (CN) | 2.5.4.3 | Organization name | Company Name Sdn Bhd |
| Country (C) | 2.5.4.6 | 2-letter ISO code | MY |
| Email (E) | 1.2.840.113549.1.9.1 | Optional email | noemail@org.com.my |
| Organization (O) | 2.5.4.10 | Organization name | Company Name Sdn Bhd |
| Organization identifier | 2.5.4.97 | Tax ID Number (TIN) | C20830570210 |
| Organization Unit (OU) | 2.5.4.11 | Optional unit name | |
| Serial number | 2.5.4.5 | Business registration number | 202005123456 |

### Key Usage Requirements

1. **Key Usage**
   - Must include "Non-Repudiation (40)"
   - Can include additional usages

2. **Enhanced Key Usage**
   - Must include "Document Signing (1.3.6.1.4.1.311.10.3.12)"
   - Can include additional usages (e.g., "Secure Email")

## Digital Signature Requirements

1. **Algorithm**: XAdES (XML Advanced Electronic Signature)
2. **Hashing**: SHA 256
3. **Signature**: RSA
4. **Profile**: Enveloped signature
5. **Timestamp**: Required in signed properties
6. **Single Signature**: Only one signature element allowed

## Signature Structure

### Main Components

1. **Signature Element** (Mandatory, [1..1])
   - Root element containing digital signature information
   - ExtensionURI: `urn:oasis:names:specification:ubl:dsig:enveloped:xades`

2. **Signed Information** (Mandatory, [1..1])
   - Document transformation details
   - Signature method and canonicalization
   - Document digest and properties

3. **Key Information** (Mandatory, [1..1])
   - X509 certificate public information

4. **Object Element** (Mandatory, [1..1])
   - Contains signed properties
   - Includes xades:QualifyingProperties

## Signature Creation Process

1. **Document Preparation**
   - Exclude UBLExtension sections
   - Exclude existing Signature elements
   - Apply SHA256 hashing

2. **Required Values**
   - Signature value (Sig)
   - Document digest (DocDigest)
   - Properties digest (PropsDigest)
   - Certificate digest (CertDigest)

## Validation Rules

1. **Structure**
   - Must be valid XAdES structure
   - Must be Base64 encoded

2. **Certificate**
   - Must be valid at submission
   - Must match issuer or service provider
   - Must be from approved Malaysian CA

3. **Signature**
   - Must be valid RSA signature
   - Must include required properties

## Sample Files

- [UBL 2.1 Invoice Sample XML with Signature](/files/one-doc-signed.xml)
- [UBL 2.1 Invoice Sample JSON with Signature](/files/sample-ul-invoice-2.1-signed.min.json)

> **Disclaimer**: Sample JSON file is for illustration only. Consult local laws for implementation guidance. 