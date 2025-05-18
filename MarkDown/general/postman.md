# Testing APIs with Postman

This guide explains how to test the MyInvois APIs using Postman.

## Getting Started

[Postman](https://www.postman.com/) can be used to quickly test and understand how to call both the identity service and document exchange APIs.

## Required Files

1. **Collection File**
   - [e-Invoice SDK.postman_collection.json](/files/EInvoicing%20SDK.postman_collection.json)
   - Right-click to save the file

2. **Environment Files**

| Environment | File |
|------------|------|
| Production | [PROD Env.postman_environment.json](/files/PROD%20Env.postman_environment.json) |
| Sandbox | [Sandbox Env.postman_environment.json](/files/SANDBOX%20Env.postman_environment.json) |

## Installation Steps

1. **Access Environment Manager**
   - Click the gear icon (⚙️) in top right corner
   - Select "Manage Environments"

2. **Import Environment**
   - Click "Import" button
   - Select environment file
   - Click "Ok"

3. **Select Environment**
   - New environment ("Sandbox Env" or "Prod Env") appears in dropdown
   - Click gear icon
   - Select desired environment

## Troubleshooting SSL Issues

If you encounter "SSL Error: Self signed certificate in certificate chain":

### Option 1: Quick Fix
- Click "Disable SSL Verification" in the error dialog

### Option 2: Settings Change
1. Click gear icon for Settings
2. Turn off "SSL Certificate Verification"

## Related Documentation
- [Digital Signature Guide](/signature/)
- [Integration Practices](/integration-practices/)
- [API Documentation](/api/) 