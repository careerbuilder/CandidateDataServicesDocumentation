# Candidate Upload API

Contents

- [Overview](#overview)
- [Usage](#usage)
- [Parameters](#parameters)
- [Request](#request)
- [Responses](#responses)

## Overview

*This API is owned and maintained by CareerBuilder's Candidate Data Services team. Please contact this team for any questions or feedback regarding this API.*

CareerBuilder's Candidate Upload API allows the caller to upload a candidate, their resume, and additional candidate information to Careerbuilder's MyCandidates application. The candidate is parsed, enriched, and indexed into the MyCandidates data repository.

This endpoint is synchronous and provides immediate feedback if there are issues in the request. If the resume fails to parse properly the API creates a "contact card" of the most important information in your request.

A Contact Card is a lean version of a candidate profile and is made up of the candidate's email and base64-encoded resume, along with name and plain-text resume if available. A contact card will not be parsed, and the resume text is stored for retrieval purposes only. A candidate record stored as a contact card can be converted into a full profile easily by fixing the errors in the original request and re-uploading via the API.

Only the most recent record for a candidate per vendor-client integration is indexed.

The candidate can also be searched for via the Search API or downloaded via the Candidate API. Additional candidate fields may be edited via the Tags, Attributes, JobReqID, and Contact Date APIs.

## Usage

- As with all CareerBuilder APIs, the OAuth 2.0 Client Credentials flow must be used for authentication. More information can be found [here.](/OAuth.md)
- Authorized bearer token must be included in the header as per CareerBuilder's OAuth documentation.
- Resumes will be parsed as English by default unless a country code parameter is provided. If the resume is in another language, it is recommended to provide a location.
- A valid email address is required for indexing a candidate. It is suggested to provide an email address in case one cannot be parsed from the resume.
- HTML resumes are accepted, but should be base64-encoded.

```
HTTP POST
Accept: application/json;version=2.0 
https://api.careerbuilder.com/corporate/mysupply/upload
```

## Parameters

 Parameters marked with (Required) must be included in the request. All other fields are optional.

NOTE: With the exception of geography data, all information added in optional fields are prioritized over parsed resume information.

|**Parameter**|**Description**
| --- | --- |
|file_name|Name of the file to be uploaded via the upload API; if none is given, we will create one from the candidate's name.
|plain_text_resume|Text representation of resume. The text can be formatted or contain HTML. Must be greater than 100 characters in length.
|base64_resume|(Required) Candidate’s resume file, encoded as a Base64 string. Acceptable file types are: .docx, .rtf, .doc, .txt, .wps, .odt, .wpd, .docm, and .pdf.
|candidate_name|Candidate’s full name. 64 characters max.
|email|Candidate email. This email overrides any parsed information. Must be a valid email address 'example@domain.tld'. Max 128 characters.
|phone_number|Phone number of the candidate
|gender|Candidate’s gender. Possible values: M, F, or U.
|relocation| (Boolean) Indicates whether the candidate is willing to relocate.
|postal_code|Postal code of the candidate. Accepts international values.
|locality|Candidate’s current city. Max 64 characters.
|admin_area| First level civil entity below country level. For the US this is the state. Short names should follow ISO 3166-2.
|country_code|2 letter country code. ISO 3166-1 alpha-2. https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2
|race|Candidate’s race. Possible Values: AmericanIndian, Asian, AfricanAmerican, Hispanic, NativeHawaiian, or White. The following codes are also valid: ETHWH,ETHAA,ETHNS,ETHHS,ETHAS,ETHOT,ETHNA,ETHHP
|currently_employed| (Boolean) Indicates whether the candidate is currently employed. Possible values are "Yes" or "No".
|last_contact_date|Last date the candidate was contacted. Date must be formatted as `yyyy-MM-dd`.
|months_experience|Candidate’s total months of experience as a string.
|most_recent_job_title| Name of the candidate’s most recent job title. 128 characters max.
|military_experience|Candidate’s military experience. Possible values: None, Veteran, ActiveDuty, ActiveReserve, or InActiveReserve.
|us_security_clearance|Indicates whether the candidate has a US security clearance.
|last_contact_date|The last date the candidate was active. Date must be formatted as `yyyy-MM-dd`.
|languages| Languages in which the candidate is fluent. Provided in ISO 639-1 two letter language codes as an array of strings. Possible values: en(English), zh(Chinese), cs(Czech), da(Danish), nl(Dutch), et(Estonian), fi(Finnish), hi(Hindi), fr(French), de(German), el(Greek), he(Hebrew), hu(Hungarian), is(Icelandic), it(Italian), ja(Japanese), ko(Korean), lv(Latvian), lt(Lithuanian), no(Norwegian), pl(Polish), pt(Portuguese), ro(Romanian), ru(Russian), es(Spanish), or sv(Swedish).
|vendor_key|(Required) Key passed in to identify the ATS/vendor. Prefixed with “VK”
|candidate_url|URL linking back to the original source’s version of the candidate. max 1024 characters.
|should_export_to_tn| (Boolean) Should this candidate be added to the client’s Talent Network? The client must have an active TN and the vendor must be configured for export to TN.
|talent_network_did|Used in conjunction with "should_export_to_tn". This allows the user to specify what TalentNetworkDID they want this candidate sent to. If not specified, we look up the TalentNetworkDID based on the AccountDID.
|talent_network_desired_job_title|Used in conjunction with "should_export_to_tn". This allows the user to specify the candidate's desired job title. If not specified, this will default to the most recent job title in the candidate's resume.
|talent_network_join_path|Used in conjunction with "should_export_to_tn". This allows the user to specify what Join Path they want this candidate associated to in Talent Network. If not specified, this will default to "MySupply".
|job_req_ids|A list of strings containing any job requisitions with which this candidate should be associated.
|activity_history|A list of activity history objects containing a date, in yyyy/MM format, indicating when a candidate was active. An active candidate is one that has applied to a job, changed a resume or done any activity that would be viewed as being active in the job market. This field is used when selecting the document exposed to users.
|tags|Pipe delimited string containing any custom values that should be associated to the candidate. [See the Tag API for more information on Tags](TagAPI.md)
|vendor_dynamic_field|A list of CustomField objects to store with this candidate. Must not be an empty list. [See Custom Fields](CustomFields.md).
|client_dynamic_field|A list of Attribute objects to store with this candidate. Must not be an empty list. [See Attributes](Attributes.md).

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
|202|Candidate was queued for upload, or a contact card was created.
|400|Issue with the request. A required field may be missing or the request may be malformed.
|401|Unauthorized. Authentication credentials provided are invalid.
|500|An error occurred within the service.

### Success Response

|**Node**|**Details**|
| --- | --- |
|status|More informative string representation of the status e.x. SUCCESSFUL_QUEUE, FAILED_LEGACY
|message|A message containing the status of the candidate. Present for errors and successes.
|removed_elements|Array of strings where each element in the array describes an element that was removed, along with the reason for removal.
|failed_to_parse_message|Contains the error message returned for a failed resume parse.

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
  "message" : "Failed to upload candidate due to errors in legacy endpoint."
  "errors" : [
    {
      "error" : "POST content was empty."
    }
  ]
}
````
