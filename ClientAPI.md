# Overview
The MySupply Client APIs provides access to client level information and within MySupply.

You will need CareerBuilder OAuth credentials to use this API, and auth code flow is required as document retrieval must be performed on behalf of a CareerBuilder user.

## What is a "Client"?
MyCandidates is a private data store, and we use clients to assign ownership of subsets of myCandidates data. Each client has a unique key. Currently there is one client key for each axiom account with data in myCandidates

Clients have top level information about the client, and they also own Attribute types.

# Client API

##  Resource URI
 [https://api.careerbuilder.com/corporate/mysupply/client](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fapi.careerbuilder.com%2F&postURL=corporate%2Fmysupply%2Fclient&method=get&contentType=application%2Fjson&acceptType=application%2Fjson&version=default&region=productionus&flow=client_credentials&userDid=&accountDid=&headers=&body=)

## Actions

### GET
Retrieves the list of all active clients and their data.

```json
{
    data: [
        {
            "client_key": "CK8Q0276KVZS0Z0LYVLM",
            "client_name": "Amerit Consulting Inc.",
            "account_did": "A7A0RX6PY8FX77TQ0RW",
            "account_tcv": 457189,
            "annual_price": 38500,
            "contract_start_date": "2015-11-19T01:00:00",
            "contract_end_date": "2017-09-18T01:00:00",
            "deactivation_date": "1970-01-01T00:00:00Z",
            "currency": "USD"
        }

    ]

}
```

### GET

 [https://api.careerbuilder.com/corporate/mysupply/client/{id}](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fapi.careerbuilder.com%2F&postURL=corporate%2Fmysupply%2Fclient%2FCK000000000000000000&method=get&contentType=application%2Fjson&acceptType=application%2Fjson&version=default&region=productionus&flow=client_credentials&userDid=&accountDid=&headers=&body=)

 Returns information for a specific client by providing a ClientKey/AccountDID or PIID

```json
{
    "data": {
        "client_name": "Test Account",
        "account_did": "000000000000000001",
        "is_active": true,
        "product_instance_id": "",
        "client_key": "CK0003Z6CJQH0ZX1WYMC",
        "contract_start_dt": "4/11/2017 12:00:00 AM",
        "contract_end_dt": "4/11/2017 12:00:00 AM",
        "deactivated_dt": ""
    }
}
```

# Client Custom Fields API
 
##  Resource URI
[https://api.careerbuilder.com/corporate/mysupply/candidate/client/{id}/customfields](https://apimanagement.cbplatform.link/?#routes/tester?preURL=https%3A%2F%2Fwwwtest.api.careerbuilder.com%2F&postURL=corporate%2Fmysupply%2Fclient%2FCK000000000000000000%2Fcustomfields&method=get&contentType=application%2Fjson&acceptType=application%2Fjson&version=default&region=staging&flow=client_credentials&userDid=&accountDid=&headers=&body=)

## Custom Fields

### GET
returns all the custom fields defined for that ClientKey, AccountDID or ProductInstanceID

```JSON
{
  "data": {
    "custom_fields": [
        {
            "name": "MyCustomFieldName_HasCDL",
            "label": "Do you possess a CDL?",
            "vendor_key": "VK5K5ZB61ZLSWX5PXKV8",
            "is_facetable": true,
            "is_searchable": true
        },
        {
            "name": "SomeOtherCustomField",
            "label": "Any random question text here",
            "vendor_key": "VK5K5ZB61ZLSWX5PXKV8",
            "is_facetable": false,
            "is_searchable": true
        }
    ]
  }
}
```

# Client Attribute Types API
Attribute types are defined per client. Their values are defined per candidate. See the [Attribute API](https://github.com/cbdr/MyCandidatesDocumentation/blob/master/AttributeAPI.md)
 
##  Resource URI
[https://api.careerbuilder.com/corporate/mysupply/candidate/client/{clientkey}/attribute](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fwwwtest.api.careerbuilder.com%2F&postURL=corporate%2Fmysupply%2Fcandidate%2Fclient%2FCKHT82M6S1Y6DRRBSQLY%2Fattribute&method=get&contentType=application%2Fjson&acceptType=application%2Fjson&version=default&region=staging&flow=client_credentials&userDid=&accountDid=&headers=&body=)

## Actions

### GET
returns all the attribute types defined for that client key

```json
{
    "data": {
        "attribute_types": [
            {
                "type": "Available_Date",
                "label": "Available Date",
                "owner_vendor_key": "VK1234567890"
            },
            {
                "type": "CAMPAIGN_DELIVERED",
                "label": "CAMPAIGN_DELIVERED",
                "owner_vendor_key": "VK1234567890"
            },
        ]
    }
 }
 ```

### POST

Creates a new attribute type for the specified client. This operation is an upsert, meaning if the type already exists, it is not re-created or duplicated.

``` JSON

{
    "type":"yournewtype",
    "label": "Your New Type",
    "owner_vendor_key": "Vendor Key of vendor creating attribute"
}

```

### PUT Multiple Types for a Client
 
 [PUT {accountDID}/attribute/]
 
 An attribute type can only exist for an account once and are currently de-duplicated internally.
 
 This endpoint will create type definitions and update any matching existing types with the new definition. 
 
 The request body consists of a JSON array of the following JSON object:

 ```javascript

 {
     [
         {
             "type": "**type**",
             "label": "**label**"
         }
     ]
 }
 ```

 **type** is required and is the identifier for the type.
 
 **label** is required and is the name that will be returned when searching.
 
 **vendorkey** is a required QSVAR that is used to specify the owning myCandidates vendor. This field is by the Search UI for grouping attributes under their owning vendors.
 [POST {candidate_email}/attribute/{type}](https://apimanagement.cbplatform.link/#routes/tester?preURL=https%3A%2F%2Fwwwtest.api.careerbuilder.com%2F&postURL=corporate%2Fmysupply%2Fcandidate%2Fcandidate%40email.com%2Fattribute%2Femailcampaign%3Fvendorkey%3DVKK7JT6ZP0DY9Y6TWCM&method=post&contentType=application%2Fjson&acceptType=application%2Fjson&version=default&region=staging&flow=authorization_code&userDid=U7I5HW6PZR57WG9SWQC&accountDid=A7A2CS6782H1NVC3HVB&headers=&body={"value"%3A"xmas"})
 
 
 A 200 and the values placed is returned upon success.
 
 ```javascript

 {
     "data": {
         "Attributes": [
             {
                 "clientkey": "CK1234567890",
                 "owner_vendor_key": "VK1234567890",
                 "type": "emailcampaign",
                 "label": "Email Campaign",
                 "is_searchable": true,
                 "is_facetable": true
             },
             {
                 "clientkey": "CK1234567890",
                 "owner_vendor_key": "VK1234567890",
                 "type": "Interview",
                 "label": "Interview",
                 "is_searchable": true,
                 "is_facetable": true
             },
             {
                 "clientkey": "CK1234567890",
                 "owner_vendor_key": "VK1234567890",
                 "type": "Favorite_Color",
                 "label": "What's Your Favorite Color?",
                 "is_searchable": true,
                 "is_facetable": true
             },
          ]
     }
 }
 ```


