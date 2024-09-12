# Get Lead Fields crm.lead.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.fields` returns the description of lead fields, including custom ones.

No parameters.

## Examples

{% list tabs %}

- cURL

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    https://xxx.bitrix24.com/rest/crm.lead.fields
    ```

- JS

    ```javascript 
    BX24.callMethod(
      'crm.lead.fields',
      {},
      (result) => {
        if(result.error())
        {
          console.error(result.error());
  
          return;
        }
        
        console.info(result.data());
      }
    );
    ```

- PHP

    ```php
    $result = CRest::call('crm.lead.fields');
    ```

- HTTPS

    ```http
    https://xxx.bitrix24.com/rest/1/5***/crm.lead.fields.json
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Successful Response

> 200 OK

```json
{
  "result": {
    "ID": {
      "type": "integer",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "ID"
    },
    "TITLE": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Lead Title"
    },
    "HONORIFIC": {
      "type": "crm_status",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "statusType": "HONORIFIC",
      "title": "Salutation"
    },
    "NAME": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "First Name"
    },
    "SECOND_NAME": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Middle Name"
    },
    "LAST_NAME": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Last Name"
    },
    "BIRTHDATE": {
      "type": "date",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Birthdate"
    },
    "COMPANY_TITLE": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Company Name"
    },
    "SOURCE_ID": {
      "type": "crm_status",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "statusType": "SOURCE",
      "title": "Source"
    },
    "SOURCE_DESCRIPTION": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Source Description"
    },
    "STATUS_ID": {
      "type": "crm_status",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "statusType": "STATUS",
      "title": "Stage"
    },
    "STATUS_DESCRIPTION": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Stage Description"
    },
    "STATUS_SEMANTIC_ID": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Status State"
    },
    "POST": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Position"
    },
    "ADDRESS": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Address"
    },
    "ADDRESS_2": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Address (line 2)"
    },
    "ADDRESS_CITY": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "City"
    },
    "ADDRESS_POSTAL_CODE": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Postal Code"
    },
    "ADDRESS_REGION": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Region"
    },
    "ADDRESS_PROVINCE": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Province"
    },
    "ADDRESS_COUNTRY": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Country"
    },
    "ADDRESS_COUNTRY_CODE": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Country Code"
    },
    "ADDRESS_LOC_ADDR_ID": {
      "type": "integer",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Location Address ID"
    },
    "CURRENCY_ID": {
      "type": "crm_currency",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Currency"
    },
    "OPPORTUNITY": {
      "type": "double",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Amount"
    },
    "IS_MANUAL_OPPORTUNITY": {
      "type": "char",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "IS_MANUAL_OPPORTUNITY"
    },
    "OPENED": {
      "type": "char",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Available to Everyone"
    },
    "COMMENTS": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Comment"
    },
    "HAS_PHONE": {
      "type": "char",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Phone Provided"
    },
    "HAS_EMAIL": {
      "type": "char",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "E-mail Provided"
    },
    "HAS_IMOL": {
      "type": "char",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Open Line Assigned"
    },
    "ASSIGNED_BY_ID": {
      "type": "user",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Responsible"
    },
    "CREATED_BY_ID": {
      "type": "user",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Created By"
    },
    "MODIFY_BY_ID": {
      "type": "user",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Modified By"
    },
    "MOVED_BY_ID": {
      "type": "user",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "MOVED_BY_ID"
    },
    "DATE_CREATE": {
      "type": "datetime",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Creation Date"
    },
    "DATE_MODIFY": {
      "type": "datetime",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Modification Date"
    },
    "MOVED_TIME": {
      "type": "datetime",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "MOVED_TIME"
    },
    "COMPANY_ID": {
      "type": "crm_company",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Company",
      "settings": {
        "parentEntityTypeId": 4
      }
    },
    "CONTACT_ID": {
      "type": "crm_contact",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "isDeprecated": true,
      "title": "Contact"
    },
    "CONTACT_IDS": {
      "type": "crm_contact",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": true,
      "isDynamic": false,
      "title": "CONTACT_IDS"
    },
    "IS_RETURN_CUSTOMER": {
      "type": "char",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Returning Lead"
    },
    "DATE_CLOSED": {
      "type": "datetime",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Closing Date"
    },
    "ORIGINATOR_ID": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "External Source"
    },
    "ORIGIN_ID": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Identifier in External Source"
    },
    "UTM_SOURCE": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Advertising System"
    },
    "UTM_MEDIUM": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Traffic Type"
    },
    "UTM_CAMPAIGN": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Campaign Designation"
    },
    "UTM_CONTENT": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Campaign Content"
    },
    "UTM_TERM": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Campaign Search Condition"
    },
    "LAST_ACTIVITY_TIME": {
      "type": "datetime",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "LAST_ACTIVITY_TIME"
    },
    "LAST_ACTIVITY_BY": {
      "type": "user",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "LAST_ACTIVITY_BY"
    },
    "PHONE": {
      "type": "crm_multifield",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": true,
      "isDynamic": false,
      "title": "Phone"
    },
    "EMAIL": {
      "type": "crm_multifield",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": true,
      "isDynamic": false,
      "title": "E-mail"
    },
    "WEB": {
      "type": "crm_multifield",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": true,
      "isDynamic": false,
      "title": "Website"
    },
    "IM": {
      "type": "crm_multifield",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": true,
      "isDynamic": false,
      "title": "Messenger"
    },
    "LINK": {
      "type": "crm_multifield",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": true,
      "isDynamic": false,
      "title": "LINK"
    }
  },
  "time": {
    "start": 1716903269.951179,
    "finish": 1716903270.017765,
    "duration": 0.06658601760864258,
    "processing": 0.029553890228271484,
    "date_start": "2024-05-28T16:34:29+03:00",
    "date_finish": "2024-05-28T16:34:30+03:00",
    "operating": 0
  }
}
```

