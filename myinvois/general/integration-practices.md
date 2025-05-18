# Integration Practices

This guide explains best practices for integrating with the MyInvois API.

## Polling Approach

In this integration approach, the ERP system submits documents using submission APIs and then checks the status using submission reference numbers.

### Key Integration Steps

1. **Login**
   - Use [Login as Taxpayer](/api/07-login-as-taxpayer-system/) API
   - Cache access tokens (valid for 60 minutes)
   - Investigate and resolve login failures

2. **Submit Documents**
   - Use [Submit documents](/einvoicingapi/02-submit-documents/) API
   - Wait appropriate time based on submission size
   - Store submission ID for status checking

3. **Check Status**
   - Use [Get Submission](/einvoicingapi/06-get-submission/) API with submission UID
   - Update internal ERP system when processing completes

## Common Anti-patterns to Avoid

1. **Token Management**
   - DON'T acquire new authentication token for every API call
   - DO cache tokens for their full 60-minute validity period

2. **Status Checking**
   - DON'T check individual document statuses during submission
   - DO use the Get Submission API for submission status

3. **Submission Handling**
   - DON'T resubmit without checking API responses
   - DO only resubmit on error responses (not on 20x status codes)

4. **API Usage**
   - DON'T overuse Get Recent Documents API
   - DO prefer the optimized Search Documents API

5. **Authentication**
   - DON'T call authentication API without required parameters
   - DO always include client_id, grant_type, and client_secret

## Rate Limits

| API | Requests Per Minute (RPM) per Client Id |
|-----|----------------------------------------|
| Login as Taxpayer System | 12 |
| Login as Intermediary System | 12 |
| Submit Documents | 100 |
| Get Submission | 300 |
| Cancel Document | 12 |
| Reject Document | 12 |
| Get Document | 60 |
| Get Document Details | 125 |
| Get Recent Documents | 12 |
| Search Documents | 12 |
| Search Taxpayer's TIN | 60 |
| Taxpayer's QR Code | 60 |

### Rate Limit Handling
- Returns 429 Too Many Requests status code
- Includes Retry-After header for wait time
- See [error response documentation](https://sdk.myinvois.hasil.gov.my/standard-error-response/)

**Note:** Sandbox environment has lower rate limits than production (as of April 28, 2025). Sandbox data is retained for 3 months maximum.

## Document Submission Code Example

```python
# Main function to orchestrate the process
def main():
    # Step 1: Login as Taxpayer System
    token = get_token_from_login_endpoint(client_id, client_secret)
    # Cache the token based on expires_in response parameter
    
    # Step 2: Submit documents
    base64_document = encode_document_to_base64()
    request_body = append_base64_to_request_body()
    submission_uid = submit_documents(request_body, token)
    
    # Step 3: Poll for Submission Status
    submission_status = poll_submission_status(submission_uid)
    
    # Step 4: Process further

# Define function to submit documents via SubmitDocuments API
def submit_documents(request_body, token):
    response = call_submit_documents_api(request_body, token)
    submission_uid = response.submission_uid
    return submission_uid

# Define function to poll GetSubmission API for Submission Status
def poll_submission_status(submission_uid):
    in_progress = True
    while in_progress:
        submission = call_document_submissions_api(submission_uid)
        submission_status = submission.overall_status
        if submission_status.is_in_progress:
            wait_for_some_time()            
        else:
            in_progress = False
            return submission_status
``` 