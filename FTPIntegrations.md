### Overview 

FTP Integrations can be configured for both push and pull FTP and API types. The main configuration is a relatively simple json file which 
manages identifying information of the client, file to field mappings, where to send the information to be uploaded to MyCandidates,
and file types. 

### Configuration file fields

| Field | Data Type | Description |
| --- | --- | --- |
| keys | JSON Object | Root level json field which contains a json object specifying the keys for the configuration |
| keys.vendorKey | String | The vendorkey identifying the data source in MyCandidates. This should be specified by the CDS team, as it will determine the source of the data in MyCandidates |
| keys.clientKey | String | The unique key assigned to the client in MyCandidates. |
| keys.accountDID | String | The careerbuilder account id for the client. |
| mappings | JSON Object | Root level json field which contains a json object specifying file field to MyCandidates field mappings |
| mappings.mappingNamesAreAttributes | Boolean | Only applicable to XML file types. If true, will search in the attributes of an xml tag for the mapped values (the "attribute parent" and "attribute name" fields will be used to determine which tag is read). If false, will use the tag name as the mapping. |
| mappings.attributeName | String | If mappings.mappingNamesAreAttributes is true, specifies which attribute determines the field the content of the xml tag is mapped to. |
| mappings.attributeParent | String | If mappings.mappingNamesAreAttributes is true, specifies which xml tag name contains all field mapping attributes. |
| mappings.resumeText | String | If XML, the attribute or tag to use for the resume text MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the resume text field. |
| mappings.fileName | String | If XML, the attribute or tag to use for the file name MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the file name field. |
| mappings.email | String |  If XML, the attribute or tag to use for the email MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the email field. |
| mappings.postalCode | String |  If XML, the attribute or tag to use for the postal code MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the postal code field. |
| mappings.contactName | MultiField JSON Object | Determines how the integration should parse the contact name from the file. |
| mappings.contactName.fields | List of String | The fields to parse the contact name from. If XML, the list of attributes or tags. If CSV, the string list of column numbers (0 indexed). |
| mappings.contactName.displayDelimiter | String | The delimiter which will join the fields specified in the above box when the contact name is parsed for ingestion to MyCandidates. |
| mappings.tags | List of MultiField JSON Object | Determines how the integration should parse the one or more tags fields from the file. See the ContactName field for further explanation of the Multifield structure. |
| mappings.jobReqID | String | If XML, the attribute or tag to use for the job requisition id MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the job requisition id field. |
| mappings.activeHistory | String | If XML, the attribute or tag to use for the active history MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the active history field. |
| mappings.phoneNumber | String | If XML, the attribute or tag to use for the phone MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the phone number field. |
| mappings.countryCode | String | If XML, the attribute or tag to use for the country code MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the country code field. |
| mappings.cityName | String | If XML, the attribute or tag to use for the city name MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the city name field. |
| mappings.stateCode | String | If XML, the attribute or tag to use for the state code MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the state code field. |
| mappings.applicationJobTitle | String | If XML, the attribute or tag to use for the application job title MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the application job title field. |
| mappings.applicationJobSource | String | If XML, the attribute or tag to use for the application job source MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the application job source field. |
| mappings.mostRecentActivity | String | If XML, the attribute or tag to use for the most recent activity MyCandidates field. If a CSV, the string of the column number (0 indexed) to use for the most recent activity field. |
| apiInfo | JSON Object | Contains field related to where to upload candidates |
| apiInfo.CandidateEndpoint | String | The endpoint to retrieve the candidates from, if the importType is API|
| apiInfo.HostUrl | String | The host for the candidate endpoint if the importType is API |
| apiInfo.Password | String | The password for Basic Authentication if the importType is API |
| apiInfo.Username | String | the username for Basic Authentication if the importType is API |
| apiInfo.contentType | ContentType Enum integer | One of XML (1) or JSON (2). Used to parse the response if the importType is API |
| importType | ImportType enum integer | One of FTP (1) , API (2) or LOCAL (3). Determines the import type of the configuration |
| fileType | FileType enum integer | One of XML (1) or CSV (2). Determines the file type of imported candidates |
| filePath | String | If the importType is FTP, where to save candidates before upload. If the importType is LOCAL, where to look for candidates on the local server. For push FTP to CB's FTP server, this will typically be /tmp/processing/<FTP account name>  |
| filesAreIndexed | Boolean | Determines whether to look for resume files outside the main data file if the importType is LOCAL. If true, the mappings.fileName field will determine which file resume corresponds to that document in the "pathOfIndexedResumeFiles" directory |
| pathOfIndexedResumeFiles | String | Directory path to look for resume files when the importType is LOCAL. For push FTP to CB's FTP server, this will typically be /tmp/processing/<FTP account name> |
| credentials | JSON Object | Contains fields that determine how to connect to remote FTP servers if the importType is FTP. |
| credentials.username | String | Username for connecting to the FTP server if the importType is FTP. |
| credentials.password | String | Password for connecting to the FTP server if the importType is FTP. |
| dateTimeFormat | String | The date time format used in the MostRecentActivity and ActivityHistory fields for the data. Must be compatible with java DateTimeFormat.ForPattern method. |
| filesAreZipped | Boolean | Whether LOCAL indexed resumes are zipped. If set to true, the integration will attempt to unzip the resumes first. |
| ftpInfo | JSON Object | Fields that configure the FTP pull importType. |
| ftpInfo.HostUrl | The host URL of the FTP Server for the FTP pull importType. |
| ftpInfo.Port | The port of the FTP server to attempt to connect to for the FTP pull importType. |
| ftpInfo.Path | The path to change directory to to locate files on the FTP server for the FTP pull importType. |


### Example configuration

```
{
  "keys": {
    "vendorKey": "VKWQ5LV6FXT7GD4BY9S4",
    "clientKey": "CK9007068GFXXNWVW8WH",
    "accountDID": "A7C7ZB64KK855V1RR6W"
  },
  "mappings": {
    "mappingNamesAreAttributes": false,
    "fileName": "9",
    "email": "3",
    "postalCode": "6",
    "contactName": {
      "fields": [
        "1",
        "2"
      ],
      "displayDelimiter": " "
    },
    "jobReqID": "0",
    "applicationJobSource": "10"
  },
  "importType": 3,
  "fileType": 2,
  "filePath": "/tmp/processing/directory",
  "pathOfIndexedResumeFiles": "/tmp/processing/directory",
  "filesAreIndexed": true,
  "dateTimeFormat": "MM/dd/yyyy",
  "documentType": 0
}
```
