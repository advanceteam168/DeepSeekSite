Here's the C# implementation based on the API specification:

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using System.Linq;
using System.Net.Http;
using System.Runtime.Serialization;
using System.Threading.Tasks;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;

public class DocumentSearchRequest
{
    public string Uuid { get; set; }

    public DateTimeOffset? SubmissionDateFrom { get; set; }

    public DateTimeOffset? SubmissionDateTo { get; set; }

    [Range(1, 100)]
    public int? PageSize { get; set; }

    public int? PageNo { get; set; }

    public DateTimeOffset? IssueDateFrom { get; set; }

    public DateTimeOffset? IssueDateTo { get; set; }

    [JsonConverter(typeof(StringEnumConverter))]
    public InvoiceDirection? InvoiceDirection { get; set; }

    [JsonConverter(typeof(StringEnumConverter))]
    public DocumentStatus? Status { get; set; }

    public DocumentType? DocumentType { get; set; }

    public string SearchQuery { get; set; }
}

public enum InvoiceDirection
{
    Sent,
    Received
}

public enum DocumentStatus
{
    Valid,
    Invalid,
    Cancelled,
    Submitted
}

public enum DocumentType
{
    [EnumMember(Value = "01")] Invoice,
    [EnumMember(Value = "02")] CreditNote,
    [EnumMember(Value = "03")] DebitNote,
    [EnumMember(Value = "04")] RefundNote,
    [EnumMember(Value = "11")] SelfBilledInvoice,
    [EnumMember(Value = "12")] SelfBilledCreditNote,
    [EnumMember(Value = "13")] SelfBilledDebitNote,
    [EnumMember(Value = "14")] SelfBilledRefundNote
}

public class DocumentSearchResult
{
    [JsonProperty("documents")] 
    public List<Document> Documents { get; set; }

    [JsonProperty("metadata")] 
    public Metadata Metadata { get; set; }
}

public class Document
{
    [JsonProperty("uuid")] public string Uuid { get; set; }
    [JsonProperty("submissionUID")] public string SubmissionUID { get; set; }
    [JsonProperty("longld")] public string LongId { get; set; }
    [JsonProperty("internalld")] public string InternalId { get; set; }
    [JsonProperty("typeName")] public string TypeName { get; set; }
    [JsonProperty("typeVersionName")] public string TypeVersionName { get; set; }
    [JsonProperty("issuerTin")] public string IssuerTin { get; set; }
    [JsonProperty("issuerName")] public string IssuerName { get; set; }
    [JsonProperty("receiverld")] public string ReceiverId { get; set; }
    [JsonProperty("receiverName")] public string ReceiverName { get; set; }
    [JsonProperty("dateTimelssued")] public DateTimeOffset DateTimeIssued { get; set; }
    [JsonProperty("dateTimeReceived")] public DateTimeOffset DateTimeReceived { get; set; }
    [JsonProperty("dateTimeValidated")] public DateTimeOffset DateTimeValidated { get; set; }
    [JsonProperty("totalSales")] public decimal TotalSales { get; set; }
    [JsonProperty("totalDiscount")] public decimal TotalDiscount { get; set; }
    [JsonProperty("netAmount")] public decimal NetAmount { get; set; }
    [JsonProperty("total")] public decimal Total { get; set; }
    [JsonProperty("status")] public string Status { get; set; }
    [JsonProperty("cancelDateTime")] public DateTimeOffset? CancelDateTime { get; set; }
    [JsonProperty("rejectRequestDateTime")] public DateTimeOffset? RejectRequestDateTime { get; set; }
    [JsonProperty("documentStatusReason")] public string DocumentStatusReason { get; set; }
    [JsonProperty("createdByUserld")] public string CreatedByUserId { get; set; }
    [JsonProperty("supplierTIN")] public string SupplierTIN { get; set; }
    [JsonProperty("supplierName")] public string SupplierName { get; set; }
    [JsonProperty("submissionChannel")] public string SubmissionChannel { get; set; }
    [JsonProperty("intermediaryName")] public string IntermediaryName { get; set; }
    [JsonProperty("intermediaryTin")] public string IntermediaryTin { get; set; }
    [JsonProperty("intermediaryRob")] public string IntermediaryRob { get; set; }
    [JsonProperty("buyerName")] public string BuyerName { get; set; }
    [JsonProperty("buyerTIN")] public string BuyerTIN { get; set; }
}

public class Metadata
{
    [JsonProperty("totalPages")] public int TotalPages { get; set; }
    [JsonProperty("totalCount")] public int TotalCount { get; set; }
}

public class DocumentSearchService
{
    private readonly HttpClient _httpClient;

    public DocumentSearchService(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }

