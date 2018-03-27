# Candidate-Actions

## General

The candidate actions endpoint is designed to allow users to store and retrieve small pieces of information around an action taken on a candidate. A Candidate is defined as an email/account combination. Actions are tied to this combination, not a specific resume. 

### Access

The endpoints can be accessed through the Candidate Search Zuul interface, with OAuth credentials. Users (tied to OAuth credentials) must be white listed by Candidate Search before they will be accepted (other users will get a 401 – Unauthorized reply). Authorized users are specific to region (staging, US production and EU production.

### Endpoints

 - Staging: https://wwwtest.api.careerbuilder.com/corporate/candidate-actions
 - US Production: https://api.careerbuilder.com/corporate/candidate-actions
 - EU Production: https://api.careerbuilder.eu/corporate/candidate-actions

### Verbs

 - PUT – record a new action
 - GET – Retrieve actions from a specific Account/Candidate combination
 - POST – Retrieve actions from a specific Account and a list of candidates

## Support

The Candidate-Actions endpoint is currently supported by the Candidate Search team. Email CandidateSearchDevelopment@careerbuilder.com for answers to questions and support.

## Versions

| Version | Description | Release Date |
| ------- | ----------- | ------------ |
| 0.1.0 | Store and retrieve by Account and Candidate Email | 1/4/2017 |

### Details

0.1.0 - initial rollout. Data is inserted and retrieved at this end point only, is not tied into Candidate Search in any way. Cannot be used to search.
 
# PUT

A put is used to insert a new action into the database. All of the data comes from the payload, which is JSON with the following fields (all are of type string):

| Field Name | Description | Required | Validation |
| ---------- | ----------- | -------- | ---------- |
| event | The action type | Yes | Limited to set of actions, see below. Contact Candidate Search if you need another action. |
| account | CB account code | Yes | Currently not validated |
| person | ID used by application to define the person | Yes | Max length 256. This is the persons email unless they do not have one. |
| recruiter | The person who took the action | No | Max length 80 |
| recruiter_id | ID of the person who took the action | No | Max length 40. The CB recruiter ID. |
| product_instance | Sub account information | No | Max length 120 |
| detail | Additional information about action.  Should be human readable, not machine readable. | No | Max length 4096 |
| source_system_candidate_id | Candidate ID as known to the source system | No | Max length 40 |
| source_origin | Where this event originated | No | Max length 20 |
| document_id | Document ID associated with this event | No | Max length 40 |
| datetime | Date and time action happened | No (but recommended) | Defaults to ‘now’ if present, must be in form ccyy-mm-ddThh:mm:ssZ, or some other form readable by JSON |

## Events (action types):  

 - AddToEmailCampaign
 - Assign
 - Blocked
 - DownloadProfile
 - DownloadResume
 - Email
 - ExportToAts
 - Forward
 - Hired
 - InPersonInterview
 - JobOffer
 - Note
 - OfferReject
 - PhoneCall
 - PhoneInterview
 - Preview
 - Print
 - ReceiveResume
 - RemoveFromList
 - RequestResume
 - SaveToList
 - StatusChange
 - Unblocked
 - View
 
*Note:  this list might change.  If an invalid event is requested, the response will list the valid events.*

Example of contents:

    {
      "event": "Email",
      "account": "acct1234",
      "person": "person4568",
      "recruiter": "recruit-abcc",
      "product_instance": "Product Instance 1",
      "detail": "This is just a test",
      "source_system_candidate_id": "some-test-candidate",
      "source_origin": "Postman",
      "document_id": "54321abc",
      "datetime": "2016-03-15T13:52:12Z"
    }
    
The response is just a string, either “Created New Action” or an error message describing the problem. 

Response Code 200 is Success, 400 indicates an error has been found in the request and 500 indicates and internal error has occurred. Requests that return a 400 should not be repeated, they should fail reliably. Requests that generate a 500 may work, even immediately, depending on the nature of the problem.

Current Valid Actions are limited to: Assign, Blacklist, Comment, DownloadProfile, DownloadResume, Email, Forward, Hired, InPersonInterview, JobOffer, Note, OfferReject, PhoneCall, PhoneInterview, ReceiveResume, RemoveFromList, RequestResume, SaveToList, StatusChange, View, Whitelist.

# Post

A post is used to retrieve a collection of actions pertaining to a single account and a number of persons. In addition, the maximum number of actions per person and the oldest action to be returned can be specified. 

| Field Name | Description | Required | Validation |
| ---------- | ----------- | -------- | ---------- |
| account | AccountID all persons belong to | Yes | None |
| persons | List of strings | Yes | Must have at least 1 |
| results_per_person | Max number of actions per person | No | Integer, defaults to all |
| datetime | Date of earliest action to be returned | No | Defaults to all (beginning of time) |

Example:

    URL:  corporate/candidate-actions
    
    Request:
    {
      "persons": ["person4568", “person1111”],
      "results_per_person": "6",
      "account": "acct1234",
      "datetime" : "2016-01-11T14:34:09Z"
    }
    
    Response:
    {
      "account": "acct1234",
      "actions_per_person": [{
        "person": "person4568",
        "actions": [{
          "event": "Action-Event03",
          "product_instance": "Hilton-MySupply",
          "datetime": "2017-01-11T14:34:10Z",
          "person": "person4568",
          "recruiter": "recruit-abcc",
          "detail": "This is just a test",
          "account": "acct1234"
        }, {
          "event": "Action-Event99",
          "product_instance": "Hilton-MySupply",
          "datetime": "2016-03-15T13:52:12Z",
          "person": "person4568",
          "recruiter": "recruit-abcc",
          "detail": "This is just a test",
          "account": "acct1234"
        }, {
          "event": "Action-Event01",
          "product_instance": "Hilton-MySupply",
          "datetime": "2016-03-15T13:52:12Z",
          "person": "person4568",
          "recruiter": "recruit-abcc",
          "detail": "This is just a test",
          "account": "acct1234"
        }]
      }]
    }
    
# Post Recent

A post for recent actions is used to retrieve a collection of actions pertaining to a person and optionally to an account too.  A collection of the most recent actions is returned for the past 90 days (configurable).  Actions are distinct if the account or event or recruiter_id are different.  Recent actions are restricted to a subset of events.  The current subset of events is View, Email and SaveToList (configurable).

| Field Name | Description | Required | Validation |
| ---------- | ----------- | -------- | ---------- |
| account | AccountID the person belongs to | No | None |
| person | Identifier of a candidate | Yes | Nonempty |

Example:

    URL:  corporate/candidate-actions/recent

    Request:
    {
      "person": "blaine@host.com",
      "account": "blaine-portal"
    }

    Response:
    {
      "person": "blaine@host.com",
      "actions": [
        {
          "event": "Email",
          "product_instance": "dev-region",
          "datetime": "2017-06-02T03:04:05Z",
          "source_origin": "Postman",
          "recruiter_id": "shaunid",
          "person": "blaine@host.com",
          "account": "blaine-portal"
        }
      ]
    }

# GET

The get is used to retrieve data for a specific 

    Get: https://cs-actionsapi-staging.ace.careerbuilder.com/actions/acct1234/person4567

It currently does not include parameters for maximum number of records or earliest record to be returned

The action return fields are the same as the PUT request fields.

Example returns:

    [{
      "event": "Action-Event03",
      "product_instance": "Hilton-MySupply",
      "datetime": "2017-01-11T14:34:10Z[UTC]",
      "person": "person4568",
      "recruiter": "recruit-abcc",
      "detail": "This is just a test",
      "account": "acct1234"
    }, {
      "event": "Action-Event99",
      "product_instance": "Hilton-MySupply",
      "datetime": "2016-03-15T13:52:12Z[UTC]",
      "person": "person4568",
      "recruiter": "recruit-abcc",
      "detail": "This is just a test",
      "account": "acct1234"
    }]
