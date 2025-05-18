# Application Programming Interface (API)

Platform API focuses on key platform functionalities and not dealing with the actual Invoice document.

## Login as Taxpayer System

This API is used to authenticate the ERP system associated with a specific taxpayer calling and issue access token which allows ERP system to access those protected APIs.

[Read more](/api/07-login-as-taxpayer-system/)

## Login as Intermediary System

This API is used to authenticate the ERP system associated with an intermediary that is representing a taxpayer (acting on behalf of a specific taxpayer) calling and issue access token which allows ERP system to access those protected APIs.

[Read more](/api/08-login-as-intermediary-system/)

## Get All Document Types

This API allows taxpayer's systems to retrieve list of document types published by the MyInvois System.

[Read more](/api/03-get-document-types/)

## Get Document Type

This API allows taxpayer's ERP system to retrieve the details of single document type that contains structure definitions of the document.

[Read more](/api/04-get-document-type/)

## Get Document Type Version

This API allows taxpayer's ERP system to retrieve the details of document type version that contains structure definitions of the documents.

[Read more](/api/05-get-document-type-version/)

## Get Notifications

This API allows ERP system to query for previously sent notifications.

[Read more](/api/06-get-notifications/) 