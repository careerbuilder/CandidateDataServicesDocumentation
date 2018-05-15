## Customer

corporate/candidatesearch/customer

### GET

Returns the Custom Fields defined for a customer as well as some limited customer information.

|Field|Type|Description|
|--- |--- |--- |
|customer_key|string|the key with which the customer was created|

[corporate/candidatesearch/customer?customer_key=CKHT82M6S1Y6DRRBSQLY](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fwwwtest.api.careerbuilder.com%2F&postURL=corporate%2Fcandidatesearch%2Fcustomer%3Fcustomer_key%3DCKHT82M6S1Y6DRRBSQLY+&method=get&contentType=application%2Fjson&acceptType=application%2Fjson&version=default&region=staging&flow=client_credentials&userDid=&accountDid=&headers=&body=)

```json
{
  "data": {
    "customer": {
      "customer_key": "CKHT82M6S1Y6DRRBSQLY",
      "customer_name": "CKHT82M6S1Y6DRRBSQLY",
      "remaining_facetable_fields": 9,
      "languages_supported": [
        "English"
      ],
      "custom_fields": [
        {
          "field_name": "field_name",
          "field_type": "string",
          "is_searchable": true,
          "is_facetable": true,
          "max_field_size_in_characters": 128
        }
      ]
    },
    "status_code": 200
  },
  "timing": {
    "time_received": "2018-05-15T15:56:17.3164000Z",
    "time_elapsed": 0.026
  },
  "forensics": {
    "input": {
      "customer_key": "CKHT82M6S1Y6DRRBSQLY ",
      "request_id": "3a89a626-685e-4bc7-b9a8-248415400d92"
    },
    "internal_forensics": {
      "StatusCodeReturnedFromSearchCore": 0
    }
  }
}
```

### POST

At a minimum this will require a customer name. It will generate a customer. Must set Content-Type = application/json in header. Parameter type: BODY

#### Parameter

|Field|Type|Description|
|--- |--- |--- |
|customer_name|string||
|base_customer_key optional|string||
|languages_supported optional|string[]||
|custom_fields optional|CustomerCreationCustomFieldRequest[]||
|field_name|string||
|field_type|string||
|is_searchable optional|boolean||
|is_facetable optional|boolean||
|max_field_size_in_characters optional|int||
|customer_key|string||
|request_id optional|string||

```json
{
  "customer_name": "",
  "base_customer_key": "",
  "languages_supported": [
  ""
  ],
  "custom_fields": [
    {
      "field_name": "",
      "field_type": "",
      "is_searchable": false,
      "is_facetable": false,
      "max_field_size_in_characters": 0
    }
  ],
  "customer_key": "",
  "request_id": ""
}
```

#### Response

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

### PUT

Customer Key is required. Must set Content-Type = application/json in header. Parameter type: BODY

#### Parameter

|Field|Type|Description|
|--- |--- |--- |
|field_name|string||
|field_type|string||
|is_searchable|boolean||
|is_facetable|boolean||
|max_field_size_in_characters optional|int||
|customer_key|string||
|request_id optional|string||

```json
{
    "field_name": "",
    "field_type": "",
    "is_searchable": false,
    "is_facetable": false,
    "max_field_size_in_characters": 0,
    "customer_key": ""
}
```

#### Response

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


### DELETE 
Customer Key is required. Must set Content-Type = application/json in header. Parameter type: QUERY

#### Parameter

|Field|Type|Description|
|--- |--- |--- |
|customer_key|string||

#### Response

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
