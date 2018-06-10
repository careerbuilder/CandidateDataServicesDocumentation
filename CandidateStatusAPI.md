
#Overview
The candidate status API provides a key value store for storing ATS status values for a candidate.

#Schema

<table>
<thead>
<tr class="header">
<th><strong>Field</strong></th>
<th><strong>Req’d</strong></th>
<th><strong>Type</strong></th>
<th><strong>Comment</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>account</td>
<td>Y</td>
<td>String(20)</td>
<td>CB Account DID</td>
</tr>
<tr class="even">
<td>candidate_email</td>
<td>Y</td>
<td>String(255)</td>
<td>255 is not a typo.</td>
</tr>
<tr class="odd">
<td>vendor_key</td>
<td>N</td>
<td>String(20)</td>
<td>Type of source system(Bullhorn, Luceo, TEE…)</td>
</tr>
<tr class="even">
<td>client</td>
<td>N</td>
<td>String(20)</td>
<td>System calling this service<br />
(MyCandidates, CandidateStream)</td>
</tr>
<tr class="odd">
<td>requisition_id</td>
<td>N</td>
<td>String(1024)</td>
<td>Job this status applies to (can be null)</td>
</tr>
<tr class="even">
<td>requisition_title</td>
<td>N</td>
<td>String(256)</td>
<td></td>
</tr>
<tr class="odd">
<td>level_id</td>
<td>N</td>
<td>String(64)</td>
<td>ID of Candidate/Application</td>
</tr>
<tr class="even">
<td>status</td>
<td>Y</td>
<td>String(100)</td>
<td>The raw status as recorded by source system</td>
</tr>
<tr class="odd">
<td>status_date</td>
<td>Y</td>
<td>Date/time</td>
<td><p>UTC Date/time of status change</p>
<p>Format: YYYY-MM-DDTHH:MM:SSZ</p>
<p>1776-07-04T19:04:01Z</p></td>
</tr>
<tr class="even">
<td>status_level</td>
<td>Y</td>
<td>String(20)</td>
<td>Candidate/Application</td>
</tr>
<tr class="odd">
<td>status_detail</td>
<td>N</td>
<td>String(4096)</td>
<td>free form string</td>
</tr>
</tbody>
</table>

#Usage

####Unique Key

  - account
  - candidate_email
  - vendor_key
  - requisition_id
  - status_level

This assumes that we are:

  - Combining step and status for TEE
  - Only storing 1 translation (TSR has translations for each status
    value)

####Bullhorn

Three possible status entries –
candidate, jobsubmission, and placement.  It looks like placement can
replace jobsubmission status, so that we could just have one status for
a job requisition.

  - Candidate->Status
  - Candidate->JobSubmission(s)->JobOrder and Status
  - Candidate->Placement(s)-> JobOrder and Status
  - Candidate->Placement(s)->JobSubmission->JobOrder and Status

####TEE

Candidate status and application
status entries.

  - Candidate->Step and Status
  - Candidate->Application(s)->Requisition, Step and Status

Each step has a status.  Step is like New, Reviewed, First Interview,
Second Interview, Third Interview, Testing, Offer, Hired, Rejected,
Declined, Pipeline, Inactive.  Status is like not started, in progress,
completed.

####Campaign Mgt.
Status per campaign

####TSR
Candidate status and application status entries (~4 translations per status)

#Resource URI
/Corporate/CandidateStatus

#Actions
##GET

Retrieve all statuses for a single email.

GET /Corporate/CandidateStatus/{account}/{candidate_email}

Optional query string parameters:
  - Results_per_page (Defaults to 10)
  - Status_date (only statuses that have  dates after the value provided are returned.)

GET /Corporate/CandidateStatus/AAAA_1234/testEmail@123.com?results_per_page=15&status_date=2017-02-18T19:04:01Z

