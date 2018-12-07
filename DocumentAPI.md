# Document API

```sh
/corporate/candidatesearch/document
```
## Update Document

**Method:** POST
**URL:** /corporate/candidatesearch/document
Optional query string parameters:
  - document_id, if do cument if is not provided email should be provided
  - email, if email is not provided document_id should be provided
  - request_id
  - pool

### Parameter

|Field|Type|Description|
|--- |--- |--- |
|document_id|string|Document identificator. MaxLength: 50|
|email|string|Email that will be used to retrieve documents that belong to that email. Max 128 characters.|
|update_fields|UpdateField[]|Array that contains the fields that will be updated|
|name|string|Name of the field to be updated, example: email|
|value|string|Value of the field provided in the name, example: test@test.com|
|customer_key|string|The customer Key|
|request_id|string|Request Identificator|
|pool|string|Name of the pool that will be used, by default it will use privatecandidates|

### Request Body

```json
 {
  "document_id": "",
  "email": "",
  "pool": "",
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

**Sample request:**
```json
{
  "document_id": "3F1AASDQWAS123123",
  "update_fields": [{
    "name": "skill_list",
    "value": ["Java"]
  }],
  "customer_key": "CSTESTSQUIRRELY00001"
}
```

**Sample response:**

```json
{
  "data": {
    "status_message": "Customer was successfully updated.",
    "status_code": 200
  },
  "timing": {
    "time_received": "2016-12-06T19:41:46.7711185Z",
    "time_elapsed": 14.2441964
  },
  "forensics": {
    "input": {
      "document_id": "3F1AASDQWAS123123",
      "update_fields": [
        {
          "name": "skill_list",
          "value": [
            "Java"
          ]
        }
      ],
      "customer_key": "CSTESTSQUIRRELY00001",
      "request_id": "CSREQ0061N65B5W8X043C5V"
    },
    "internal_forensics": {
      "StatusCodeReturnedFromSearchCore": 0
    }
  }
}
```


## Create Document

**Method:** PUT
**URL:** /corporate/candidatesearch/document
Optional query string parameters:
  - request_id

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
### Request Body
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

**Sample request:**
```json
{
  "document_id": "3F1AASDQWAS123123",
  "email": "test.test@test.com",
  "first_name": "test",
  "last_name": "test",
  "country": "US",
  "resume_description": "Test for Document Creation",
  "pool": "privatecandidates",
  "skill_list": [
    "Java"
  ],
  "customer_key": "CSTESTSQUIRRELY00001"
}
```

**Sample response:**

```json
{
  "data": {
    "status_message": "Applicant was successfully indexed.",
    "status_code": 200
  },
  "timing": {
    "time_received": "2018-12-06T19:47:38.1486562Z",
    "time_elapsed": 9.1482392
  },
  "forensics": {
    "input": {
      "document_id": "3F1AASDQWAS123123",
      "email": "test.test@test.com",
      "first_name": "test",
      "last_name": "test",
      "latitude": 0.0,
      "longitude": 0.0,
      "country": "US",
      "total_years_experience": 0.0,
      "resume_description": "Test for Document Creation",
      "skill_list": [
        "Java"
      ],
      "priority": 0,
      "company_experience_list": [],
      "education_list": [],
      "custom_field_list": [],
      "pool": "privatecandidates",
      "customer_key": "CSTESTSQUIRRELY00001",
      "request_id": "CSREQ006FZ6NT8VC310B5VX"
    },
    "internal_forensics": {
      "StatusCodeReturnedFromSearchCore": 0
    }
  },
  "CompositeID": "CSTESTSQUIRRELY00001|3F1AASDQWAS123123"
}
```

## Retrieve a document

Customer Key and Document Key are required to search for a single document. Parameter type: QUERY

**Method:** GET
**URL:** /corporate/candidatesearch/document?**Parameters**

### Parameter

|Field|Type|Description|
|--- |--- |--- |
|customer_key|string||
|document_id|string||

### Response Body

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

**Sample request:**
```sh
/corporate/candidatesearch/document?customer_key=CSTESTSQUIRRELY00001&document_id=3F1AASDQWAS123123
```

**Sample response:**

```json
{
  "data": {
    "document": {
      "customer_key": "CSTESTSQUIRRELY00001",
      "document_id": "3F1AASDQWAS123123",
      "email": "test.test@test.com",
      "first_name": "test",
      "last_name": "test",
      "address_1": "",
      "city": "",
      "admin_area_1": "",
      "postal_code": "",
      "latitude": 0.00000,
      "longitude": 0.00000,
      "country": "US",
      "phone": "",
      "currently_employed": "",
      "total_years_experience": 0.00000,
      "resume_description": "Test for Document Creation",
      "activity_history": "",
      "skill_list": "Java",
      "education_list": [],
      "company_experience_list": []
    },
    "status_code": 200
  },
  "timing": {
    "time_received": "2018-12-06T19:54:33.6089440Z",
    "time_elapsed": 2.7219459
  },
  "forensics": {
    "input": {
      "radius": 0.0,
      "enable_multi_facet": false,
      "start_page": 0,
      "results_per_page": 10,
      "group_by_candidate": false,
      "group_by_candidate_limit": 1,
      "by_candidate": false,
      "indecisive_facets": false,
      "debug": false,
      "show_reasons": false,
      "sort_by_distance": false,
      "solr_score": false,
      "semantic": false,
      "customer_key": "CSTESTSQUIRRELY00001",
      "request_id": "CSREQ003TW6Z58R03KXL38P"
    },
    "internal_forensics": {
      "StatusCodeReturnedFromSearchCore": 0
    }
  }
}
```

## Delete a Document

Customer Key and Document Key are required. Must set Content-Type = application/json in header. Parameter type: QUERY

**Method:** DELETE
**URL:** /corporate/candidatesearch/document?**Parameters**

### Parameter

|Field|Type|Description|
|--- |--- |--- |
|customer_key|string||
|document_id|string|Note: either provide the document_id or the DIDs|
|DIDs|string|Note: either provide the document_id or the DIDs|

### Response Body

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

**Sample request:**
```sh
/corporate/candidatesearch/document?customer_key=CSTESTSQUIRRELY00001&document_id=3F1AASDQWAS123123
```

**Sample response:**

```json
{
  "data": {
    "status_code": 200
  },
  "timing": {
    "time_received": "2018-12-06T19:59:22.6824285Z",
    "time_elapsed": 5.280952
  },
  "forensics": {
    "input": {
      "document_id": "3F1AASDQWAS123123",
      "customer_key": "CSTESTSQUIRRELY00001",
      "request_id": "CSREQ004C06WC2FF7MY52H8"
    },
    "internal_forensics": {
      "StatusCodeReturnedFromSearchCore": 0
    }
  }
}
```
