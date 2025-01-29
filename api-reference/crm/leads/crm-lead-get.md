# Get Lead by Id crm.lead.get

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with read access permission for the requested lead

The method `crm.lead.get` returns a lead by its identifier.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** || 
|| **id***
[`integer`](../../data-types.md) | The identifier of the lead.

The identifier can be obtained using the methods [crm.lead.list](./crm-lead-list.md) or [crm.lead.add](./crm-lead-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":123}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":123,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.get
    ```

- JS

    ```javascript 
    BX24.callMethod(
      'crm.lead.get',
      { id: 123 },
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
        'crm.lead.get',
        [
            'ID' => 123
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
  "result": {
    "ID": "123",
    "TITLE": "Lead #1591",
    "HONORIFIC": null,
    "NAME": "",
    "SECOND_NAME": null,
    "LAST_NAME": null,
    "COMPANY_TITLE": null,
    "COMPANY_ID": null,
    "CONTACT_ID": null,
    "IS_RETURN_CUSTOMER": "N",
    "BIRTHDATE": "",
    "SOURCE_ID": "1",
    "SOURCE_DESCRIPTION": null,
    "STATUS_ID": "IN_PROCESS",
    "STATUS_DESCRIPTION": null,
    "POST": null,
    "COMMENTS": null,
    "CURRENCY_ID": "USD",
    "OPPORTUNITY": "0.00",
    "IS_MANUAL_OPPORTUNITY": "N",
    "HAS_PHONE": "N",
    "HAS_EMAIL": "N",
    "HAS_IMOL": "N",
    "ASSIGNED_BY_ID": "1",
    "CREATED_BY_ID": "1",
    "MODIFY_BY_ID": "1",
    "DATE_CREATE": "2024-05-23T18:18:25+02:00",
    "DATE_MODIFY": "2024-05-23T18:18:25+02:00",
    "DATE_CLOSED": "",
    "STATUS_SEMANTIC_ID": "P",
    "OPENED": "Y",
    "ORIGINATOR_ID": null,
    "ORIGIN_ID": null,
    "MOVED_BY_ID": "1",
    "MOVED_TIME": "2024-05-23T18:18:25+02:00",
    "ADDRESS": null,
    "ADDRESS_2": null,
    "ADDRESS_CITY": null,
    "ADDRESS_POSTAL_CODE": null,
    "ADDRESS_REGION": null,
    "ADDRESS_PROVINCE": null,
    "ADDRESS_COUNTRY": null,
    "ADDRESS_COUNTRY_CODE": null,
    "ADDRESS_LOC_ADDR_ID": null,
    "UTM_SOURCE": null,
    "UTM_MEDIUM": null,
    "UTM_CAMPAIGN": null,
    "UTM_CONTENT": null,
    "UTM_TERM": null,
    "LAST_ACTIVITY_BY": "1",
    "LAST_ACTIVITY_TIME": "2024-05-23T18:18:25+02:00",
    "PHONE": [
      {
        "ID": "11658",
        "VALUE_TYPE": "OTHER",
        "VALUE": "+15454777777",
        "TYPE_ID": "PHONE"
      }
    ],
    "IM": [
      {
        "ID": "11660",
        "VALUE_TYPE": "OPENLINE",
        "VALUE": "imol|livechat|1|67|21",
        "TYPE_ID": "IM"
      }
    ]
  },
  "time": {
      "start": 1705764932.998683,
      "finish": 1705764937.173995,
      "duration": 4.1753120422363281,
      "processing": 3.3076529502868652,
      "date_start": "2024-01-20T18:35:32+02:00",
      "date_finish": "2024-01-20T18:35:37+02:00",
      "operating_reset_at": 1705765533,
      "operating": 3.3076241016387939
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`lead`](#lead) | The root element of the response. Contains information about the lead fields. The structure is described [below](#lead) ||
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
[`string`](../../data-types.md) | Title of the lead ||
|| **HONORIFIC**
[`crm_status`](../../data-types.md) | Type of address. Status from the directory. A list of possible identifiers can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `filter[ENTITY_ID]=HONORIFIC` ||
|| **NAME**
[`string`](../../data-types.md) | Contact's first name ||
|| **SECOND_NAME**
[`string`](../../data-types.md) | Contact's middle name ||
|| **LAST_NAME**
[`string`](../../data-types.md) | Contact's last name ||
|| **COMPANY_ID**
[`crm_company`](../../data-types.md) | Link of the lead to the company ||
|| **COMPANY_TITLE**
[`string`](../../data-types.md) | Company name ||
|| **CONTACT_ID**
[`crm_contact`](../../data-types.md) | Link of the lead to the contact ||
|| **IS_RETURN_CUSTOMER**
[`char`](../../data-types.md) | Indicator of a returning lead. Allowed values are Y or N ||
|| **BIRTHDATE**
[`date`](../../data-types.md) | Date of birth ||
|| **SOURCE_ID**
[`crm_status`](../../data-types.md) | Identifier of the source. Status from the directory. A list of possible identifiers can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `filter[ENTITY_ID]=SOURCE` ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Description of the source ||
|| **STATUS_ID**
[`crm_status`](../../data-types.md) | Identifier of the lead stage. Status from the directory. A list of possible identifiers can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `filter[ENTITY_ID]=STATUS` ||
|| **STATUS_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the stage ||
|| **POST**
[`string`](../../data-types.md) | Position ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comments ||
|| **CURRENCY_ID**
[`crm_currency`](../../data-types.md) | Currency identifier ||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Estimated amount ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Indicator of manual calculation of the amount. Allowed values are Y or N ||
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
[`user`](../../data-types.md) | Identifier of the user who made the last modification ||
|| **MOVED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user who moved the item to the current stage ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Creation date ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Modification date ||
|| **DATE_CLOSED**
[`datetime`](../../data-types.md) | Closing date ||
|| **STATUS_SEMANTIC_ID**
[`string`](../../data-types.md) |
- F (failed) – processed unsuccessfully
- S (success) – processed successfully
- P (processing) – lead is being processed ||
|| **OPENED**
[`char`](../../data-types.md) | Indicator of whether the lead is available to everyone. Allowed values are Y or N ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the data source. Used only for linking to an external source ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the item in the data source. Used only for linking to an external source ||
|| **MOVED_TIME**
[`datetime`](../../data-types.md) | Date the item was moved to the current stage ||
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
[`string`](../../data-types.md) | Used for internal purposes ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system. Google Ads, Facebook Ads, etc. ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Type of traffic. CPC (ads), CPM (banners) ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Designation of the advertising campaign ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Search term of the campaign. For example, keywords for contextual advertising ||
|| **LAST_ACTIVITY_BY**
[`string`](../../data-types.md) | Identifier of the user responsible for the last activity in this lead (e.g., who created a new CRM activity in the lead) ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../../data-types.md) | Time of the last activity ||
||**UF_...** | Custom fields. For example, `UF_CRM_25534736`.  

Depending on the account settings, leads may have a set of custom fields of specific types. 

To create, modify, or delete custom fields in leads, use the methods [crm.lead.userfield.*](./userfield/index.md) ||
|| **PHONE**
[`crm_multifield`](../../data-types.md) | Array of phone numbers ||
|| **IM**
 [`crm_multifield`](../../data-types.md) | Array of messengers ||
|#

## Error Handling

> 40x, 50x Error

```json
{
    "error": "",
    "error_description": "Not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Error Text** | **Description** ||
|| `ID is not defined or invalid` | The `id` parameter is either not provided or is not a positive integer ||
|| `Not found`  | The lead with the specified `id` was not found ||
|| `Access denied` | The user does not have permission to read the lead ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}