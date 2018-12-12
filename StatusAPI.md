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
Endpoints: https://api.careerbuilder.com/corporate/CandidateStatus
```
## Retrieve all statuses for a single email
**Method:** GET
**URL:** [https://api.careerbuilder.com/corporate/CandidateStatus/*your account here*/*candidate_email here*](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fwwwtest.api.careerbuilder.com%2F&postURL=corporate%2FCandidateStatus%2FAAAA_1234%2FtestEmail%40123.com%3Fresults_per_page%3D15%26status_date%3D2017-02-18T19%3A04%3A01Z&method=get&contentType=application%2Fjson&acceptType=application%2Fjson&version=default&region=staging&flow=client_credentials&userDid=&accountDid=&headers=&body=)
Optional query string parameters:
  - Results_per_page
    - Defaults to 10
  - Status_date
    - only statuses that have status dates after the given status_date will be returned.

**Sample request:** 
[https://api.careerbuilder.com/corporate/CandidateStatus/AAAA_1234/testEmail@123.com?results_per_page=15&status_date=2017-02-18T19:04:01Z](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fwwwtest.api.careerbuilder.com%2F&postURL=corporate%2FCandidateStatus%2FAAAA_1234%2FtestEmail%40123.com%3Fresults_per_page%3D15%26status_date%3D2017-02-18T19%3A04%3A01Z&method=get&contentType=application%2Fjson&acceptType=application%2Fjson&version=default&region=staging&flow=client_credentials&userDid=&accountDid=&headers=&body=)

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
**URL:** [https://api.careerbuilder.com/corporate/CandidateStatus](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fwwwtest.api.careerbuilder.com%2F&postURL=corporate%2FCandidateStatus&method=put&contentType=application%2Fjson&acceptType=application%2Fjson&version=default&region=staging&flow=client_credentials&userDid=&accountDid=&headers=&body={++%0D%0A+++%22account%22%3A%22AAAA_1234%22%2C%0D%0A+++%22candidate_email%22%3A%22testEmail%40123.com%22%2C%0D%0A+++%22vendor_key%22%3A%22test_vendor_key%22%2C%0D%0A+++%22client%22%3A%22test_client%22%2C%0D%0A+++%22requisition_id%22%3A%22123%22%2C%0D%0A+++%22level_id%22%3A%22test_level_id%22%2C%0D%0A+++%22status%22%3A%22test_status%22%2C%0D%0A+++%22status_date%22%3A%222017-03-19T19%3A04%3A01Z%22%2C%0D%0A+++%22status_level%22%3A%22application%22%2C%0D%0A+++%22status_detail%22%3A%22Test+status+detail%22%0D%0A})

Requirements: 
  - account
  - email
  - status
  - status_date
  - status_level (must be either candidate or application)
    - if candidate: requisition_id and requisition_title must be left off
    - if application: requisition_id is required. Requisition_title is still optional.

**Sample requests:**
```
https://www.api.careerbuilder.com/corporate/CandidateStatus
```
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
**URL:** [https://api.careerbuilder.com/corporate/CandidateStatus](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fwwwtest.api.careerbuilder.com%2F&postURL=corporate%2FCandidateStatus&method=post&contentType=application%2Fjson&acceptType=application%2Fjson&version=default&region=staging&flow=client_credentials&userDid=&accountDid=&headers=&body={++%0D%0A+++%22account%22%3A%22aaaa_1234%22%2C%0D%0A+++%22candidate_email%22%3A[++%0D%0A++++++%22testEmail%40123.com%22%2C%0D%0A++++++%22testEmail%401234.com%22%0D%0A+++]%2C%0D%0A+++%22status_date%22%3A%222017-03-18T19%3A04%3A01Z%22%0D%0A})

Requirements:
  - account
  - either a list of candidate emails or a list of requisition ids.
Optional:
  - status_date
    - only statuses that have status dates after the given status_date will be returned.

**Sample requests:**
```
https://www.api.careerbuilder.com/corporate/CandidateStatus
```
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