Sample result:
```json
[{
    "requisition_id": "1",
    "vendor_key": "test_vendor_key",
    "status_detail": "Test status detail",
    "status_level": "application",
    "status_date": "2017-03-19T23:04:01Z",
    "level_id": "test_level_id",
    "status": "test_status",
    "client": "test_client",
    "requisition_title": "test_req_title",
    "account": "AAAA_1234",
    "candidate_email": "testEmail@123.com"
},
{
    "requisition_id": "2",
    "vendor_key": "test_vendor_key",
    "status_detail": "Test status detail",
    "status_level": "application",
    "status_date": "2017-03-19T23:04:01Z",
    "level_id": "test_level_id",
    "status": "test_status",
    "client": "test_client",
    "requisition_title": "test_req_title",
    "account": "AAAA_1234",
    "candidate_email": "testEmail@123.com"
},
{
    "requisition_id": "3",
    "vendor_key": "test_vendor_key",
    "status_detail": "Test status detail",
    "status_level": "application",
    "status_date": "2017-03-19T23:04:01Z",
    "level_id": "test_level_id",
    "status": "test_status",
    "client": "test_client",
    "requisition_title": "test_req_title",
    "account": "AAAA_1234",
    "candidate_email": "testEmail@123.com"
}]
```

##PUT
Insert a status

PUT /Corporate/CandidateStatus

Requirements:
  - account
  - email
  - status
  - status_date
  - status_level (must be either candidate or application) 
      - if candidate: requisition_id and requisition_title must be
        left off
    
      - if application: requisition_id is required. Requisition_title
        is still optional.

Sample request 1 (application):
```json
{
"account": "AAAA_1234",
"candidate_email": "testEmail@123.com",
"vendor_key": "test_vendor_key",
"client": "test_client",
"requisition_id": "123",
"level_id": "test_level_id",
"status": "test_status",
"status_date": "2017-03-19T19:04:01Z",
"status_level": "application",
"status_detail": "Test status detail"
}
```
Sample request 2 (candidate):
```json
{
"account": "AAAA_1234",
"candidate_email": "testEmail@123.com",
"vendor_key": "test_vendor_key",
"client": "test_client",
"level_id": "test_level_id",
"status": "test_status",
"status_date": "2017-03-19T19:04:01Z",
"status_level": "candidate",
"status_detail": "Test status detail"
}
```

Sample response
```
"Created/updated status."
```

##POST
Retrieve multiple emails or multiple requisition_ids

POST /Corporate/CandidateStatus

Requirements:
- account
- either a list of candidate emails or a list of requisition ids.

Optional:
- Status_date (only statuses that have  dates after the value provided are returned.)
    
Sample request 1:
```json
{
    "account": "aaaa_1234",
    "candidate_email": ["testEmail@123.com", "testEmail@1234.com"],
    "status_date": "2017-03-18T19:04:01Z"
}
```
Sample request 2:
```json
{
    "account": "aaaa_1234",
    "requisition_id": ["1", "2", "123"],
    "status_date": "2017-03-18T19:04:01Z"
}
```

Sample response:  
```json
[{
    "requisition_id": "1",
    "vendor_key": "test_vendor_key",
    "status_detail": "Test status detail",
    "status_level": "application",
    "status_date": "2017-03-19T23:04:01Z",
    "level_id": "test_level_id",
    "status": "test_status",
    "client": "test_client",
    "requisition_title": "test_req_title",
    "account": "AAAA_1234",
    "candidate_email": "testEmail@123.com"
},
{
    "requisition_id": "2",
    "vendor_key": "test_vendor_key",
    "status_detail": "Test status detail",
    "status_level": "application",
    "status_date": "2017-03-19T23:04:01Z",
    "level_id": "test_level_id",
    "status": "test_status",
    "client": "test_client",
    "requisition_title": "test_req_title",
    "account": "AAAA_1234",
    "candidate_email": "testEmail@123.com"
},
{
    "requisition_id": "123",
    "vendor_key": "test_vendor_key",
    "status_detail": "Test status detail",
    "status_level": "application",
    "status_date": "2017-03-19T23:04:01Z",
    "level_id": "test_level_id",
    "status": "test_status",
    "client": "test_client",
    "account": "AAAA_1234",
    "candidate_email": "testEmail@123.com"
}]
```
