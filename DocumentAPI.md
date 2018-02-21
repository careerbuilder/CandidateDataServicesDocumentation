# Document API

/Document

## POST

Handles update Request. Must set Content-Type = application/json in header. Parameter type: BODY

### Parameter

|Field|Type|Description|
|--- |--- |--- |
|document_id|string||
|update_fields|UpdateField[]||
|name|string||
|value|string||
|customer_key|string||
|request_id optional|string||

```json
 {
  "document_id": "",
  "update_fields": [
    {
      "name": "",
      "value": ""
    }
  ],
  "customer_key": "",
  "request_id": ""
}
```

### Response

Success 200

|Field|Type|Description|
|--- |--- |--- |
|data|DataResponse||
|status_message|string||
|status_code|string||
|timing|TimingResponse||
|time_received|string||
|time_elapsed|int||
|errors|ErrorResponse[]||
|type|string||
|message|string||
|code|string||
|forensics|ForensicsResponse||
|audit_log_id|string||
|input|Request||
|customer_key|string||
|request_id|string||
|internal_forensics|ForensicsInternalResponse||
|SolrURL|string||
|SearchRestfulQuery|string||
|SearchException|string||
|SearchResultDIDs|string[]||
|ErrorReturnedFromSolrSearch|string[]||

## PUT

Handles Create Request. Must set Content-Type = application/json in header. Parameter type: BODY. Custom fields can be passed like standard fields. Please look at example.

### Parameters
|Field|Type|Description|
|--- |--- |--- |
|document_id|string|External ATS's identifier for applicant. MaxLength: 50|
|email|string|MaxLength: 128|
|first_name optional|string|MaxLength: 64|
|last_name optional|string|MaxLength: 64|
|address_1 optional|string|MaxLength: 128|
|city optional|string|MaxLength: 64|
|admin_area_1 optional|string|Placeholder for State/StateCode or the countries equivalent type. MaxLength: 75|
|postal_code optional|string|MaxLength: 30|
|latitude optional|decimal|MaxLength: 10|
|longitude optional|decimal|MaxLength: 10|
|country|string|MaxLength: 32|
|phone optional|string|MaxLength: 32|
|currently_employed optional|string|MaxLength: 5|
|total_years_experience optional|string|MaxLength: 6|
|resume_description|string||
|activity_history optional|string|MaxLength: 1024|
|skill_list optional|string[]|MaxLength: 4096|
|priority optional|string|'Low' or 'Normal' or 'High'|
|company_experience_list optional|DocumentCreationCompanyExperienceRequest[]||
|company_name optional|string|MaxLength: 128|
|job_title optional|string|MaxLength: 128|
|is_current_position optional|boolean||
|normailed_did optional|string|MaxLength: 20|
|normalized_company_name optional|string|MaxLength: 128|
|company_size optional|int||
|company_naics optional|string|MaxLength: 1024|
|start_date|string||
|end_date|string||
|onet_17 optional|string|MaxLength: 1024|
|description optional|string||
|carotene_v2 optional|string|MaxLength: 1024|
|education_list optional|DocumentCreationEducationRequest[]||
|school_name optional|string|MaxLength: 512|
|normalized_school_did optional|string|MaxLength: 20|
|normalized_school_name optional|string|MaxLength: 128|
|major_name optional|string|MaxLength: 512|
|normalized_major_did optional|string|MaxLength: 20|
|normalized_major_name optional|string|MaxLength: 512|
|degree_code optional|string|MaxLength: 24|
|degree_name optional|string|MaxLength: 128|
|graduation_date|string||
|customer_key|string||
|request_id optional|string||

```json
{
  "document_id": "",
  "email": "",
  "first_name": "",
  "last_name": "",
  "address_1": "",
  "city": "",
  "admin_area_1": "",
  "postal_code": "",
  "latitude": 0,
  "longitude": 0,
  "country": "",
  "phone": "",
  "currently_employed": "",
  "total_years_experience": 0,
  "resume_description": "",
  "activity_history": "",
  "skill_list": [
    ""
  ],
  "priority": "",
  "company_experience_list": [
    {
      "company_name": "",
      "job_title": "",
      "is_current_position": false,
      "normalized_did": "",
      "normalized_company_name": "",
      "company_size": 0,
      "company_naics": "",
      "start_date": "",
      "end_date": "",
      "onet_17": "",
      "description": "",
      "carotene_v2": ""
    }
  ],
  "education_list": [
    {
      "school_name": "",
      "normalized_school_did": "",
      "normalized_school_name": "",
      "major_name": "",
      "normalized_major_did": "",
      "normalized_major_name": "",
      "degree_code": "",
      "degree_name": "",
      "graduation_date": ""
    }
  ],
  "custom_field_1": "7",
  "custom_field_2": "blue",
  "customer_key": "",
  "request_id": ""
}
```

### Response

Success 200

|Field|Type|Description|
|--- |--- |--- |
|string||DID|
|data|DataResponse||
|status_message|string||
|status_code|string||
|timing|TimingResponse||
|time_received|string||
|time_elapsed|int||
|errors|ErrorResponse[]||
|type|string||
|message|string||
|code|string||
|forensics|ForensicsResponse||
|audit_log_id|string||
|input|Request||
|customer_key|string||
|request_id|string||
|internal_forensics|ForensicsInternalResponse||
|SolrURL|string||
|SearchRestfulQuery|string||
|SearchException|string||
|SearchResultDIDs|string[]||
|ErrorReturnedFromSolrSearch|string[]||


## GET

Customer Key and Document Key are required to search for a single document. Parameter type: QUERY

### Parameter

|Field|Type|Description|
|--- |--- |--- |
|customer_key|string||
|document_id|string||

### Response

Success 200

|Field|Type|Description|
|--- |--- |--- |
|data|DataResponse||
|status_message|string||
|status_code|string||
|timing|TimingResponse||
|time_received|string||
|time_elapsed|int||
|errors|ErrorResponse[]||
|type|string||
|message|string||
|code|string||
|forensics|ForensicsResponse||
|audit_log_id|string||
|input|Request||
|customer_key|string||
|request_id|string||
|internal_forensics|ForensicsInternalResponse||
|SolrURL|string||
|SearchRestfulQuery|string||
|SearchException|string||
|SearchResultDIDs|string[]||
|ErrorReturnedFromSolrSearch|string[]||

## DELETE

Customer Key and Document Key are required. Must set Content-Type = application/json in header. Parameter type: QUERY

### Parameter

|Field|Type|Description|
|--- |--- |--- |
|customer_key|string||
|document_id|string|Note: either provide the document_id or the DIDs|
|DIDs|string|Note: either provide the document_id or the DIDs|

### Response

Success 200

|Field|Type|Description|
|--- |--- |--- |
|data|DataResponse||
|status_message|string||
|status_code|string||
|timing|TimingResponse||
|time_received|string||
|time_elapsed|int||
|errors|ErrorResponse[]||
|type|string||
|message|string||
|code|string||
|forensics|ForensicsResponse||
|audit_log_id|string||
|input|Request||
|customer_key|string||
|request_id|string||
|internal_forensics|ForensicsInternalResponse||
|SolrURL|string||
|SearchRestfulQuery|string||
|SearchException|string||
|SearchResultDIDs|string[]||
|ErrorReturnedFromSolrSearch|string[]||
