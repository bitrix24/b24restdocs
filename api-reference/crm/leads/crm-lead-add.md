# Create a New Lead crm.lead.add

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to create leads

The method `crm.lead.add` creates a new lead.

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — field name
- `value_n` — field value

The list of available fields is described [below](#fields)
||
|| **options**
[`object`](../../data-types.md) | Optional array of options (`"optionName"=>"value"[, ...]`). The list of possible fields is described [below](#options) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ADDRESS**
[`string`](../../data-types.md) | Address ||
|| **ADDRESS_2**
[`string`](../../data-types.md) | Second line of the address. In some countries, it is common to split the address into 2 parts ||
|| **ADDRESS_CITY**
[`string`](../../data-types.md) | City ||
|| **ADDRESS_COUNTRY**
[`string`](../../data-types.md) | Country ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../../data-types.md) | Country code ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../../data-types.md) | Postal code ||
|| **ADDRESS_PROVINCE**
[`string`](../../data-types.md) | State ||
|| **ADDRESS_REGION**
[`string`](../../data-types.md) | Region ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Responsible person ||
|| **BIRTHDATE**
[`date`](../../data-types.md) | Date of birth ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comments ||
|| **COMPANY_ID**
[`crm_company`](../../data-types.md) | Link the lead to a company ||
|| **COMPANY_TITLE**
[`string`](../../data-types.md) | Company name specified in the corresponding lead field. To link an existing company, pass its id in the COMPANY_ID field ||
|| **CONTACT_ID**
[`crm_contact`](../../data-types.md) | Link the lead to a contact ||
|| **CONTACT_IDS**
[`crm_contact`](../../data-types.md) | List of contacts linked to the lead.

Contacts can be added or removed using the group of methods [crm.lead.contact.*](./management-communication/index.md) ||
|| **CURRENCY_ID**
[`crm_currency`](../../data-types.md) | Currency identifier ||
|| **EMAIL**
[`crm_multifield`](../../data-types.md) | Email address. Multiple ||
|| **HONORIFIC**
[`crm_status`](../../data-types.md) | Salutation ||
|| **IM**
[`crm_multifield`](../../data-types.md) | Messenger. Multiple ||
|| **LINK**
[`crm_multifield`](../../data-types.md) | User ID linked through an open line. Multiple ||
|| **LAST_NAME**
[`string`](../../data-types.md) | Last name ||
|| **NAME**
[`string`](../../data-types.md) | First name ||
|| **SECOND_NAME**
[`string`](../../data-types.md) | Middle name ||
|| **OPENED**
[`char`](../../data-types.md) | Indicator of lead availability to everyone. Acceptable values are `Y` or `N`.||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Amount ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Indicator of manual calculation mode for the amount. Acceptable values are Y or N||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the data source.

Used only for linking to an external source ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the item in the data source. Used only for linking to an external source ||
|| **PHONE**
[`crm_multifield`](../../data-types.md) | Phone. Multiple ||
|| **POST**
[`string`](../../data-types.md) | Position ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Source description ||
|| **SOURCE_ID**
[`crm_status`](../../data-types.md) | Source identifier. 
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

The list of all possible identifiers from the directory can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `filter[ENTITY_ID]=SOURCE` ||
|| **STATUS_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the stage ||
|| **STATUS_ID**
[`crm_status`](../../data-types.md) | Identifier of the lead stage. Default stages:

#|
||STATUS_ID|Name||
||NEW | Unprocessed||
||IN_PROCESS | In progress||
||PROCESSED | Processed||
||JUNK | Low-quality lead||
||CONVERTED | High-quality lead||
|#

The list of all possible stages from the directory can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `filter[ENTITY_ID]=STATUS` ||
|| **TITLE**
[`string`](../../data-types.md) | Lead title ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Advertising campaign designation ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Campaign content. For example, for contextual ads ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Traffic type. CPC (ads), CPM (banners) ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system. Google Ads and others ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Campaign search condition. For example, keywords for contextual advertising ||
|| **WEB**
[`crm_multifield`](../../data-types.md) | Website. Multiple ||
|| **UF_...** | Custom fields. For example, `UF_CRM_25534736`.  

Depending on the account settings, leads may have a set of custom fields of specific types. 

To create, modify, or delete custom fields in leads, use the methods [crm.lead.userfield.*](./userfield/index.md) ||
|#

{% note info %}

Additionally, to find out the required format of fields, you can execute the method [crm.lead.fields](crm-lead-fields.md) and check the format of the incoming values of these fields. 

{% endnote %}

{% note info %}

When adding a lead, you cannot explicitly set the indicator for a duplicate lead (the `IS_RETURN_CUSTOMER` field), however, this field automatically takes the value Y if you specify a value for `COMPANY_ID` or `CONTACT_ID` when adding the lead.

{% endnote %}

## Parameter options {#options}

#|
|| **Name**
`type`  | **Description** ||
|| **REGISTER_SONET_EVENT**
[`boolean`](../../data-types.md) | Register the event of adding a lead in the activity stream. A notification will also be sent to the responsible person for the lead ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"IP Titov","NAME":"Gleb","SECOND_NAME":"Egorovich","LAST_NAME":"Titov","STATUS_ID":"NEW","OPENED":"Y","ASSIGNED_BY_ID":1,"CURRENCY_ID":"USD","OPPORTUNITY":12500,"PHONE":[{"VALUE":"555888","VALUE_TYPE":"WORK"}],"WEB":[{"VALUE":"www.mysite.com","VALUE_TYPE":"WORK"}]},"params":{"REGISTER_SONET_EVENT":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"IP Titov","NAME":"Gleb","SECOND_NAME":"Egorovich","LAST_NAME":"Titov","STATUS_ID":"NEW","OPENED":"Y","ASSIGNED_BY_ID":1,"CURRENCY_ID":"USD","OPPORTUNITY":12500,"PHONE":[{"VALUE":"555888","VALUE_TYPE":"WORK"}],"WEB":[{"VALUE":"www.mysite.com","VALUE_TYPE":"WORK"}]},"params":{"REGISTER_SONET_EVENT":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.lead.add",
        {
            fields:
            {
                TITLE: "IP Titov",
                NAME: "Gleb",
                SECOND_NAME: "Egorovich",
                LAST_NAME: "Titov",
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

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.lead.add',
        [
            'fields' => [
                'TITLE' => 'IP Titov',
                'NAME' => 'Gleb',
                'SECOND_NAME' => 'Egorovich',
                'LAST_NAME' => 'Titov',
                'STATUS_ID' => 'NEW',
                'OPENED' => 'Y',
                'ASSIGNED_BY_ID' => 1,
                'CURRENCY_ID' => 'USD',
                'OPPORTUNITY' => 12500,
                'PHONE' => [
                    [
                        'VALUE' => '555888',
                        'VALUE_TYPE' => 'WORK',
                    ],
                ],
                'WEB' => [
                    [
                        'VALUE' => 'www.mysite.com',
                        'VALUE_TYPE' => 'WORK',
                    ],
                ],
            ],
            'params' => [
                'REGISTER_SONET_EVENT' => 'Y',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 3465,
    "time": {
        "start": 1705764932.998683,
        "finish": 1705764937.173995,
        "duration": 4.1753120422363281,
        "processing": 3.3076529502868652,
        "date_start": "2024-01-20T18:35:32+01:00",
        "date_finish": "2024-01-20T18:35:37+01:00",
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
[`integer`](../../data-types.md) | Root element of the response, contains the identifier of the created lead ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

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
|| Empty value | Access denied. | The user does not have permission to add a lead ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-lead-delete.md)
- [{#T}](./crm-lead-fields.md)