# Create a New Lead crm.lead.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to create leads.

This method creates a new lead.

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** | **Revision** ||
|| **fields***
`object` | An array must specify the values of specific lead fields (`"field"=>"value"[, ...]`). [List of possible fields](./crm-lead-fields.md) | 1 ||
|| **options**
`object` | An optional array of options (`"optionName"=>"value"[, ...]`). The list of possible fields is described below | 1 ||
|#

### Parameter fields

#|
|| **Field** / **Type** | **Description** ||
|| **ADDRESS**
[`string`](../../data-types.md) | Contact address ||
|| **ADDRESS_2**
[`string`](../../data-types.md) | Second line of the address. In some countries, it is customary to split the address into 2 parts. ||
|| **ADDRESS_CITY**
[`string`](../../data-types.md) | City. ||
|| **ADDRESS_COUNTRY**
[`string`](../../data-types.md) | Country. ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../../data-types.md) | Country code. ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../../data-types.md) | Postal code. ||
|| **ADDRESS_PROVINCE**
[`string`](../../data-types.md) | Province. ||
|| **ADDRESS_REGION**
[`string`](../../data-types.md) | Region. ||
|| **ADDRESS_LOC_ADDR_ID**
[`string`](../../data-types.md) | Used for internal purposes only. ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Responsible person. ||
|| **BIRTHDATE**
[`date`](../../data-types.md) | Date of birth. ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comments. ||
|| **COMPANY_ID**
[`crm_company`](../../data-types.md) | Link the lead to a company. ||
|| **COMPANY_TITLE**
[`string`](../../data-types.md) | The name of the company specified in the corresponding lead field. To link an existing company, pass its id in the COMPANY_ID field. ||
|| **CONTACT_ID**
[`crm_contact`](../../data-types.md) | Link the lead to a contact. ||
|| **CONTACT_IDS**
[`crm_contact`](../../data-types.md) | Identifier of the linked contact. Multiple. When using crm.deal.update and crm.deal.add, an array of contacts can be submitted. In the methods crm.deal.list and crm.deal.get, this field is not present, and crm.deal.contact.items.get must be used to retrieve the list of contacts. To clear the field, use crm.deal.contact.items.delete; to replace the value, use crm.deal.contact.items.set. ||
|| **CURRENCY_ID**
[`crm_currency`](../../data-types.md) | Currency identifier. ||
|| **EMAIL**
[`crm_multifield`](../../data-types.md) | Email address. Multiple. ||
|| **HONORIFIC**
[`crm_status`](../../data-types.md) | Form of address. ||
|| **IM**
[`crm_multifield`](../../data-types.md) | Messenger. Multiple. ||
|| **LINK**
[`crm_multifield`](../../data-types.md) | User ID linked through an open line. Multiple. ||
|| **LAST_NAME**
[`string`](../../data-types.md) | Last name ||
|| **NAME**
[`string`](../../data-types.md) | First name ||
|| **OPENED**
[`char`](../../data-types.md) | Indicates the availability of the lead for everyone. Acceptable values are `Y` or `N`. ||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Amount. ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Indicates manual mode for calculating the amount. Acceptable values are Y or N. ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the data source. Used only for linking to an external source. ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the item in the data source. Used only for linking to an external source. ||
|| **PHONE**
[`crm_multifield`](../../data-types.md) | Phone. Multiple. ||
|| **POST**
[`string`](../../data-types.md) | Position. ||
|| **SECOND_NAME**
[`string`](../../data-types.md) | Middle name ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Description of the source. ||
|| **SOURCE_ID**
[`crm_status`](../../data-types.md) | Identifier of the source. 
Default values:

#|
||SOURCE_ID|Name||
||CALL|Call||
||EMAIL|E-mail||
||WEB|Website||
||ADVERTISING|Advertising||
||PARTNER|Existing client||
||RECOMMENDATION|By recommendation||
||TRADE_SHOW|Trade show||
||WEBFORM|CRM form||
||CALLBACK|Callback||
||RC_GENERATOR|Sales generator||
||STORE|Online store||
||OTHER|Other||
|#

A list of all possible identifiers from the directory can be obtained using the method crm.status.list with the filter `filter[ENTITY_ID]=SOURCE` ||
|| **STATUS_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the stage ||
|| **STATUS_ID**
[`crm_status`](../../data-types.md) | Identifier of the lead stage. Default stages:

