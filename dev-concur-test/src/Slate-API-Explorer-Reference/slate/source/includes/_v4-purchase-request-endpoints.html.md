
## Purchase Request v4 - Endpoints



* [Create a New Purchase Request](#post)
* [Get Purchase Request Details](#get)
* [Schema](#schema)
  * [Create Purchase Request Schema](#schema-create-purchase-request)
    * [LineItem](#schema-line-item)
  * [Create Purchase Request Response Schema](#schema-create-purchase-request-response)
  * [Get Purchase Request Response Schema](#schema-get-purchase-request-response)
    * [PurchaseOrders](#schema-purchase-orders)
    * [PurchaseRequestExceptions](#schema-purchase-request-exceptions)
  * [Error](#schema-error)
* [Error Codes When the HTTP Status Code is 4xx](#error-codes)

### Version

4.0

#### <a name="post"></a>Create a New Purchase Request

Create a Purchase Request based on provided header and line item details. If the request is valid it creates a purchase request and returns back a unique identifier to get the purchase request details.

### Scopes

`purchaserequest.write` - Refer to [Scope Usage](/api-reference/invoice/v4.purchase-request-get-started.html#scope-usage) for full details.

### Request

#### URI

##### Template

```shell
POST /purchaserequest/v4/purchaserequests
```

##### Parameters

None

#### Headers

* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)
* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2) - Bearer Token that identifies the caller. This is the Company or User access token.
* `concur-correlationid` is a Concur specific custom header used for technical support in the form of a [RFC 4122 A Universally Unique IDentifier (UUID) URN Namespace](https://tools.ietf.org/html/rfc4122)

#### Payload

* [Create Purchase Request Schema](#schema-create-purchase-request)

### Response

#### Status Codes

* [202 Accepted](https://tools.ietf.org/html/rfc7231#section-6.3.3)
* [400 Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)
* [401 Unauthorized](https://tools.ietf.org/html/rfc7235#section-3.1)
* [500 Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)
* [503 Service Unavailable](https://tools.ietf.org/html/rfc7231#section-6.6.4)

#### Headers

* `concur-correlationid` is a Concur specific custom header used for technical support in the form of a [RFC 4122 A Universally Unique IDentifier (UUID) URN Namespace](https://tools.ietf.org/html/rfc4122)
* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)
* [RFC 7230 Content-Length](https://tools.ietf.org/html/rfc7230#section-3.3.2)
* [RFC 7231 Date](https://tools.ietf.org/html/rfc7231#section-7.1.1.2)
* [RFC 7231 Server](https://tools.ietf.org/html/rfc7231#section-7.4.2)

#### Payload

* [Create Purchase Request Response Schema](#schema-create-purchase-request-response)

### Example

#### Request

This is a sample set of fields. The fields and values your entity requires will vary based on your edition of Concur Invoice, and your forms and fields configuration. This example includes commonly used fields.

```shell
POST /purchaserequest/v4/purchaserequests
Authorization: Bearer {token}
Content-Type: application/json
```

```json
{
  "description" : "New office supplies",
  "userLoginId" : "john.deo@concur",
  "policyExternalId" : "po-external-id",
  "currencyCode" : "USD",
  "notesToSupplier" : "Office space request phase 1",
  "comments" : "office supplies request",
  "custom1" : "ADVT",
  "shipToAddressCode" : "SHIP15139",
  "billToAddressCode" : "MNSLP129",
  "lineItems" : [
    {
      "purchaseType" : "SERVICES",
      "vendorCode" :"VEN1",
      "vendorAddressCode" : "ADDR1",
      "description" : "monitor",
      "quantity" : "20",
      "unitPrice" : "154.4",
      "expenseType" : "1250",
      "receiptType" : "NONE",
      "neededByDate": "2018-06-28",
      "uomCode" : "DA",
      "shipping" : "13.5",
      "tax" : "11",
      "supplierPartId" : "DAQT1",
      "url" :[
        "http://officesupplies.com/monitor"
      ],
      "notesToVendor" : "Phase 1 request monitor",
      "comments" : "Phase 1 request for new employees for monitor",
      "custom2" : "LGVT1"
    },
    {
      "purchaseType" : "GOODS",
      "vendorCode" :"VEN1",
      "vendorAddressCode" : "ADDR1",
      "description" : "office chair",
      "quantity" : "20",
      "unitPrice" : "346.2",
      "expenseType" : "1251",
      "receiptType" : "QUANTITY_RECEIPT",
      "neededByDate": "2018-06-28",
      "uomCode" : "DA",
      "shipping" : "15",
      "tax" : "17.5",
      "supplierPartId" : "DAQT2",
      "url" :[
        "http://officesupplies.com/officechair"
      ],
      "notesToVendor" : "Phase 1 request office chair",
      "comments" : "Phase 1 request for new employees for office chair",
      "custom3" : "DEPT",
      "custom4" : "SALES"
    }
  ]
}
```

#### Response

```shell
HTTP/1.1 202 Accepted
Content-Type: application/json
Date: date-requested
Content-Length: 1000
concur-correlationid: 1234abcd-12ab-34cd-56ef-123456abcdef
```

```json
{
  "id" : "b1e22581-ff4a-48e9-981b-2f5065579096",
  "uri": "http://us.api.concursolutions.com/purchaserequest/v4/purchaserequests/b1e22581-ff4a-48e9-981b-2f5065579096?mode=COMPACT"
}
```

#### <a name="get"></a>Get Purchase Request Details

Gets purchase request details. The supported mode is COMPACT, which returns basic info about the purchase request along with any exceptions.

### Scopes

`purchaserequest.read` - Refer to [Scope Usage](/api-reference/invoice/v4.purchase-request-get-started.html#scope-usage) for full details.

### Request

#### URI

##### Template

```shell
GET /purchaserequest/v4/purchaserequests/{id}?mode=COMPACT
```

#### Parameters

Name|Type|Format|Description
---|---|---|---
`mode`|`string`|-|**Required**: Specifies mode for Get Purchase Request Details. Supported value: COMPACT

#### Headers

* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)
* [RFC 7235 Authorization](https://tools.ietf.org/html/rfc7235#section-4.2) - Bearer Token that identifies the caller. This is the Company or User access token.
* `concur-correlationid` is a Concur specific custom header used for technical support in the form of a [RFC 4122 A Universally Unique IDentifier (UUID) URN Namespace](https://tools.ietf.org/html/rfc4122)

#### Payload

None

### Response

#### Status Codes

* [200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)
* [404 Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)
* [500 Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)
* [503 Service Unavailable](https://tools.ietf.org/html/rfc7231#section-6.6.4)

#### Headers

* `concur-correlationid` is a Concur specific custom header used for technical support in the form of a [RFC 4122 A Universally Unique IDentifier (UUID) URN Namespace](https://tools.ietf.org/html/rfc4122)
* [RFC 7231 Content-Type](https://tools.ietf.org/html/rfc7231#section-3.1.1.5)
* [RFC 7230 Content-Length](https://tools.ietf.org/html/rfc7230#section-3.3.2)
* [RFC 7231 Date](https://tools.ietf.org/html/rfc7231#section-7.1.1.2)
* [RFC 7231 Server](https://tools.ietf.org/html/rfc7231#section-7.4.2)

#### Payload

* [Get Purchase Request Response Schema](#schema-get-purchase-request-response)

### Example

#### Request

```shell
GET /purchaserequest/v4/purchaserequests/de9c0894-b807-6943-8e3f-49a707da3456?mode=COMPACT
Authorization: Bearer {token}
Content-Type: application/json
```

#### Response

```shell
HTTP/1.1 200 OK
Content-Type: application/json
Date: date-requested
Content-Length: 1000
concur-correlationid: 1234abcd-12ab-34cd-56ef-123456abcdef
```

```json
{
  "purchaseRequestId" : "de9c0894-b807-6943-8e3f-49a707da3456",
  "purchaseRequestNumber" : "100000",
  "purchaseRequestQueueStatus" : "CREATED",
  "purchaseRequestWorkflowStatus" : "Approved",
  "purchaseRequestExceptions": [
    {
      "message": "Line Item Quantity does not match",
      "eventCode": "PURCH_DETAIL_ITEM_SAVE",
      "exceptionCode": "0070071",
      "isCleared": false,
      "prExceptionId": "fe636831-43a1-9540-bf86-32e2c19400af"
    }
  ],
  "purchaseOrders": [
    {
      "purchaseOrderNumber": "PO10001"
    }
  ]        
}
```

#### <a name="schema"></a>Schema

#### <a name="schema-create-purchase-request"></a>Create Purchase Request Schema

Name|Type|Format|Description
---|---|---|---
`userId`|`string`|-|**Required**: The employee that is requesting the items. This is the UUID of the employee. Either `UserId` or `UserEmail` or `UserLoginId` is required to identify the employee.
`userEmail`|`string`|-|**Required**: The employee that is requesting the items. This is the employee's email. Either `UserId` or `UserEmail` or `UserLoginId` is required to identify the employee.
`userLoginId`|`string`|-|**Required**: The employee that is requesting the items. This is the employee's Login Id. Either `UserId` or `UserEmail` or `UserLoginId` is required to identify the employee.
`description`|`string`|-|A description of the purchase request.
`policyExternalId`|`string`|-|The external identifier of the policy that should be associated with the purchase request. If not supplied, the API will use the default policy set up for the user group assigned to the requesting employee. This is the **External Id** from the Invoice Policy configuration. Clients will need to get these Ids from their SAP Concur contact if they need to assign policies other than the group default.
`currencyCode`|`string`|-|**Required**: The 3-letter ISO 4217 currency code of the currency that is associated with the purchase request. This code will be used for all items on this request. Example: USD
`notesToSupplier`|`string`|-|Notes to print on the transmitted purchase order PDF sent to the supplier.
`comments`|`string`|-|Internal comments related to this record.
`custom1 through custom24`|`string`|-|Each custom field used should have its own row in the message. If the field is tied to a connected list, the accepted value is the list Item Code configured for the list in SAP Concur.
`shipToAddressCode`|`string`|-|The shipping address of the Purchase Request. The accepted value is the address code from ShipTo record. If not supplied, the API will use the requesting user's default shipping address.
`billToAddressCode`|`string`|-|The billing address of the Purchase Request to be used for invoicing. The accepted value is the address code from the BillTo record. If not supplied the API will use the policy's default BillTo address.
`lineItems`|`array`|[`LineItem`](#schema-lineItem)|**Required**: Requested items or services related to this Purchase Request.

#### <a name="schema-line-item"></a>LineItem

Name|Type|Format|Description
---|---|---|---
`purchaseType`|`string`|-|**Required**: The type of item, either goods or services. Displayed as **Type** in Concur Invoice. Supported values: GOODS, SERVICES. 
`vendorCode`|`string`|-|**Required**: The code that identifies the vendor. This value can be found in the vendor information form of Vendor Manager. This is used along with `Vendor Address Code` to determine the specific Vendor record.
`vendorAddressCode`|`string`|-|**Required**: The code that identifies the vendor's address. This value can be found in the vendor information form of Vendor Manager and is labeled **Address Accounting Code**. This is used along with `Vendor Code` to determine the specific Vendor record.
`description`|`string`|-|**Required**: A description of the line item.
`quantity`|`decimal`|-|**Required**: The quantity associated with the line item.
`unitPrice`|`decimal`|-|**Required**: The unit price of the line item.
`expenseType`|`string`|-|The PET code of the Expense Type that will be assigned to the line item. If not supplied it will default to the Expense Type set up on the Vendor Profile used for the item. Clients will need to get these PET codes from their SAP Concur contact.
`receiptType`|`string`|-| The type of receipt. If not supplied, the API will use the `purchaseType` to set this field to NONE for SERVICES, or QUANTITY_RECEIPT for GOODS. If you are using SAP Concur Receiving and need to enter Goods Receipts against the resulting PO lines use QUANTITY_RECEIPT. Supported values: QUANTITY_RECEIPT, NONE.
`neededByDate`|`date`|YYYY-MM-DD|The date by which the purchase order must be fulfilled. Example: 2018-03-23
`uoMCode`|`string`|-|Unit of Measure (UOM) code for the purchase request item. Accepted values are the UOM Codes set up in the Unit of Measure configuration in Concur Invoice. If not supplied, the API will default a UOM based on the defaults for goods and services.
`shipping`|`decimal`|-|The total shipping cost for the item.
`tax`|`decimal`|-|Tax amount that is associated with the line item.
`supplierPartId`|`string`|-|An Id value that helps to identify the line item. This could be a value such as the vendor’s part number or the manufacturer number.
`url`|`array`|-|A URL related to the item. You can have multiple URLs per item, enclosed in quotes and comma separated.
`notesToVendor`|`string`|-|Notes related to the item that display on the transmitted purchase order PDF to the vendor.
`comments`|`string`|-|Internal comments related to this record.
`custom1 through custom20`|`string`|-|Each custom field used should have its own row in the message. If the field is tied to a connected list, the accepted value is the List Item Code configured for the list in SAP Concur.

#### <a name="schema-create-purchase-request-response"></a>Create Purchase Request Response Schema

Name|Type|Format|Description
---|---|---|---
`errors`|`array`|[`Error`](#schema-error)|An array of errors indicating which fields have failed validation.
`id`|`string`|-|The unique purchase request reference ID if the request has passed all validations. This reference ID will be needed to look up details of the purchase request.
`uri`|`string`|-|The URI to look up details of the newly created purchase request.

#### <a name="schema-get-purchase-request-response"></a>Get Purchase Request Response Schema

Name|Type|Format|Description
---|---|---|---
`purchaseRequestId`|`string`|-|The unique purchase request reference Id. Returned by the Create Purchase Request API call. 
`purchaseRequestNumber`|`string`|-|The unique purchase request identifier which can be used to uniquely identify a purchase request in SAP Concur products. 
`purchaseRequestQueueStatus`|`string`|-|The creation status of the purchase request. Possible values are: PENDING_CREATION, CREATED, CREATE_FAILED.
`purchaseRequestWorkflowStatus`|`string`|-|The workflow status of the purchase request. Possible values are: Approved, Pending Approval, Pending Cost Object Approval, Sent Back To Employee, Not Submitted, Submitted, Pending Processor Review, Vendor Approval, Approval Time Expired.
`purchaseOrders`|`array`|[`PurchaseOrders`](#schema-purchaseOrders)|If the purchase request has been approved and a purchase order generated, this array contains the purchase order details. If empty, this element will not be returned. 
`purchaseRequestExceptions`|`array`|[`PurchaseRequestExceptions`](#schema-purchaseRequestExceptions)|An array of exceptions, if present on the purchase request. If empty, this element will not be returned.

#### <a name="schema-purchase-orders"></a>PurchaseOrders

Name|Type|Format|Description
---|---|---|---
`purchaseOrderNumber`|`string`|-|The purchase order number.

#### <a name="schema-purchase-request-exceptions"></a>PurchaseRequestExceptions

Name|Type|Format|Description
---|---|---|---
`eventCode`|`string`|-|The event code of the exception. Example: PURCH_DETAIL_SUBMIT
`exceptionCode`|`string`|-|The unique exception code.
`isCleared`|`boolean`|-|Whether the exception has been cleared.
`prExceptionId`|`string`|-|The unique exception id of the purchase request.
`message`|`string`|-|The message of the exception with details.

#### <a name="schema-error"></a>Error

Name|Type|Format|Description
---|---|---|---
`errorCode`|`string`|-|An error code indicating why a field failed validation.
`errorMessage`|`string`|-|A description of the error.
`dataPath`|`string`|-|The path to the request data which has the error message.

#### <a name="error-codes"></a>Error Codes When the HTTP Status Code is 4xx

ErrorCode|Error Message
---|---
missingRequestBody|Missing request body.
invalidRequestBody|Passed request body is invalid.
missingUserInfo|Either `userID` or `userEmail` or `userLoginId` is required.
invalidUserInfo|Either `userID` or `userEmail` or `userLoginId` is invalid, or user does not have access to this resource.
provideOneUserInformation|Either `userID` or `userEmail` or `userLoginId` is required.
missingCurrencyCode|`currencyCode` is missing.
invalidCurrencyCode|`currencyCode` is invalid.
invalidPolicyInformation|Cannot find a purchase order policy with the supplied `policyExternalId`.
missingLineItems|`lineItems` are missing.
invalidPurchaseType|`purchaseType` is invalid.
missingPurchaseType|`purchaseType` is required.
missingVendorAddressCode|`vendorAddressCode` is required.
missingVendorCode|`vendorCode` is required.
invalidVendor|Vendor / Address code combination is invalid.
missingDescription|Line item `description` is required.
missingQuantity|Line item `quantity` is required.
invalidQuantity|Line item `quantity` is invalid.
missingUnitPrice|`unitPrice` is required.
invalidUnitPrice|`unitPrice` is invalid.
invalidDateFormat|Expected a date in the format YYYY-MM-DD.
