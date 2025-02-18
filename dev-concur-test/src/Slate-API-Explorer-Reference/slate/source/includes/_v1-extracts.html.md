
## Extracts v1


* [Get an array of extract definitions](#get-definitions)
* [Get a single extract definition](#get-definition)
* [Get an extract file or files](#get-file)
* [Get an array of extract jobs](#get-job-array)
* [Get a single extract job](#getexjd)
* [Get a single extract job status](#getexjs)
* [Post an extract job initiation request](#postexjr)
* [Schema](#schema)
  * [Definitions](#schema-definitions)
  * [Definition](#schema-definition)
  * [Jobs](#schema-jobs)
  * [Job](#schema-job)

### Capabilities

The Extract web service allows for the development of a customized process that can be used to request and extract available data objects such as “Approved” Expense Reports, Invoices, and Payment Requests.

Through a series of web service calls an Application Connector can GET the Extract Definition List, then use that list to perform a POST the Extract Job of their choosing. This POST will initiate the extract process. While the job runs another process can periodically GET the Extract Job Status and once “Complete” the Connector can then perform a GET to obtain the Extract Job File data.

The series of calls that your connector will make will remain the same for each implementation of the Extract Web Service.

Additionally, it provides:

* Access to extract files using HTTP to move or manage the data as opposed to using a secure FTP site.
* Clients with organizational units around the globe can create and receive extracts at times that work best for their time zones. To accomplish this, you can work with your SAP Concur support resource to create a unique Extract Definition for each organizational unit that requires a separate extract.
* A way to receive extract files throughout the day including, but not limited to, outside the Overnight Processing Period (ONP).

> Note: A client may elect to include additional functionality that could result in complex journal entries. For example, they may allow cash advances or utilize a company-paid corporate card program where personal amounts result in an employee owing the employer. These configuration choices require more care when pulling the extract file from SAP Concur. Work with their functional experts on their specific use case, then watch the relevant videos in the [**SAE Detailed Discussions**](http://www.concurtraining.com/prdeployment/sts) section at the bottom of the page for detailed information on which fields to include.

Resource URI

```
https://{InstanceURL}/api/expense/extract/v1.0
https://{InstanceURL}/api/expense/extract/v1.0/{DefinitionID}/job
https://{InstanceURL}/api/expense/extract/v1.0/{DefinitionID}/job/status
https://{InstanceURL}/api/expense/extract/v1.0/{DefinitionID}/job/{JobID}/file
```

### Version

1.0

### Base URI

```
/api/expense/extract/v1.0/
```

### Authorization

The `Authorization` header with OAuth token for a valid SAP Concur user must be included with all API calls. The OAuth consumer must have one of the following user roles in SAP Concur: Company Administrator or Web Services Administrator for Professional, or Can Administer for Standard. These roles allow the user to manage data for the entire company.

#### <a name="get-definitions"></a>Get an Array of Extract Definitions

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)

#### Parameters

None

#### Payload

Name|Type|Format|Description
---|---|---|---
`name`|`type`|JSON|**Required** Description.

### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)

#### Headers

* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5) `application/xml`

#### Payload

[Definitions](#schema-definitions)

### Example

#### Request

```
GET https://www.concursolutions.com/api/expense/extract/v1.0/
Authorization: OAuth {token}
```

#### Response

```
HTTP/1.1 200 OK
Content-Type: application/xml
```

```xml
<definitions xmlns="http://www.concursolutions.com/api/expense/extract/2010/02">
  <definition>
    <id>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd</id>
    <job-link>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job</job-link>
    <name>Standard Accounting Extract</name>
  </definition>
  <definition>
    <id>https://www.concursolutions.com/api/expense/extract/v1.0/Umj$swS19lpd7Sk$phUYl67wE1ss4Q$shu</id>
    <job-link>https://www.concursolutions.com/api/expense/extract/v1.0/Umj$swS19lpd7Sk$phUYl67wE1ss4Q$shu</job-link>
    <name>European Extract</name>
  </definition>
  <definition>
    <id>https://www.concursolutions.com/api/expense/extract/v1.0/8LjhN23Hs33$piUUfy4ytTqa$sqqacYeP1</id>
    <job-link>https://www.concursolutions.com/api/expense/extract/v1.0/8LjhN23Hs33$piUUfy4ytTqa$sqqacYeP1</job-link>
    <name>Asian Extract</name>
  </definition>
</definitions>
```

#### <a name="get-definition"></a>Get a single extract definition

Retrieves the details of the specified extract definition.

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)

#### Parameters

Name|Type|Format|Description
---|---|---|---
`DefinitionID`|`string`|-|**Required** The identifier for the desired extract definition.

##### URI Template

```
https://www.concursolutions.com/api/expense/extract/v1.0/{DefinitionID}
```

#### Payload

Name|Type|Format|Description
---|---|---|---
`name`|`type`|JSON|**Required** Description.

### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)

#### Headers

* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5) `application/xml`

#### Payload

[Definition](#schema-definition)

### Example
```
GET https://www.concursolutions.com/api/expense/extract/v1.0/n59FpBJ8hN3qVWTFIrtxkOT5pef6DmIj3
Authorization: OAuth {token}
```

#### Request
```
HTTP/1.1 200 OK
Content-Type: application/xml
```


#### Response



```xml
<definition xmlns="http://www.concursolutions.com/api/expense/extract/2010/02" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <id>https://www.concursolutions.com/api/expense/extract/v1.0/n59FpBJ8hN3qVWTFIrtxkOT5$pef6DmIj3</id>
    <name>AMEX Remittance - US</name>
    <job-link>https://www.concursolutions.com/api/expense/extract/v1.0/n59FpBJ8hN3qVWTFIrtxkOT5$pef6DmIj3/job</job-link>
</definition>
```

#### <a name="get-file"></a>Get an extract file or files

The extracted data for the specified extract job.

The response is the extracted data in `text/csv` format if there was a single file produced or as `application/zip` if the extract definition is configured to produce more than one file.

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)

#### Parameters

Name|Type|Format|Description
---|---|---|---
`DefinitionID`|`string`|-|**Required** The definition identifier.
`JobID`|`string`|-|**Required** The identifier for the job.

##### URI Template

```
https://www.concursolutions.com/api/expense/extract/v1.0/{DefinitionID}/job/{JobID}/file
```

#### Payload

Name|Type|Format|Description
---|---|---|---
`name`|`type`|JSON|**Required** Description.

### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)

#### Headers

* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)
  * `text/csv`
  * `application/zip`

#### Payload

Name|Type|Format|Description
---|---|---|---
`name`|`type`|format|description

### Example
```
GET https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uIo87jk3SHudi$sdlYle8$peot$pD21jyd/file
Authorization: OAuth {token}
```
#### Request
```
HTTP/1.1 200 OK
Content-Type: text/csv
```

```
100,AAA,BBBB,CCCC,...<rest of file>
```


#### Response
```
HTTP/1.1 200 OK
Content-Type: application/zip
```

```
{ zip archive data}
```

#### Single file


#### Multiple files



#### <a name="get-job-array"></a>Get an Array of Extract Jobs

Requests a list of the last 100 extract jobs ran for the specified Extract Definition.

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)

#### Parameters

Name|Type|Format|Description
---|---|---|---
`DefinitionID`|`string`|-|**Required** The definition identifier.

##### URI Template

```
https://www.concursolutions.com/api/expense/extract/v1.0/{DefinitionID}/job
```

#### Payload

Name|Type|Format|Description
---|---|---|---
`name`|`type`|JSON|**Required** Description.

### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)

#### Headers

* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5) `application/xml`

#### Payload

[Jobs](#schema-jobs)

### Example

```
GET https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job HTTP/1.1
Authorization: OAuth {token}
```

#### Request
```
HTTP/1.1 200 OK
Content-Type: application/xml
```

#### Response



```xml
<jobs xmlns="...">
  <job>
    <id>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uIo87jk3SHudi$sdlYle8$peot$pD21jyd</id>
    <status-link>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uIo87jk3SHudi$sdlYle8$peot$pD21jyd/status</status-link>
    <start-time>2010-01-13T18:30:02Z</start-time>
    <status>Queued</status>
  </job>
  <job>
    <id>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/21UwwqA3jk25Lis77jF$piiD21c89lLwEq</id>
    <status-link>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/21UwwqA3jk25Lis77jF$piiD21c89lLwEq/status</status-link> 
    <start-time>2010-01-13T18:30:02Z</start-time>
    <stop-time>2010-01-13T18:30:50Z</stop-time>
    <status>Complete</status>
    <file-link>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/21UwwqA3jk25Lis77jF$piiD21c89lLwEq/file</file-link>
  </job>
  <job>
    <id>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uI77ndy0Q1szuU73XSh56lshi$p215gHs1</id>
    <status-link>https://www.concursolutions.com/api/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uI77ndy0Q1szuU73XSh56lshi$p215gHs1/status</status-link>
    <start-time>2010-01-12T18:30:01Z</start-time>
    <stop-time>2010-01-12T18:31:01Z</stop-time>
    <status>Complete</status>
    <file-link>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uI77ndy0Q1szuU73XSh56lshi$p215gHs1/file</file-link>
  </job>
</jobs>
```

#### <a name="getexjd"></a>Get a Single Extract Job

Retrieves the details of the specified extract definition.

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)

#### Parameters

Name|Type|Format|Description
---|---|---|---
`DefinitionID`|`string`|-|**Required** The identifier for the desired extract definition.
`JobID`|`string`|-|**Required** The identifier for the job and the job keyword.

##### URI Template

```
https://www.concursolutions.com/api/expense/extract/v1.0/{DefinitionID}/job/{JobID}
```

#### Payload

None

### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)

#### Headers

* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5) `application/xml`

#### Payload

[Job](#schema-job)

### Example
```
GET https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uIo87jk3SHudi$sdlYle8$peot$pD21jyd
Authorization: OAuth {token}
```

#### Request
```
HTTP/1.1 200 OK
Content-Type: application/xml
```


#### Response



```xml
<job xmlns="...">
  <id>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uIo87jk3SHudi$sdlYle8$peot$pD21jyd></id>
  <status-link>https://www.concursolutions.com/api/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uIo87jk3SHudi$sdlYle8$peot$pD21jyd/status</status-link>
  <start-time>2010-01-12T18:30:01</start-time>
  <stop-time>2010-01-12T18:31:01</stop-time>
  <status>Complete</status>
  <file-link> https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uIo87jk3SHudi$sdlYle8$peot$pD21jyd/file</file-link>
</job>
```

#### <a name="getexjs"></a>Get a Single Extract Job Status

Retrieves the details of the specified extract definition.

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)

#### Parameters

Name|Type|Format|Description
---|---|---|---
`DefinitionID`|`string`|-|**Required** The identifier for the desired extract definition.
`JobID`|`string`|-|**Required** The identifier for the job and the job keyword.

##### URI Template

```
https://www.concursolutions.com/api/expense/extract/v1.0/{DefinitionID}/job/{JobID}/status
```

#### Payload

None

### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)

#### Headers

* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5) `application/xml`

#### Payload

[Job](#schema-job)

### Example
```
https://www.concursolutions.com/api/expense/extract/v1.0/nX8O9$pytn6vJEWvLOZxyy3GcNGyj0ZklG/job/nIJp1lR2R0LNT4XcO5fXG$s$sZmVuRTuG$ps/status
{Header}: {Value}
```

#### Request
```
HTTP/1.1 200 OK
Content-Type: application/xml
```

#### Response


```xml
<job xmlns="http://www.concursolutions.com/api/expense/extract/2010/02" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <id>https://www.concursolutions.com/api/expense/extract/v1.0/nX8O9$pytn6vJEWvLOZxyy3GcNGyj0ZklG/job/nIJp1lR2R0LNT4XcO5fXG$s$sZmVuRTuG$ps</id>
  <status-link>https://www.concursolutions.com/api/expense/extract/v1.0/nX8O9$pytn6vJEWvLOZxyy3GcNGyj0ZklG/job/nIJp1lR2R0LNT4XcO5fXG$s$sZmVuRTuG$ps/status</status-link>
  <start-time>2011-08-25T14:25:22.58</start-time>
  <stop-time>2011-08-25T14:25:23.537</stop-time>
  <status>Completed</status>
  <file-link>https://www.concursolutions.com/api/expense/extract/v1.0/nX8O9$pytn6vJEWvLOZxyy3GcNGyj0ZklG/job/nIJp1lR2R0LNT4XcO5fXG$s$sZmVuRTuG$ps/file</file-link>
</job>
```

#### <a name="postexjr"></a>Post an Extract Job Initiation Request

Initiates a new extract job for the specified extract definition. The job will be scheduled to start as soon as possible. The value will be Queued until the job begins.

### Request

#### Headers

* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2)
* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5) `application/xml`

#### Parameters

Name|Type|Format|Description
---|---|---|---
`DefinitionID`|`string`|-|**Required** The identifier for the desired extract definition.

##### URI Template

```
https://www.concursolutions.com/api/expense/extract/v1.0/{DefinitionID}/job
```

#### Payload

[Definition](#schema-definition)

### Response

#### Status Codes

* [201 Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)

#### Headers

* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5) `application/xml`
* [RFC 7231 Location](https://tools.ietf.org/html/rfc7231#section-7.1.2)

#### Payload

[Job](#schema-job)

### Example
```
POST https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job
Authorization: OAuth {token}
Content-Type: application/xml
```

#### Request
```
HTTP/1.1 201 Created
Location: https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uIo87jk3SHudi$sdlYle8$peot$pD21jyd
Content-Type: application/xml
```


```xml
<definition xmlns="http://www.concursolutions.com/api/expense/extract/2010/02">
  <id>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd</id>
</definition>
```

#### Response



```xml
<job xmlns="...">
  <id>https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uIo87jk3SHudi$sdlYle8$peot$pD21jyd</id>
  <status-link> https://www.concursolutions.com/api/expense/extract/v1.0/nYoPK$pZmcowMRUqcl5bnDAwwsMydyt$xd/job/uIo87jk3SHudi$sdlYle8$peot$pD21jyd/status</status-link>
  <start-time>2010-01-13T18:30:02Z</start-time>
  <status>Queued</status>
</job>
```

#### <a name="schema"></a>Schema

### <a name="schema-definitions"></a>Definitions

Name|Type|Format|Description
---|---|---|---
`definitions`|`array`|[definition](#schema-definition)|**Required**

### <a name="schema-definition"></a>Definition

Name|Type|Format|Description
---|---|---|---
`id`|`string`|-|The extract definition ID URI with encrypted ID.
`job-link`|`string`|-|The extract job URI with encrypted ID. The job-link value is used as the URI when creating the extract job data request.
`name`|`string`|-|The extract definition name.

### <a name="schema-jobs"></a>Jobs

Name|Type|Format|Description
---|---|---|---
`jobs`|`array`|[job](#schema-job)|**Required**

### <a name="schema-job"></a>Job

Name|Type|Format|Description
---|---|---|---
`id`|`string`|-|The unique job identifier with encrypted ID.
`status-link`|`string`|-|A URI for retrieving the current job status, with encrypted ID. The **status-link** value is used as the URI when requesting the job status.
`start-time`|`string`|-|The time in UTC (GMT) the job is scheduled to start.
`stop-time`|`string`|-|The time in UTC (GMT) the job finished. Not included if the job is still running.
`status`|`string`|-|The current status of the job.
`file-link`|`string`|-|A URI for retrieving the file or files produced by the job run, with encrypted ID. Not included if the job is still running. The `file-link` value is used as the URI when retrieving the extract data.

#### <a name="erp-integration"></a>Enterprise Resource Planning Integration

For the purposes of ERP integration this API is available to Professional Edition. For Standard Edition use [Payment Batches v1 API ERP Integration](/api-reference/expense/payment-batch/v1.payment-batches.html#erp-integration).

1. [Get an array of extract definitions](#get-definitions)
  * Locate the desired definitions for your client. You may need to obtain additional details as there may be multiple files to obtain depending on client requirements.
  * Record the Definition ID to use in subsequent API requests.
2. [Get an array of extract jobs](#get-job-array)
  * OPTIONAL: the Partner only needs to do this if the Partner is running the job, otherwise skip this step.
  * Determine which jobs are interesting and record their `file-link`.
3. [Get an extract file or files](#get-file)