    public async Task<DocumentSearchResult> SearchDocumentsAsync(DocumentSearchRequest request)
    {
        ValidateRequest(request);
        var queryParams = BuildQueryParameters(request);
        var response = await _httpClient.GetAsync($"api/v1.0/documents/search?{queryParams}");

        if (response.IsSuccessStatusCode)
        {
            var content = await response.Content.ReadAsStringAsync();
            return JsonConvert.DeserializeObject<DocumentSearchResult>(content);
        }

        throw new HttpRequestException($"API request failed with status code: {response.StatusCode}");
    }

    private string BuildQueryParameters(DocumentSearchRequest request)
    {
        var parameters = new Dictionary<string, string>();

        void AddDateTimeParam(string name, DateTimeOffset? value)
        {
            if (value.HasValue)
                parameters.Add(name, value.Value.ToUniversalTime().ToString("yyyy-MM-ddTHH:mm:ssZ"));
        }

        void AddEnumParam<T>(string name, T? value) where T : struct
        {
            if (value.HasValue)
                parameters.Add(name, value.Value.ToString());
        }

        if (!string.IsNullOrEmpty(request.Uuid)) parameters.Add("uuid", request.Uuid);
        AddDateTimeParam("submissionDateFrom", request.SubmissionDateFrom);
        AddDateTimeParam("submissionDateTo", request.SubmissionDateTo);
        if (request.PageSize.HasValue) parameters.Add("pageSize", request.PageSize.ToString());
        if (request.PageNo.HasValue) parameters.Add("pageNo", request.PageNo.ToString());
        AddDateTimeParam("issueDateFrom", request.IssueDateFrom);
        AddDateTimeParam("issueDateTo", request.IssueDateTo);
        AddEnumParam("invoiceDirection", request.InvoiceDirection);
        AddEnumParam("status", request.Status);
        
        if (request.DocumentType.HasValue)
        {
            var memberInfo = typeof(DocumentType).GetMember(request.DocumentType.ToString())[0];
            var attribute = memberInfo.GetCustomAttributes(typeof(EnumMemberAttribute), false)
                .FirstOrDefault() as EnumMemberAttribute;
            parameters.Add("documentType", attribute?.Value ?? request.DocumentType.ToString());
        }

        if (!string.IsNullOrEmpty(request.SearchQuery)) 
            parameters.Add("searchQuery", Uri.EscapeDataString(request.SearchQuery));

        return string.Join("&", parameters.Select(kvp => $"{kvp.Key}={Uri.EscapeDataString(kvp.Value)}"));
    }

    private void ValidateRequest(DocumentSearchRequest request)
    {
        var submissionDatesProvided = request.SubmissionDateFrom.HasValue && request.SubmissionDateTo.HasValue;
        var issueDatesProvided = request.IssueDateFrom.HasValue && request.IssueDateTo.HasValue;

        if (!submissionDatesProvided && !issueDatesProvided)
            throw new ArgumentException("Either submission dates or issue dates must be provided");

        if (submissionDatesProvided)
        {
            if (request.SubmissionDateFrom >= request.SubmissionDateTo)
                throw new ArgumentException("SubmissionDateFrom must be before SubmissionDateTo");

            if ((request.SubmissionDateTo - request.SubmissionDateFrom).Value.TotalDays > 31)
                throw new ArgumentException("Date range cannot exceed 31 days");
        }

        if (issueDatesProvided)
        {
            if (request.IssueDateFrom >= request.IssueDateTo)
                throw new ArgumentException("IssueDateFrom must be before IssueDateTo");

            if ((request.IssueDateTo - request.IssueDateFrom).Value.TotalDays > 31)
                throw new ArgumentException("Date range cannot exceed 31 days");
        }

        if (request.PageSize is < 1 or > 100)
            throw new ArgumentException("PageSize must be between 1 and 100");
    }
}
```

Key features:
1. Input validation ensures API requirements are met
2. Proper handling of date/time formatting in UTC
3. Enum conversions matching API spec values
4. Pagination support with pageSize/pageNo parameters
5. Error handling for HTTP errors
6. JSON property mapping for API response structure
7. Rate limiting awareness (implementation left to consumer)

Usage example:
```csharp
var httpClient = new HttpClient { BaseAddress = new Uri("https://api.myinvois.gov.my/") };
httpClient.DefaultRequestHeaders.Add("Authorization", "Bearer YOUR_TOKEN");

var service = new DocumentSearchService(httpClient);

var request = new DocumentSearchRequest
{
    SubmissionDateFrom = DateTimeOffset.UtcNow.AddDays(-7),
    SubmissionDateTo = DateTimeOffset.UtcNow,
    PageSize = 50
};

try
{
    var result = await service.SearchDocumentsAsync(request);
    // Process results...
}
catch (Exception ex)
{
    // Handle errors...
}
```

Note: You'll need to:
1. Add proper authentication handling
2. Implement rate limiting (recommended 12 RPM)
3. Add retry logic for throttling (1 request/5 seconds)
4. Handle API-specific error responses
5. Add logging and monitoring
6. Consider implementing cancellation tokens for async operations