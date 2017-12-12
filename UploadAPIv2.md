# Candidate Upload APIv2

Contents

- [Overview](#overview)
- [Usage](#usage)
- [Parameters](#parameters)
- [Request](#request)
- [Responses](#responses)

## Overview

The Upload APIv2 allows the caller to upload a candidate, their resume, and additional candidate information to Careerbuilder's MyCandidates application. This API operates as a wrapper for the legacy Upload APIv1 endpoint. Through the legacy endpoint the candidate is parsed, enriched, and indexed into MyCandidates for use in various products throughout the CareerBuilder ecosystem.

This endpoint is synchronous and provides immediate feadback if there are issues in the request. If the resume fails parsing the API creates a Contact Card of the most important information in your request.

A Contact Card is a lean version of a candidate profile and is made up of the candidate's email and base64_resume; also, name and plain_text_resume if available. A contact card will not be parsed and the resume is provided for hisotrical record and retrival only. The contact card can be made into a full profile easily by fixing the errors in your original request and re-uploading via the API.

Only the most recent record for a candidate per vendor, per client is indexed. See the [myCandidates Overview](readme.md) for more information.

The candidate can also be [searched for](SearchAPI.md), [downloaded](CandidateAPI.md), and additional candidate fields edited through the Tags, Attributes, JobReqID, and Contact Date APIs.

## Usage

- Client Credentials flow must be used for authentication.
- Authorized bearer token must be included in the header as per CareerBuilder's OAuth documentation.
- Resumes will be parsed as English by default unless a country code parameter is provided. If the resume is in another language, it is recommended to provide a location.
- A valid email address is required for indexing a candidate. It is suggested to provide an email address in case one cannot be parsed from the resume .
- HTML resumes are accepted, but it is recommended that they are uploaded as base64.

```
HTTP POST
Accept: application/json;version=2.0 
https://api.careerbuilder.com/corporate/mysupply/upload
```

## Parameters

 Parameters marked with (Required) must be included in the request. Fields not marked as (Required) are optional fields.

NOTE: With the exception of geography data, all information added in optional fields are prioritized over parsed resume information.

|**Parameter**|**Description**
| --- | --- |
|file_name|Name of the file to be uploaded via the upload API, if none given, we will create one from the candidate's name.
|plain_text_resume|Text representation of resume. The text can be formatted or contain HTML. Must be greater than 100 characters in length.
|base64_resume|(Required) Candidate’s resume as base64. (e.g., PDF or Word Doc). Acceptable file types are: .docx, .rtf, .doc, .txt, .wps, .odt, .wpd, .docm, and .pdf.
|candidate_name|Candidate’s full name. 64 characters max.
|email|Candidate email. This email overrides any parsed information. Must be a valid email address 'example@domain.tld'. Max 128 characters.
|phone_number|Phone number of the candidate
|gender|Candidate’s gender. Possible values: M, F, or U.
|relocation| (Boolean) Will the candidate relocate?
|postal_code|Postal code of the candidate. Accepts international values.
|locality|Candidate’s current city. Max 64 characters.
|admin_area| First level civil entity below country level. For the US this is the state. Short names should follow ISO 3166-2.
|country_code|2 letter country code. ISO 3166-1 alpha-2. https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2
|race|Candidate’s race. Possible Values: AmericanIndian, Asian, AfricanAmerican, Hispanic, NativeHawaiian, or White. The following codes are also valid: ETHWH,ETHAA,ETHNS,ETHHS,ETHAS,ETHOT,ETHNA,ETHHP
|currently_employed| (Boolean) Are they currently employed? Values: Yes or No.
|last_contact_date|Last date the candidate was contacted. Date must be formatted using ISO YYYY-MM-DD.
|months_experience|Candidate’s total months of experience as a string.
|most_recent_job_title| Name of the candidate’s most recent job title. 128 characters max.
|military_experience|Candidate’s military experience. Possible values: None, Veteran, ActiveDuty, ActiveReserve, or InActiveReserve.
|us_security_clearance|Does the candidate have a security clearance? .
|last_contact_date|The last date the candidate was active. yyyy-MM-dd format suggested.
|languages|Json string list of Languages that the candidate knows provided in ISO 639-1 two letter language codes. Possible values: en(English), zh(Chinese), cs(Czech), da(Danish), nl(Dutch), et(Estonian), fi(Finnish), hi(Hindi), fr(French), de(German), el(Greek), he(Hebrew), hu(Hungarian), is(Icelandic), it(Italian), ja(Japanese), ko(Korean), lv(Latvian), lt(Lithuanian), no(Norwegian), pl(Polish), pt(Portuguese), ro(Romanian), ru(Russian), es(Spanish), or sv(Swedish).
|vendor_key|(Required) Key passed in to identify the ATS/vendor. Prefixed with “VK”
|candidate_url|URL linking back to the original source’s version of the candidate. max 1024 characters.
|should_export_to_tn| (Boolean) Should this candidate be added to the client’s Talent Network? The Client must have an active TN and the Vendor must be configured for export to TN. (should be able to tell from a config table, martin does not like)
|talent_network_did|Used in conjunction with "should_export_to_tn". This allows the user to specify what TalentNetworkDID they want this candidate sent to. If not specified, we look up the TalentNetworkDID based on the AccountDID.
|talent_network_desired_job_title|Used in conjunction with "should_export_to_tn". This allows the user to specify the candidate's desired job title. If not specified, this will default to the most recent job title in the candidate's resume.
|talent_network_join_path|Used in conjunction with "should_export_to_tn". This allows the user to specify what Join Path they want this candidate associated to in Talent Network. If not specified, this will default to "MySupply".
|job_req_ids|Json string list containing any job requisitions this candidate should be associated to.
|activity_history|A list of Activity History Objects containing a date, in yyyy/MM format, indicating when a candidate was active. An active candidate is one that has applied to a job, changed a resume or done any activity that would be viewed as being active in the job market. [This field is used when selecting the document exposed to users.](readme.md)
|tags|Pipe delimited string containing any custom values that want to be associated to the candidate. [See the Tag API for more information on Tags](TagAPI.md)
|vendor_dynamic_field|A list of CustomField objects to store with this candidate. Should this field be provided a full vendor_dynamic_field object is required. [See Custom Fields](CustomFields.md).
|client_dynamic_field|A list of Attribute objects to store with this candidate. Should this field be provided a full client_dynamic_field object is required.[See Attributes](Attributes.md).

## Request

````javascript
{
  "file_name" : "string",
  "plain_text_resume" : "string",
  "base64_resume" : "string"
  "candidate_name" : "string",
  "email" : "string",
  "phone_number" : "string",
  "gender" : "string",
  "relocation" : boolean,
  "postal_code" : "string",
  "locality" : "string",
  "admin_area" : "String",
  "country_code" : "string",
  "currently_employed" : boolean,
  "last_contact_date" : "YYYY-MM-DD",
  "months_experience" : "string",
  "most_recent_job_title" : "string"
  "military_experience" : "string",
  "us_security_clearance" : boolean
  "laguages" :[
    "en",
    "fr"
  ],
  "vendor_key" : "string",
  "candidate_url" : "string"
  "should_export_to_tn" : "",
  "job_req_ids" : [
      "string"
  ], 
  "activity_history" : [
      "YYYY-MM-DD"
  ],
  "tags" : [
    "string"
  ],
  "vendor_fields" : [
    {
      "name" : "string",
      "label" : "string",
      "value" : "string",
      "searchable" : boolean,
      "facetable" : boolean
    }
  ],
  "client_fields" : [
    {
      "type" : "string",
      "value" : "string"
    }
  ]
}    
````

## Responses

### Response Codes
|**Code**|**Description**|
| --- | --- |
|202|Candidate was queued for upload. Or Contact Card Created.
|500|Something went wrong on our end.
|400|Issue with the request. A required field may be missing or the request may be malformed.
|401|Unauthorized. Authentication credentials provided are invalid.

### Success Response

|**Node**|**Details**|
| --- | --- |
|status|More informatative string representation of the status e.x. SUCCESSFUL_QUEUE, FAILED_LEGACY
|message|A message containing the status of the candidate. Present for errors and successes.
|removed_elements|Array of RemovedElemet objects and information containing why the elements were removed.
|failed_to_parse_message| This message only returns in the instance of Contact Card creation. It returns the errors returned from parsing. 

````javascript
{
  "data": {
    "status": "SUCCESSFUL_QUEUE",
    "message": "Candidate was successfully queued for upload.",
    "removed_elements": [
      "EducationDegreeLevel was removed for: The 'EducationDegreeLevel' element is invalid - The value '' is invalid according to its datatype 'String' - The Pattern constraint failed.",
      "CurrentlyEmployed was removed for: The 'CurrentlyEmployed' element is invalid - The value '' is invalid according to its datatype 'String' - The Pattern constraint failed.",
      "ManagementExperience was removed for: The 'ManagementExperience' element is invalid - The value '' is invalid according to its datatype 'String' - The Pattern constraint failed.",
      "WillingToRelocate was removed for: The 'WillingToRelocate' element is invalid - The value '' is invalid according to its datatype 'String' - The Pattern constraint failed.",
      "MostRecentEmployeeType was removed for: The 'MostRecentEmployeeType' element is invalid - The value '' is invalid according to its datatype 'String' - The Pattern constraint failed.",
      "MilitaryExperience was removed for: The 'MilitaryExperience' element is invalid - The value '' is invalid according to its datatype 'String' - The Pattern constraint failed.",
      "USSecurityClearance was removed for: The 'USSecurityClearance' element is invalid - The value '' is invalid according to its datatype 'String' - The Pattern constraint failed.",
      "Gender was removed for: The 'Gender' element is invalid - The value '' is invalid according to its datatype 'String' - The Pattern constraint failed.",
      "Race was removed for: The 'Race' element is invalid - The value '' is invalid according to its datatype 'String' - The Pattern constraint failed.",
      "Age was removed for: The 'Age' element is invalid - The value '' is invalid according to its datatype 'Byte' - The string '' is not a valid SByte value.",
      "StateCode was removed for: The 'StateCode' element is invalid - The value '' is invalid according to its datatype 'String' - The Pattern constraint failed.",
      "Priority was removed for: The 'Priority' element is invalid - The value '' is invalid according to its datatype 'Byte' - The string '' is not a valid SByte value."
    ]
  }
}
````

### Error Response

|**Node**|**Details**|
| --- | --- |
|status|String representation of the status e.x. SUCCESSFUL_QUEUE, FAILED_LEGACY
|message|A message containing the status of the candidate. Present for errors and successes.
|errors|List of Error nodes specifying any error that occurred while trying to process your request.

````javascript
{
  "status" : "FAILED_LEGACY",
  "message" : "Failed to upload Candidate due to errors in legacy endpoint."
  "errors" : [
    {
      "error" : "POST Content was empty."
    }
  ]
}
````
