# Create a New Deal crm.deal.add

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with the "add" access permission for deals

The method `crm.deal.add` creates a new deal.

## Method Parameters

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
|| **params**
[`object`](../../data-types.md) | Object containing an additional set of parameters ([detailed description](#params)) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../data-types.md) | Deal title.

By default, it is generated using the template `Deal #{id}`, where `id` is the element identifier
||
|| **TYPE_ID**
[`crm_status`](../data-types.md) | String identifier of the deal type. 

The list of available deal types can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `{ ENTITY_ID: 'DEAL_TYPE' }`.

By default — the first available deal type
||
|| **CATEGORY_ID**
[`crm_category`](../data-types.md) | Identifier of the sales funnel. Must be greater than or equal to 0.

The list of available funnels can be obtained using the method [crm.category.list](../universal/category/crm-category-list.md) by passing `entityTypeId = 2`.

By default — the identifier of the default funnel
||
|| **STAGE_ID**
[`crm_status`](../data-types.md) | Stage of the deal. 

The list of available stages can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter:
- `{ ENTITY_ID: "DEAL_STAGE" }` — if the deal is in the general funnel (direction)
- `{ ENTITY_ID: "DEAL_STAGE_{categoryId}" }` — if the deal is not in the general funnel, where `categoryId` is the identifier of the [funnel](../universal/category/index.md) of the deal  

By default — the first available stage relative to the funnel ||
|| **IS_RECURRING**
[`char`](../../data-types.md) | Indicates whether the deal is a template for a recurring deal. Possible values:
- `Y` — yes
- `N` — no

By default `N`
||
|| **IS_RETURN_CUSTOMER**
[`char`](../../data-types.md) | Indicates whether the deal is a repeat. Possible values:
- `Y` — yes
- `N` — no

By default `N`
||
|| **IS_REPEATED_APPROACH**
[`char`](../../data-types.md) | Indicates whether the deal is a repeated approach. Possible values:
- `Y` — yes
- `N` — no

By default `N`
||
|| **PROBABILITY**
[`integer`](../../data-types.md) | Probability, % ||
|| **CURRENCY_ID**
[`crm_currency`](../data-types.md#crm_currency) | Currency. 

The list of available currencies can be obtained using the method [crm.currency.list](../currency/crm-currency-list.md)
||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Amount. 

By default `0.00` ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Indicates whether manual calculation of the amount is enabled. Possible values:
- `Y` — yes
- `N` — no

By default `N`
||
|| **TAX_VALUE**
[`double`](../../data-types.md) | Tax amount.

By default `0.00`
||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the company associated with the deal.

The list of companies can be obtained using the method [crm.item.list](../universal/crm-item-list.md) by passing `entityTypeId = 4`
||
|| **CONTACT_ID**
[`crm_contact`](../data-types.md) | Contact. Deprecated ||
|| **CONTACT_IDS**
[`crm_contact[]`](../data-types.md) | List of contacts associated with the deal.

The list of contacts can be obtained using the method [crm.item.list](../universal/crm-item-list.md) by passing `entityTypeId = 3`
||
|| **BEGINDATE**
[`date`](../../data-types.md) | Start date. 

By default — the date the deal is created ||
|| **CLOSEDATE**
[`date`](../../data-types.md) | Completion date. 

By default — the date the deal is created plus 7 days
||
|| **OPENED**
[`char`](../../data-types.md) | Is the deal available to everyone. Possible values:
- `Y` — yes
- `N` — no

By default `Y`. The default value can be changed in the CRM settings
||
|| **CLOSED**
[`char`](../../data-types.md) | Is the deal closed. Possible values:
- `Y` — yes
- `N` — no

By default `N`
||
|| **COMMENTS**
[`string`](../../data-types.md) | Comment. Supports bb-codes ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Responsible person. 

By default — the user calling this method
||
|| **SOURCE_ID**
[`crm_status`](../data-types.md) | String identifier of the source type. 

The list of available sources can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

By default — the first available source type
||
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
[`string`](../../data-types.md) | Identifier of the advertising campaign ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Search condition of the campaign. For example, keywords for contextual advertising ||
|| **TRACE**
[`string`](../../data-types.md) | Information for Sales Intelligence — read more in the article [{#T}](../../../tutorials/crm/how-to-use-analitycs/info-to-analitics.md) ||
|| **UF_CRM_...** | Custom fields. For example, `UF_CRM_25534736`. 

Depending on the portal settings, deals may have a set of custom fields of specific types. 

You can add a custom field to a deal using the method [crm.deal.userfield.add](./user-defined-fields/crm-deal-userfield-add.md) ||
|| **PARENT_ID_...**
[`crm_entity`](../data-types.md) | Relationship fields. 

If there are SPAs related to deals on the portal, each such SPA has a field that stores the relationship between this SPA and the deal. The field itself stores the identifier of the element of that SPA. 

For example, the field `PARENT_ID_153` — relationship with the SPA `entityTypeId=153`, stores the identifier of the element of this SPA related to the current deal ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **REGISTER_SONET_EVENT** 
[`boolean`](../../data-types.md) | Should the event of adding a deal be registered in the live feed. Possible values:
- `Y` — yes
- `N` — no

By default `Y` ||
|#

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FIELDS":{"TITLE":"New Deal #1","TYPE_ID":"COMPLEX","CATEGORY_ID":0,"STAGE_ID":"PREPARATION","IS_RECURRING":"N","IS_RETURN_CUSTOMER":"Y","IS_REPEATED_APPROACH":"Y","PROBABILITY":99,"CURRENCY_ID":"EUR","OPPORTUNITY":1000000,"IS_MANUAL_OPPORTUNITY":"Y","TAX_VALUE":0.10,"COMPANY_ID":9,"CONTACT_IDS":[84,83],"BEGINDATE":"'"$(date --iso-8601=seconds)"'","CLOSEDATE":"'"$(date --iso-8601=seconds --date='+10 days')"'", "OPENED":"Y","CLOSED":"N","COMMENTS":"Example comment","SOURCE_ID":"CALLBACK","SOURCE_DESCRIPTION":"Additional information about the source","ADDITIONAL_INFO":"Additional information","UTM_SOURCE":"google","UTM_MEDIUM":"CPC","PARENT_ID_1220":22,"UF_CRM_1721244482250":"Hello World!"},"PARAMS":{"REGISTER_SONET_EVENT":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FIELDS":{"TITLE":"New Deal #1","TYPE_ID":"COMPLEX","CATEGORY_ID":0,"STAGE_ID":"PREPARATION","IS_RECURRING":"N","IS_RETURN_CUSTOMER":"Y","IS_REPEATED_APPROACH":"Y","PROBABILITY":99,"CURRENCY_ID":"EUR","OPPORTUNITY":1000000,"IS_MANUAL_OPPORTUNITY":"Y","TAX_VALUE":0.10,"COMPANY_ID":9,"CONTACT_IDS":[84,83],"BEGINDATE":"'"$(date --iso-8601=seconds)"'","CLOSEDATE":"'"$(date --iso-8601=seconds --date='+10 days')"'", "OPENED":"Y","CLOSED":"N","COMMENTS":"Example comment","SOURCE_ID":"CALLBACK","SOURCE_DESCRIPTION":"Additional information about the source","ADDITIONAL_INFO":"Additional information","UTM_SOURCE":"google","UTM_MEDIUM":"CPC","PARENT_ID_1220":22,"UF_CRM_1721244482250":"Hello World!"},"PARAMS":{"REGISTER_SONET_EVENT":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.add
    ```

- JS

    ```js
    const day = 60 * 60 * 24 * 1000;
  
    const now = new Date();
    const after10Days = new Date(now.getTime() + 10 * day);

    BX24.callMethod(
        'crm.deal.add',
        {
            fields: {
                TITLE: "New Deal #1",
                TYPE_ID: "COMPLEX",
                CATEGORY_ID: 0,
                STAGE_ID: "PREPARATION",
                IS_RECURRING: "N",
                IS_RETURN_CUSTOMER: "Y",
                IS_REPEATED_APPROACH: "Y",
                PROBABILITY: 99,
                CURRENCY_ID: "EUR",
                OPPORTUNITY: 1000000,
                IS_MANUAL_OPPORTUNITY: "Y",
                TAX_VALUE: 0.10,
                COMPANY_ID: 9,
                CONTACT_IDS: [84, 83],
                BEGINDATE: now.toISOString(),
                CLOSEDATE: after10Days.toISOString(),
                OPENED: "Y",
                CLOSED: "N",
                COMMENTS: "[B]Example comment[/B]",
                SOURCE_ID: "CALLBACK",
                SOURCE_DESCRIPTION: "Additional information about the source",
                ADDITIONAL_INFO: "Additional information",
                UTM_SOURCE: "google",
                UTM_MEDIUM: "CPC",
                PARENT_ID_1220: 22,
                UF_CRM_1721244482250: "Hello World!",
            },
            params: {
                REGISTER_SONET_EVENT: "N",
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
        'crm.deal.add',
        [
            'FIELDS' => [
                'TITLE' => 'New Deal #1',
                'TYPE_ID' => 'COMPLEX',
                'CATEGORY_ID' => 0,
                'STAGE_ID' => 'PREPARATION',
                'IS_RECURRING' => 'N',
                'IS_RETURN_CUSTOMER' => 'Y',
                'IS_REPEATED_APPROACH' => 'Y',
                'PROBABILITY' => 99,
                'CURRENCY_ID' => 'EUR',
                'OPPORTUNITY' => 1000000,
                'IS_MANUAL_OPPORTUNITY' => 'Y',
                'TAX_VALUE' => 0.10,
                'COMPANY_ID' => 9,
                'CONTACT_IDS' => [84, 83],
                'BEGINDATE' => (new DateTime())->format(DateTime::ATOM),
                'CLOSEDATE' => (new DateTime('+10 days'))->format(DateTime::ATOM),
                'OPENED' => 'Y',
                'CLOSED' => 'N',
                'COMMENTS' => 'Example comment',
                'SOURCE_ID' => 'CALLBACK',
                'SOURCE_DESCRIPTION' => 'Additional information about the source',
                'ADDITIONAL_INFO' => 'Additional information',
                'UTM_SOURCE' => 'google',
                'UTM_MEDIUM' => 'CPC',
                'PARENT_ID_1220' => 22,
                'UF_CRM_1721244482250' => 'Hello World!',
            ],
            'PARAMS' => [
                'REGISTER_SONET_EVENT' => 'N',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)
  
    ```php        
    try {
        $fields = [
            'TITLE' => 'New Deal',
            'TYPE_ID' => 'GIG',
            'CATEGORY_ID' => '1',
            'STAGE_ID' => 'C1:NEW',
            'CURRENCY_ID' => 'USD',
            'OPPORTUNITY' => '10000',
            'BEGINDATE' => (new DateTime())->format(DateTime::ATOM),
            'CLOSEDATE' => (new DateTime('+1 month'))->format(DateTime::ATOM),
            'COMMENTS' => 'This is a test deal.',
        ];
        $params = [
            'REGISTER_SONET_EVENT' => 'Y',
        ];
        $result = $serviceBuilder
            ->getCRMScope()
            ->deal()
            ->add($fields, $params);
        print($result->getId());
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 394,
    "time": {
        "start": 1725013197.635808,
        "finish": 1725013198.580873,
        "duration": 0.9450650215148926,
        "processing": 0.6822988986968994,
        "date_start": "2024-08-30T12:19:57+02:00",
        "date_finish": "2024-08-30T12:19:58+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Root element of the response, contains the identifier of the created deal ||
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
|| `-`     | `Parameter 'fields' must be array` | The parameter `fields` is not an object ||
|| `-`     | `Parameter 'params' must be array` | The parameter `params` is not an object ||
|| `-`     | `Access denied` | The user does not have permission to "add" deals ||
|| `-`     | Disk resource exhausted |> ||
|| `-`     | Invalid value for the "Currency" field |> ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-update.md)
- [{#T}](./crm-deal-get.md)
- [{#T}](./crm-deal-list.md)
- [{#T}](./crm-deal-delete.md)
- [{#T}](./crm-deal-fields.md)