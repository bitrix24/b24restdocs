# Get Lead Fields crm.lead.fields

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.fields` returns the description of lead fields, including custom fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.fields
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
    require_once('crest.php');

    $result = CRest::call(
        'crm.lead.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
  ```

- PHP (B24PhpSdk)

  ```php      
  try {
      $fieldsResult = $serviceBuilder
          ->getCRMScope()
          ->lead()
          ->fields();
      $fieldsDescription = $fieldsResult->getFieldsDescription();
      // Assuming you want to print the fields description
      print_r($fieldsDescription);
  } catch (Throwable $e) {
      print("Error: " . $e->getMessage());
  }
  ```

{% endlist %}

## Response Handling

HTTP status: **200**

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
      "title": "Additional Source Information"
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
      "title": "Additional Stage Information"
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
      "title": "Open Line Provided"
    },
    "ASSIGNED_BY_ID": {
      "type": "user",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Responsible User"
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
      "title": "Campaign Identifier"
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
    "date_start": "2024-05-28T16:34:29+02:00",
    "date_finish": "2024-05-28T16:34:30+02:00",
    "operating": 0
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`lead`](#lead) | Root element of the response. Contains information about lead fields. The structure is described [below](#lead) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type lead {#lead}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Integer identifier of the lead ||
|| **TITLE**
[`string`](../../data-types.md) | Lead title ||
|| **HONORIFIC**
[`crm_status`](../../data-types.md) | Type of salutation. Status from the directory. A list of possible identifiers can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `filter[ENTITY_ID]=HONORIFIC` ||
|| **NAME**
[`string`](../../data-types.md) |  Contact's first name ||
|| **SECOND_NAME**
[`string`](../../data-types.md) |  Contact's middle name ||
|| **LAST_NAME**
[`string`](../../data-types.md) |  Contact's last name ||
|| **BIRTHDATE**
[`date`](../../data-types.md) | Birthdate ||
|| **COMPANY_TITLE**
[`string`](../../data-types.md) | Name of the company associated with the lead ||
|| **SOURCE_ID**
[`crm_status`](../../data-types.md) | Identifier of the source. Status from the directory. A list of possible identifiers can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `filter[ENTITY_ID]=SOURCE` ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Description of the source ||
|| **STATUS_ID**
[`crm_status`](../../data-types.md) | Identifier of the lead's stage. Status from the directory. A list of possible identifiers can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `filter[ENTITY_ID]=STATUS` ||
|| **STATUS_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the stage ||
|| **STATUS_SEMANTIC_ID**
[`string`](../../data-types.md) |
- F (failed) – processed unsuccessfully
- S (success) – processed successfully
- P (processing) – lead in processing ||
|| **POST**
[`string`](../../data-types.md) | Position ||
|| **ADDRESS**
[`string`](../../data-types.md) | Contact's address ||
|| **ADDRESS_2**
[`string`](../../data-types.md) | Second line of the address. In some countries, it is customary to split the address into 2 parts ||
|| **ADDRESS_CITY**
[`string`](../../data-types.md) | City ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../../data-types.md) | Postal code ||
|| **ADDRESS_REGION**
[`string`](../../data-types.md) | Region ||
|| **ADDRESS_PROVINCE**
[`string`](../../data-types.md) | Province ||  
|| **ADDRESS_COUNTRY**
[`string`](../../data-types.md) | Country ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../../data-types.md) | Country code ||
|| **ADDRESS_LOC_ADDR_ID**
[`string`](../../data-types.md) | Identifier of the address from the location module ||
|| **CURRENCY_ID**
[`crm_currency`](../../data-types.md) | Currency identifier ||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Estimated amount ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Indicator of manual calculation of the amount. Allowed values are Y or N ||
|| **OPENED**
[`char`](../../data-types.md) | Available to everyone. Allowed values are Y or N ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comments ||
|| **HAS_PHONE**
[`char`](../../data-types.md) | Indicator of whether the phone field is filled. Allowed values are Y or N ||
|| **HAS_EMAIL**
[`char`](../../data-types.md) | Indicator of whether the email field is filled. Allowed values are Y or N ||
|| **HAS_IMOL**
[`char`](../../data-types.md) | Indicator of whether there is an attached open line. Allowed values are Y or N ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user responsible for the lead ||
|| **CREATED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user who created the lead ||
|| **MODIFY_BY_ID**
[`user`](../../data-types.md) | Identifier of the user who last modified it ||
|| **MOVED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user who moved the item to the current stage ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Creation date ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Modification date ||
|| **MOVED_TIME**
[`datetime`](../../data-types.md) | Date the item was moved to the current stage ||
|| **COMPANY_ID**
[`crm_company`](../../data-types.md) | Link of the lead to the company (Client->Company field) ||
|| **CONTACT_ID**
[`crm_contact`](../../data-types.md) | Link of the lead to the contact (Client->Contact field. In case of multiple linked contacts, this field will contain the id of the first linked contact) ||
|| **IS_RETURN_CUSTOMER**
[`char`](../../data-types.md) | Indicator of a returning lead. Allowed values are Y or N ||
|| **DATE_CLOSED**
[`datetime`](../../data-types.md) | Closing date ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the data source. Used only for linking to an external source ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the item in the data source. Used only for linking to an external source ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system. Google Ads, Bing Ads, etc. ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Type of traffic. CPC (ads), CPM (banners) ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Campaign identifier ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Campaign content. For example, for contextual ads ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Campaign search condition. For example, keywords for contextual advertising ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../../data-types.md) | Time of the last activity ||
|| **LAST_ACTIVITY_BY**
[`string`](../../data-types.md) | Identifier of the user responsible for the last activity in this lead (e.g., who created a new activity in the lead) ||
|| **PHONE**
[`crm_multifield`](../../data-types.md) | Contact's phone ||
|| **EMAIL**
[`crm_multifield`](../../data-types.md) | Email address ||
|| **WEB**
[`crm_multifield`](../../data-types.md) | Lead's resource URL ||
|| **IM**
[`crm_multifield`](../../data-types.md) | Messengers ||
|| **LINK**
[`crm_multifield`](../../data-types.md) |  ||
|| **UF_...** | [Custom fields](./userfield/index.md) ||
|#

### Field Description

#|
|| **type** | **Field type. Described above** ||
|| **isRequired** | Indicates whether the field is mandatory when creating a new lead ||
|| **isReadOnly** | Indicates whether the field value can be edited ||
|| **isImmutable** | Indicates whether the field value can only be filled once during the creation of a new item ||
|| **isMultiple** | Indicates whether the field can have multiple values. If true, values are passed as an array ||
|| **isDynamic** | Indicates whether the field is [custom](./userfield/index.md) ||
|| **title** | Field name ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](./../../../_includes/system-errors.md) %}