# Candidate Status Documentation

Contents

- [Candidate Status Fields](#candidate-status-fields)
- [Retrieve all statuses for a single email](#retrieve-all-statuses-for-a-single-email)
- [Insert a status](#insert-a-status)
- [Retrieve multiple emails or multiple requisition_ids](#retrieve-multiple-emails-or-multiple-requisition_ids)

## Candidate Status Fields

| Field | Req’d | Type | Comment |
| ------ | ------ | ------ | ------ |
| account | Y | String(20) | CB Account DID |
| candidate_email | Y | String(255) | Candidate Email. |
| vendor_key  | N | String(20) | Key passed in to identify the vendor |
| client | N | String(20) | System calling this service (MyCandidates, CandidateStream) |
| requisition_id | N | String(1024) | Job this status applies to (can be null) |
| requisition_title | N | String(256) |  |
| level_id | N | String(64) | ID of Candidate/Application |
| status | Y | String(100) | The raw status as recorded by source system |
| status_date | Y | Date/time | UTC Date/time of status change format: YYYY-MM-DD THH:MM:SSZ 1776-07-04T19:04:01Z |
| status_level | Y | String(20) | Possible values for Status Level: Application and Candidate, Application requires requisition_id and Candidate does not require requisition_id and requisition_title |
| status_detail | N | String(4096) | Free form string  |

#### Unique Key:
  - account
  - candidate_email
  - vendor_key
  - requisition_id
  - status_level
  
**Bullhorn:** Three possible status entries – candidate, jobsubmission, and placement.  It looks like placement can replace jobsubmission status, so that we could just have one status for a job requisition.
  - Candidate->Status
  - Candidate->JobSubmission(s)->JobOrder and Status
  - Candidate->Placement(s)-> JobOrder and Status
  - Candidate->Placement(s)->JobSubmission->JobOrder and Status

**TEE:** Candidate status and application status entries.
  - Candidate->Step and Status
  - Candidate->Application(s)->Requisition, Step and Status

Each step has a status.  Step is like New, Reviewed, First Interview, Second Interview, Third Interview, Testing, Offer, Hired, Rejected, Declined, Pipeline, Inactive.  Status is like not started, in progress, completed.

**Campaign Mgt.:** Status per campaign.
**TSR:** Candidate status and application status entries (~4 translations per status)
```
Endpoints: /Corporate/CandidateStatus
```
## Retrieve all statuses for a single email
**Method:** GET
**URL:** /Corporate/CandidateStatus/*your account here*/*candidate_email here*
Optional query string parameters:
  - Results_per_page
    - Defaults to 10
  - Status_date
    - only statuses that have status dates after the given status_date will be returned.

**Sample request:** 
```
/Corporate/CandidateStatus/AAAA_1234/testEmail@123.com?results_per_page=15&status_date=2017-02-18T19:04:01Z
```
**Sample response:** 
```json
[  
   {  
      "requisition_id":"1",
      "vendor_key":"test_vendor_key",
      "status_detail":"Test status detail",
      "status_level":"application",
      "status_date":"2017-03-19T23:04:01Z",
      "level_id":"test_level_id",
      "status":"test_status",
      "client":"test_client",
      "requisition_title":"test_req_title",
      "account":"AAAA_1234",
      "candidate_email":"testEmail@123.com"
   },
   {  
      "requisition_id":"2",
      "vendor_key":"test_vendor_key",
      "status_detail":"Test status detail",
      "status_level":"application",
      "status_date":"2017-03-19T23:04:01Z",
      "level_id":"test_level_id",
      "status":"test_status",
      "client":"test_client",
      "requisition_title":"test_req_title",
      "account":"AAAA_1234",
      "candidate_email":"testEmail@123.com"
   },
   {  
      "requisition_id":"3",
      "vendor_key":"test_vendor_key",
      "status_detail":"Test status detail",
      "status_level":"application",
      "status_date":"2017-03-19T23:04:01Z",
      "level_id":"test_level_id",
      "status":"test_status",
      "client":"test_client",
      "requisition_title":"test_req_title",
      "account":"AAAA_1234",
      "candidate_email":"testEmail@123.com"
   }
]
```

## Insert a status

**Method:** PUT
**URL:** /Corporate/CandidateStatus

Requirements: 
  - account
  - email
  - status
  - status_date
  - status_level (must be either candidate or application)
    - if candidate: requisition_id and requisition_title must be left off
    - if application: requisition_id is required. Requisition_title is still optional.

**Sample requests:**
Sample request 1 (application):
```json
{  
   "account":"AAAA_1234",
   "candidate_email":"testEmail@123.com",
   "vendor_key":"test_vendor_key",
   "client":"test_client",
   "requisition_id":"123",
   "level_id":"test_level_id",
   "status":"test_status",
   "status_date":"2017-03-19T19:04:01Z",
   "status_level":"application",
   "status_detail":"Test status detail"
}
```

Sample request 2 (candidate):
```json
{  
   "account":"AAAA_1234",
   "candidate_email":"testEmail@123.com",
   "vendor_key":"test_vendor_key",
   "client":"test_client",
   "requisition_id":"123",
   "level_id":"test_level_id",
   "status":"test_status",
   "status_date":"2017-03-19T19:04:01Z",
   "status_level":"application",
   "status_detail":"Test status detail"
}
```

**Sample response:** (Note: I will most likely be expanding this response but it has not been a priority)
```
"Created/updated status."
```

## Retrieve multiple emails or multiple requisition_ids

**Method:** POST
**URL:** /Corporate/CandidateStatus

Requirements:
  - account
  - either a list of candidate emails or a list of requisition ids.
Optional:
  - status_date
    - only statuses that have status dates after the given status_date will be returned.

**Sample requests:**
Sample request 1:
```json
{  
   "account":"aaaa_1234",
   "candidate_email":[  
      "testEmail@123.com",
      "testEmail@1234.com"
   ],
   "status_date":"2017-03-18T19:04:01Z"
}
```

Sample request 2:
```json
{  
   "account":"aaaa_1234",
   "requisition_id":[  
      "1",
      "2",
      "123"
   ],
   "status_date":"2017-03-18T19:04:01Z"
}
```

**Sample response:**

```json
[  
   {  
      "requisition_id":"1",
      "vendor_key":"test_vendor_key",
      "status_detail":"Test status detail",
      "status_level":"application",
      "status_date":"2017-03-19T23:04:01Z",
      "level_id":"test_level_id",
      "status":"test_status",
      "client":"test_client",
      "requisition_title":"test_req_title",
      "account":"AAAA_1234",
      "candidate_email":"testEmail@123.com"
   },
   {  
      "requisition_id":"2",
      "vendor_key":"test_vendor_key",
      "status_detail":"Test status detail",
      "status_level":"application",
      "status_date":"2017-03-19T23:04:01Z",
      "level_id":"test_level_id",
      "status":"test_status",
      "client":"test_client",
      "requisition_title":"test_req_title",
      "account":"AAAA_1234",
      "candidate_email":"testEmail@123.com"
   },
   {  
      "requisition_id":"123",
      "vendor_key":"test_vendor_key",
      "status_detail":"Test status detail",
      "status_level":"application",
      "status_date":"2017-03-19T23:04:01Z",
      "level_id":"test_level_id",
      "status":"test_status",
      "client":"test_client",
      "account":"AAAA_1234",
      "candidate_email":"testEmail@123.com"
   }
]
```
