# Update Lead crm.lead.update

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to edit CRM leads

The method `crm.lead.update` updates an existing lead.

## Method Parameters

{% note warning %}

It is strongly recommended to pass the complete set of address fields when updating the address in the update method. The specifics of updating address fields are described [here](../data-types.md).

{% endnote %}

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Lead identifier.

The identifier can be obtained using the methods [crm.lead.list](./crm-lead-list.md) or [crm.lead.add](./crm-lead-add.md) ||
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
- `value_n` — new field value

The list of available fields is described [below](#fields).

An incorrect field in `fields` will be ignored.

Only those fields that need to be changed should be passed in `fields`
||
|| **params** 
[`object`](../../data-types.md)|Optional set of options. (`"paramName"=>"value"[, ...]`). The list of possible fields is described [below](#params) ||
|#

## Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ADDRESS**
[`string`](../../data-types.md) | Lead address ||
|| **ADDRESS_2**
[`string`](../../data-types.md) | Second line of the address. In some countries, it is customary to split the address into 2 parts ||
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

Contacts can be added or removed using the group of methods [crm.lead.contact.*](./management-communication/index.md)||
|| **CURRENCY_ID**
[`crm_currency`](../../data-types.md) | Currency identifier ||
|| **EMAIL**
[`crm_multifield`](../../data-types.md) | Email address. Multiple ||
|| **HONORIFIC**
[`crm_status`](../../data-types.md) | Salutation ||
|| **IM**
[`crm_multifield`](../../data-types.md) | Messenger. Multiple ||
|| **LINK**
[`crm_multifield`](../../data-types.md) | User ID linked through an open channel. Multiple ||
|| **LAST_NAME**
[`string`](../../data-types.md) | Last name ||
|| **NAME**
[`string`](../../data-types.md) | First name ||
|| **SECOND_NAME**
[`string`](../../data-types.md) | Middle name ||
|| **OPENED**
[`char`](../../data-types.md) | Indicates the availability of the lead for everyone. Acceptable values Y or N ||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Amount ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Indicates manual mode for calculating the amount. Acceptable values Y or N||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the data source. Used only for linking to an external source ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the item in the data source. Used only for linking to an external source ||
|| **PHONE**
[`crm_multifield`](../../data-types.md) | Phone. Multiple ||
|| **POST**
[`string`](../../data-types.md) | Position ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Description of the source ||
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

The list of all possible identifiers from the directory can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `filter[ENTITY_ID]=SOURCE` ||
|| **STATUS_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the stage ||
|| **STATUS_ID**
[`crm_status`](../../data-types.md) | Identifier of the lead stage. Default stages:

#|
||STATUS_ID|Name||
||NEW | Not processed||
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
[`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Type of traffic. CPC (ads), CPM (banners) ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system. Google-Adwords and others ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Search condition of the campaign. For example, keywords for contextual advertising ||
|| **WEB**
[`crm_multifield`](../../data-types.md) | Website. Multiple ||
|| **UF_...** | Custom fields. For example, `UF_CRM_25534736`.  

Depending on the account settings, leads may have a set of custom fields of defined types. 

To change file fields, it is recommended to use the method [crm.item.update](../universal/crm-item-update.md).

To create, modify, or delete custom fields in leads, use the methods [crm.lead.userfield.*](./userfield/index.md) ||
|#

{% note info %}

Additionally, to find out the required format of fields, you can execute the method [crm.lead.fields](./crm-lead-fields.md) and check the format of the incoming values of these fields.

{% endnote %}

{% note info %}

When changing a lead, you cannot explicitly set the repeat lead indicator (the `IS_RETURN_CUSTOMER` field), however, this field automatically takes the value Y if you specify a value for `COMPANY_ID` or `CONTACT_ID` when changing the lead.

{% endnote %}

## Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **REGISTER_SONET_EVENT**
[`char`](../../data-types.md) | Register the event of adding a lead in the activity stream. Additionally, a notification will be sent to the person responsible for the lead. Acceptable values `Y` or `N` ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)
  
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1608,"fields":{"TITLE":"LLC Titov","NAME":"Gleb","SECOND_NAME":"Egorovich","LAST_NAME":"Titov","STATUS_ID":"NEW","OPENED":"Y","ASSIGNED_BY_ID":1,"CURRENCY_ID":"USD","OPPORTUNITY":12500,"PHONE":[{"VALUE":"555888","VALUE_TYPE":"WORK"}],"WEB":[{"VALUE":"www.mysite.com","VALUE_TYPE":"WORK"}]},"params":{"REGISTER_SONET_EVENT":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1608,"fields":{"TITLE":"LLC Titov","NAME":"Gleb","SECOND_NAME":"Egorovich","LAST_NAME":"Titov","STATUS_ID":"NEW","OPENED":"Y","ASSIGNED_BY_ID":1,"CURRENCY_ID":"USD","OPPORTUNITY":12500,"PHONE":[{"VALUE":"555888","VALUE_TYPE":"WORK"}],"WEB":[{"VALUE":"www.mysite.com","VALUE_TYPE":"WORK"}]},"params":{"REGISTER_SONET_EVENT":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.update
    ```

- JS

    ```javascript
    BX24.callMethod(
        "crm.lead.update",
        {
            id: 1608,
            fields:
            {
                TITLE: "LLC Titov",
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
                ],
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
                console.error(result.error());return;
            }
    
            console.info(result.data());
        }
    );
    ```

- PHP

    ```php
    $fields = [
        'TITLE' => $sTitle,
        'COMPANY_ID' => 123,
        'PHONE' => [
            [
                'VALUE' => '555888',
                'VALUE_TYPE' => 'WORK',
            ],
        ],
    ];
    
    $result = CRest::call(
        'crm.lead.update',
        [
            'id' => 1608,
            'fields' => $fields,
        ],
        [
            'REGISTER_SONET_EVENT' => 'Y',
        ]     
    );
    ```

- PHP (B24PhpSdk)

    ```php        
    try {
        $id = 123; // Example lead ID
        $fields = [
            'TITLE' => 'Updated Lead Title',
            'NAME' => 'John',
            'LAST_NAME' => 'Doe',
            'BIRTHDATE' => (new DateTime('1980-01-01'))->format(DateTime::ATOM),
            'COMPANY_TITLE' => 'Example Company',
            'STATUS_ID' => 'NEW',
            'COMMENTS' => 'Updated comments for the lead.',
            'PHONE' => '1234567890',
            'EMAIL' => 'john.doe@example.com',
        ];
        $params = [
            'REGISTER_SONET_EVENT' => 'Y',
        ];
        $result = $serviceBuilder->getCRMScope()->lead()->update($id, $fields, $params);
        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print("Update failed.");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
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
[`boolean`](../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

> HTTP status: 40x, 50x Error

```json
{
    "error": "",
    "error_description": "ID is not defined or invalid."
}
```

### Possible Errors

#|  
|| **Error Text** | **Description** ||
|| `ID is not defined or invalid` | The parameter `id` is not a positive integer ||
|| `Not found` | The lead with the provided `id` does not exist ||
|| `Access denied` | The user does not have permission to edit the lead ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}