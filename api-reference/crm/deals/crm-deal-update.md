# Update Deal crm.deal.update

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "modify" access permission for deals

The method `crm.deal.update` updates an existing deal.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the deal.

The identifier can be obtained using the methods [crm.deal.list](./crm-deal-list.md) or [crm.deal.add](./crm-deal-add.md) ||
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
[`object`](../../data-types.md)| Set of additional parameters ([detailed description](#params)) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../data-types.md) | Title of the deal ||
|| **TYPE_ID**
[`crm_status`](../data-types.md) | String identifier of the deal type.

The list of available deal types can be found using the method [crm.status.list](../status/crm-status-list.md) with the filter `{ ENTITY_ID: 'DEAL_TYPE' }` ||
|| **STAGE_ID**
[`crm_status`](../data-types.md) | Stage of the deal.

The list of available stages can be found using the method [crm.status.list](../status/crm-status-list.md) with the filter:
- `{ ENTITY_ID: "DEAL_STAGE" }` — if the deal is in the general Sales Funnel (direction)
- `{ ENTITY_ID: "DEAL_STAGE_{categoryId}" }` — if the deal is not in the general Sales Funnel, where `categoryId` is the identifier of the [funnel](../universal/category/index.md) and equals `CATEGORY_ID` of the deal.
  
If it is necessary to change the deal's funnel, use the method [crm.item.update](../universal/crm-item-update.md), `entityTypeId` of the deal — `2`
||
|| **IS_RECURRING**
[`char`](../../data-types.md) | Indicates whether the deal is a template for a recurring deal. Possible values:
- `Y` — yes
- `N` — no ||
|| **IS_RETURN_CUSTOMER**
[`char`](../../data-types.md) | Indicates whether the deal is a repeat. Possible values:
- `Y` — yes
- `N` — no ||
|| **IS_REPEATED_APPROACH**
[`char`](../../data-types.md) | Indicates whether the deal is a repeated approach. Possible values:
- `Y` — yes
- `N` — no
||
|| **PROBABILITY**
[`integer`](../../data-types.md) | Probability, % ||
|| **CURRENCY_ID**
[`crm_currency`](../data-types.md#crm_currency) | Currency.

The list of available currencies can be found using the method [crm.currency.list](../currency/crm-currency-list.md)
||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Amount ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Indicates whether manual calculation mode is enabled. Possible values:
- `Y` — yes
- `N` — no ||
|| **TAX_VALUE**
[`double`](../../data-types.md) | Tax amount ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the company associated with the deal.

The list of companies can be found using the method [crm.item.list](../universal/crm-item-list.md), passing `entityTypeId = 4`
||
|| **CONTACT_ID**
[`crm_contact`](../data-types.md) | Contact. Deprecated ||
|| **CONTACT_IDS**
[`crm_contact[]`](../data-types.md) | List of contacts associated with the deal.

The list of contacts can be found using the method [crm.item.list](../universal/crm-item-list.md), passing `entityTypeId = 3`
||
|| **BEGINDATE**
[`date`](../../data-types.md) | Start date ||
|| **CLOSEDATE**
[`date`](../../data-types.md) | End date ||
|| **OPENED**
[`char`](../../data-types.md) | Is the deal available to everyone. Possible values:
- `Y` — yes
- `N` — no ||
|| **CLOSED**
[`char`](../../data-types.md) | Is the deal closed. Possible values:
- `Y` — yes
- `N` — no ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comment. Supports bb-codes ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Responsible person ||
|| **SOURCE_ID**
[`crm_status`](../data-types.md) | String identifier of the source type.

The list of available sources can be found using the method [crm.status.list](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }` ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the source ||
|| **ADDITIONAL_INFO**
[`string`](../../data-types.md) | Additional information ||
|| **LOCATION_ID**
[`location`](../../data-types.md) | Client's location. System field ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the data source.

Used only for linking to an external source
||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the element in the data source.

Used only for linking to an external source
||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system (Google-Adwords and others) ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Type of traffic. Possible values:
- `CPC` — ads
- `CPM` — banners
||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Designation of the advertising campaign ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Search condition of the campaign. For example, keywords for contextual advertising ||
|| **UF_CRM_...** | Custom fields. For example, `UF_CRM_25534736`. 

Depending on the account settings, deals may have a set of custom fields of specific types. 

A custom field can be added to a deal using the method [crm.deal.userfield.add](./user-defined-fields/crm-deal-userfield-add.md) ||
|| **PARENT_ID_...**
[`crm_entity`](../data-types.md) | Relationship fields. 

If there are SPAs associated with deals in the account, there is a field for each such SPA that stores the relationship between this SPA and the deal. The field itself stores the identifier of the element of that SPA. 

For example, the field `PARENT_ID_153` — relationship with the SPA `entityTypeId=153`, stores the identifier of the element of this SPA associated with the current deal ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **REGISTER_SONET_EVENT**
[`boolean`](../../data-types.md) | Whether to register the deal change event in the activity stream. Possible values:
- `Y` — yes
- `N` — no ||
|| **REGISTER_HISTORY_EVENT**
[`boolean`](../../data-types.md) | Whether to create a record in history. Possible values:
- `Y` — yes
- `N` — no ||
|#

{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-update.md)

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":123,"FIELDS":{"TITLE":"New deal title!","TYPE_ID":"GOODS","STAGE_ID":"WON","IS_RECCURING":"Y","IS_RETURN_CUSTOMER":"Y","OPPORTUNITY":9999.99,"IS_MANUAL_OPPORTUNITY":"Y","ASSIGNED_BY_ID":1,"UF_CRM_1725365197310":"String","PARENT_ID_1032":1},"PARAMS":{"REGISTER_SONET_EVENT":"N","REGISTER_HISTORY_EVENT":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":123,"FIELDS":{"TITLE":"New deal title!","TYPE_ID":"GOODS","STAGE_ID":"WON","IS_RECCURING":"Y","IS_RETURN_CUSTOMER":"Y","OPPORTUNITY":9999.99,"IS_MANUAL_OPPORTUNITY":"Y","ASSIGNED_BY_ID":1,"UF_CRM_1725365197310":"String","PARENT_ID_1032":1},"PARAMS":{"REGISTER_SONET_EVENT":"N","REGISTER_HISTORY_EVENT":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.update
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.deal.update',
        {
            id: 123,
            fields: {
                TITLE: "New deal title!",
                TYPE_ID: "GOODS",
                STAGE_ID: "WON",
                IS_RECCURING: "Y",
                IS_RETURN_CUSTOMER: "Y",
                OPPORTUNITY: 9999.99,
                IS_MANUAL_OPPORTUNITY: "Y",
                ASSIGNED_BY_ID: 1,
                UF_CRM_1725365197310: "String",
                PARENT_ID_1032: 1,
            },
            params: {
                REGISTER_SONET_EVENT: "N",
                REGISTER_HISTORY_EVENT: "N",
            },
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.update',
        [
            'ID' => 123,
            'FIELDS' => [
                'TITLE' => 'New deal title!',
                'TYPE_ID' => 'GOODS',
                'STAGE_ID' => 'WON',
                'IS_RECCURING' => 'Y',
                'IS_RETURN_CUSTOMER' => 'Y',
                'OPPORTUNITY' => 9999.99,
                'IS_MANUAL_OPPORTUNITY' => 'Y',
                'ASSIGNED_BY_ID' => 1,
                'UF_CRM_1725365197310' => 'String',
                'PARENT_ID_1032' => 1,
            ],
            'PARAMS' => [
                'REGISTER_SONET_EVENT' => 'N',
                'REGISTER_HISTORY_EVENT' => 'N',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Method Explanation

To manage the contacts of the deal, it is recommended to use the multiple field `CONTACT_IDS`.

Example:

```js
BX24.callMethod("crm.deal.update", { id: 1, fields: { "CONTACT_IDS": [ 1, 2, 3 ] } });
```

As a result, the deal will be linked to the three specified contacts.

The field `CONTACT_ID` is deprecated and is supported for backward compatibility.

Example:

```js
BX24.callMethod("crm.deal.update", { id: 1, fields: { "CONTACT_ID": 4 } });
```

As a result of this call, a link to the specified contact will be added to the deal. 

{% note warning %}

Existing links to contacts will not be removed. If the deal was previously linked to contacts 1, 2, and 3, it will now be linked to contacts 1, 2, 3, and 4.

{% endnote %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1725365418.056843,
        "finish": 1725365419.671506,
        "duration": 1.6146628856658936,
        "processing": 1.3475170135498047,
        "date_start": "2024-09-03T14:10:18+02:00",
        "date_finish": "2024-09-03T14:10:19+02:00",
        "operating": 0
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

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'fields' must be array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | `ID is not defined or invalid` | The parameter `id` is not an integer greater than zero ||
|| `-`     | `Not found` | The deal with the provided `id` does not exist ||
|| `-`     | `Parameter 'fields' must be array` | The parameter `fields` is not an object ||
|| `-`     | `Parameter 'params' must be array` | The parameter `params` is not an object ||
|| `-`     | `Access denied` | The user does not have permission to "modify" deals ||
|| `-`     | Disk resource exhausted |> ||
|| `-`     | Invalid value for the "Currency" field |> ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-add.md)
- [{#T}](./crm-deal-get.md)
- [{#T}](./crm-deal-list.md)
- [{#T}](./crm-deal-delete.md)
- [{#T}](./crm-deal-fields.md)
- [{#T}](../universal/crm-item-update.md)