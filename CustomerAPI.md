## Customer

/Customer

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