#|
||STATUS_ID|Name||
||NEW | Unprocessed||
||IN_PROCESS | In progress||
||PROCESSED | Processed||
||JUNK | Poor quality lead||
||CONVERTED | Quality lead||
|#

A list of all possible stages from the directory can be obtained using the method crm.status.list with the filter `filter[ENTITY_ID]=STATUS` ||
|| **TITLE**
[`string`](../../data-types.md) | Title of the lead. ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Identifier of the advertising campaign. ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads. ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Type of traffic. CPC (ads), CPM (banners). ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system. Yandex-Direct, Google-Adwords, and others. ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Search condition of the campaign. For example, keywords for contextual advertising. ||
|| **WEB**
[`crm_multifield`](../../data-types.md) | Website. Multiple. ||
|| **ufCrm_xxx** | Custom fields. See section [{#T}](../universal/user-defined-fields/index.md) ||
|#

{% note info %}

Additionally, to find out the required format of the fields, you can execute the method [crm.lead.fields](crm-lead-fields.md) and check the format of the incoming values of these fields.

{% endnote %}

{% note info %}

When adding a lead, you cannot explicitly set the repeat lead indicator (the `IS_RETURN_CUSTOMER` field); however, this field automatically takes the value Y if you specify a value for `COMPANY_ID` or `CONTACT_ID` when adding the lead.

{% endnote %}

## Parameter options

#|
|| **Parameter** / **Type** | **Description** | **Revision** ||
|| **REGISTER_SONET_EVENT**
[`boolean`](../../data-types.md) | Register the event of adding a lead in the activity stream. A notification will also be sent to the person responsible for the lead. | 1 ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    ```js
    BX24.callMethod(
        "crm.lead.add",
        {
            fields:
            {
                TITLE: "LLC Miracle",
                NAME: "John",
                LAST_NAME: "Dou",
                STATUS_ID: "NEW",
                OPENED: "Y",
                ASSIGNED_BY_ID: 1,
                CURRENCY_ID: "USD",
                OPPORTUNITY: 12500,
                PHONE: [
                    { 
                        VALUE: "555888",
                        VALUE_TYPE: "WORK",
                    },
                ] ,
                WEB: [
                        {
                        VALUE: "www.mysite.com",
                        VALUE_TYPE: "WORK",
                        }
                ],
            },
            params: {
                REGISTER_SONET_EVENT: "Y",
            }
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error());
                
                return;
            }
            
            console.info(`Lead created with ID ${result.data()}`);
        }
    );
    ```

- PHP

    // example for PHP

{% endlist %}


{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response on Success

> 200 OK

```json
{
    "result": 3465,
    "time": {
        "start": 1705764932.998683,
        "finish": 1705764937.173995,
        "duration": 4.1753120422363281,
        "processing": 3.3076529502868652,
        "date_start": "2024-01-20T18:35:32+03:00",
        "date_finish": "2024-01-20T18:35:37+03:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Value** / **Type** | **Description** ||
|| **result**
`integer`| Identifier of the created lead ||
|| **time**
[`array`](../../data-types.md) | Information about the execution time of the request ||
|| **start**
[`double`](../../data-types.md) | Timestamp of the moment the request was initialized ||
|| **finish**
[`double`](../../data-types.md) | Timestamp of the moment the request execution was completed ||
|| **duration**
[`double`](../../data-types.md) | How long in milliseconds the request took (finish - start) ||
|| **date_start**
[`string`](../../data-types.md) | String representation of the date and time of the moment the request was initialized ||
|| **date_finish**
[`double`](../../data-types.md) | String representation of the date and time of the moment the request execution was completed ||
|| **operating_reset_at**
[`timestamp`](../../data-types.md) | Timestamp of the moment when the limit on REST API resources will be reset. Read more in the article [operation limits](../../../limits.md) ||
|| **operating**
[`double`](../../data-types.md) | In how many milliseconds will the limit on REST API resources be reset? Read more in the article [operation limits](../../../limits.md) ||
|#


## Response on Error

> HTTP status: 40x, 50x Error

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Error Text** | **Description** ||
|| Empty value | Access denied. | The user does not have permission to add a lead. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-lead-delete.md)
- [{#T}](./crm-lead-fields.md)