### Returned Data

#|
|| **Value** / **Type** | **Description** ||
|| **result**
[`array`](../../data-types.md) | Result of the request ||
|| **ID**
[`integer`](../../data-types.md) | Integer identifier of the lead. ||
|| **TITLE**
[`string`](../../data-types.md) | Title of the lead. ||
|| **HONORIFIC**
[`crm_status`](../../data-types.md) | Type of salutation. Status from the directory. A list of possible identifiers can be obtained using the method crm.status.list with the filter `filter[ENTITY_ID]=HONORIFIC` ||
|| **NAME**
[`string`](../../data-types.md) | First name of the contact. ||
|| **SECOND_NAME**
[`string`](../../data-types.md) | Middle name of the contact. ||
|| **LAST_NAME**
[`string`](../../data-types.md) | Last name of the contact. ||
|| **BIRTHDATE**
[`date`](../../data-types.md) | Birthdate. ||
|| **COMPANY_TITLE**
[`string`](../../data-types.md) | Name of the company associated with the lead. ||
|| **SOURCE_ID**
[`crm_status`](../../data-types.md) | Identifier of the source. Status from the directory. A list of possible identifiers can be obtained using the method crm.status.list with the filter `filter[ENTITY_ID]=SOURCE` ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Description of the source. ||
|| **STATUS_ID**
[`crm_status`](../../data-types.md) | Identifier of the lead stage. Status from the directory. A list of possible identifiers can be obtained using the method crm.status.list with the filter `filter[ENTITY_ID]=STATUS` ||
|| **STATUS_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the stage. ||
|| **STATUS_SEMANTIC_ID**
[`string`](../../data-types.md) |
- F (failed) – processed unsuccessfully,
- S (success) – processed successfully,
- P (processing) – lead is being processed. ||
|| **POST**
[`string`](../../data-types.md) | Position. ||
|| **ADDRESS**
[`string`](../../data-types.md) | Address of the contact. ||
|| **ADDRESS_2**
[`string`](../../data-types.md) | Second line of the address. In some countries, it is customary to split the address into 2 parts. ||
|| **ADDRESS_CITY**
[`string`](../../data-types.md) | City. ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../../data-types.md) | Postal code. ||
|| **ADDRESS_REGION**
[`string`](../../data-types.md) | Region. ||
|| **ADDRESS_PROVINCE**
[`string`](../../data-types.md) | Province. ||  
|| **ADDRESS_COUNTRY**
[`string`](../../data-types.md) | Country. ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../../data-types.md) | Country code. ||
|| **ADDRESS_LOC_ADDR_ID**
[`string`](../../data-types.md) | Identifier of the address from the location module. ||
|| **CURRENCY_ID**
[`crm_currency`](../../data-types.md) | Identifier of the currency. ||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Estimated amount. ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Indicator of manual calculation of the amount. Allowed values are Y or N. ||
|| **OPENED**
[`char`](../../data-types.md) | Available to everyone. Allowed values are Y or N. ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comments. ||
|| **HAS_PHONE**
[`char`](../../data-types.md) | Indicator of whether the phone field is filled. Allowed values are Y or N. ||
|| **HAS_EMAIL**
[`char`](../../data-types.md) | Indicator of whether the email field is filled. Allowed values are Y or N. ||
|| **HAS_IMOL**
[`char`](../../data-types.md) | Indicator of whether an open line is assigned. Allowed values are Y or N. ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user responsible for the lead. ||
|| **CREATED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user who created the lead. ||
|| **MODIFY_BY_ID**
[`user`](../../data-types.md) | Identifier of the user who last modified it. ||
|| **MOVED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user who moved the item to the current stage. ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Creation date. ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Modification date. ||
|| **MOVED_TIME**
[`datetime`](../../data-types.md) | Date the item was moved to the current stage. ||
|| **COMPANY_ID**
[`crm_company`](../../data-types.md) | Link of the lead to the company (Client->Company field) ||
|| **CONTACT_ID**
[`crm_contact`](../../data-types.md) | Link of the lead to the contact (Client->Contact field. In case of multiple linked contacts, this field will contain the id of the first linked contact). ||
|| **IS_RETURN_CUSTOMER**
[`char`](../../data-types.md) | Indicator of a returning lead. Allowed values are Y or N. ||
|| **DATE_CLOSED**
[`datetime`](../../data-types.md) | Closing date. ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the data source. Used only for linking to an external source. ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the item in the data source. Used only for linking to an external source. ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system. Yandex-Direct, Google-Adwords, and others. ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Traffic type. CPC (ads), CPM (banners). ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Designation of the advertising campaign. ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads. ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Search condition of the campaign. For example, keywords for contextual advertising. ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../../data-types.md) | Time of the last activity. ||
|| **LAST_ACTIVITY_BY**
[`string`](../../data-types.md) | Identifier of the user responsible for the last activity in this lead (for example, who created a new CRM activity in the lead). ||
|| **PHONE**
[`crm_multifield`](../../data-types.md) | Phone of the contact. ||
|| **EMAIL**
[`crm_multifield`](../../data-types.md) | Email address. ||
|| **WEB**
[`crm_multifield`](../../data-types.md) | URL resources of the lead. ||
|| **IM**
[`crm_multifield`](../../data-types.md) | Messengers. ||
|| **LINK**
[`crm_multifield`](../../data-types.md) |  ||
|| **ufCrm_xxx** | [Custom fields.](./userfield/index.md) ||
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
[`timestamp`](../../data-types.md) | Timestamp of the moment when the limit on REST API resources will be reset. Read more in the article [operation limit](../../../limits.md) ||
|| **operating**
[`double`](../../data-types.md) | In how many milliseconds will the limit on REST API resources be reset? Read more in the article [operation limit](../../../limits.md) ||
|#

### Field Description

|#
|| type | Type of the field. Described above. ||
|| isRequired | Indicator of whether the field is mandatory when creating a new lead. ||
|| isReadOnly | Indicator of whether the value of the field can be edited. ||
|| isImmutable | Indicator of whether the field value can only be filled once when creating a new item. ||
|| isMultiple | Indicator of whether the field can have multiple values. If true, values in the field are passed as an array. ||
|| isDynamic | Indicates whether the field is [custom](./userfield/index.md). ||
|| title | Title of the field ||
#|

{% include [Footnote on parameters](../../../_includes/required.md) %}

## Example Response in Case of Error

The method does not return errors.