# Get Deal by Id crm.deal.get

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "read" access permission for deals

{% note warning "Method Development Stopped" %}

The method `crm.deal.get` continues to function, but there is a more relevant alternative [crm.item.get](../universal/crm-item-get.md).

{% endnote %}

The method `crm.deal.get` returns a deal by its identifier.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** || 
|| **id***
[`integer`](../../data-types.md) | Identifier of the deal.

The identifier can be obtained using the methods [crm.deal.list](./crm-deal-list.md) or [crm.deal.add](./crm-deal-add.md) ||
|#

{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-get.md)

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":410}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":410,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.get',
    		{
    			id: 410,
    		}
    	);
    	
    	const result = response.getData().result;
    	result.error()
    		? console.error(result.error())
    		: console.info(result)
    	;
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.get',
                [
                    'id' => 410,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Deal data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting deal: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.get',
        {
            id: 410,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.get',
        [
            'ID' => 410
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
        "ID": "410",
        "TITLE": "New Deal #1",
        "TYPE_ID": "COMPLEX",
        "STAGE_ID": "PREPARATION",
        "PROBABILITY": "99",
        "CURRENCY_ID": "EUR",
        "OPPORTUNITY": "1000000.00",
        "IS_MANUAL_OPPORTUNITY": "Y",
        "TAX_VALUE": "0.00",
        "LEAD_ID": null,
        "COMPANY_ID": "9",
        "CONTACT_ID": "84",
        "QUOTE_ID": null,
        "BEGINDATE": "2024-08-30T02:00:00+02:00",
        "CLOSEDATE": "2024-09-09T02:00:00+02:00",
        "ASSIGNED_BY_ID": "1",
        "CREATED_BY_ID": "1",
        "MODIFY_BY_ID": "1",
        "DATE_CREATE": "2024-08-30T14:29:00+02:00",
        "DATE_MODIFY": "2024-08-30T14:29:00+02:00",
        "OPENED": "Y",
        "CLOSED": "N",
        "COMMENTS": "[B]Example comment[\/B]",
        "ADDITIONAL_INFO": "Additional information",
        "LOCATION_ID": null,
        "CATEGORY_ID": "0",
        "STAGE_SEMANTIC_ID": "P",
        "IS_NEW": "N",
        "IS_RECURRING": "N",
        "IS_RETURN_CUSTOMER": "N",
        "IS_REPEATED_APPROACH": "N",
        "SOURCE_ID": "CALLBACK",
        "SOURCE_DESCRIPTION": "Additional information about the source",
        "ORIGINATOR_ID": null,
        "ORIGIN_ID": null,
        "MOVED_BY_ID": "1",
        "MOVED_TIME": "2024-08-30T14:29:00+02:00",
        "LAST_ACTIVITY_TIME": "2024-08-30T14:29:00+02:00",
        "UTM_SOURCE": "google",
        "UTM_MEDIUM": "CPC",
        "UTM_CAMPAIGN": null,
        "UTM_CONTENT": null,
        "UTM_TERM": null,
        "PARENT_ID_1220": "22",
        "LAST_ACTIVITY_BY": "1",
        "UF_CRM_1721244482250": "Hello world!"
    },
    "time": {
        "start": 1725020945.541275,
        "finish": 1725020946.179076,
        "duration": 0.637800931930542,
        "processing": 0.21427488327026367,
        "date_start": "2024-08-30T14:29:05+02:00",
        "date_finish": "2024-08-30T14:29:06+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`deal`](#deal) | Root element of the response. Contains information about the deal fields. The structure is described [below](#deal) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Type deal {#deal}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the deal ||
|| **TITLE**
[`string`](../../data-types.md) | Title ||
|| **TYPE_ID**
[`crm_status`](../data-types.md) | String identifier of the deal type. 

To learn more about the obtained deal type, you can use the method [crm.status.list](../status/crm-status-list.md), passing the filter:

```
{
    ENTITY_ID: 'DEAL_TYPE',
    STATUS_ID: TYPE_ID,
}
```
||
|| **CATEGORY_ID**
[`crm_category`](../data-types.md) | Funnel. To learn more about this funnel, you can use the method [crm.category.get](../universal/category/crm-category-get.md), passing `entityTypeId = 2` and `id = CATEGORY_ID` ||
|| **STAGE_ID**
[`crm_status`](../data-types.md) | String identifier of the deal stage. 

To learn more about the obtained stage, you can use the method [crm.status.list](../status/crm-status-list.md), passing the filter:

```
{
    ENTITY_ID: entityId,
    STATUS_ID: statusId,
}
```

where:
- `entityId` is equal to:
    - `DEAL_STAGE` if the deal is in the general funnel (`CATEGORY_ID = 0`)
    - `DEAL_STAGE_{categoryId}`, where `categoryId = CATEGORY_ID`
- `statusId` is equal to `STAGE_ID`
||
|| **STAGE_SEMANTIC_ID**
[`string`](../../data-types.md) | Stage group. Possible values:
- `P` — in progress
- `S` — successful
- `F` — unsuccessful
||
|| **IS_NEW**
[`char`](../../data-types.md) | Indicates whether the deal is new. Possible values:
- `Y` — yes
- `N` — no ||
|| **IS_RECURRING**
[`char`](../../data-types.md) | Indicates whether the deal is recurring. Possible values:
- `Y` — yes
- `N` — no ||
|| **IS_RETURN_CUSTOMER**
[`char`](../../data-types.md) | Indicates whether the deal is a repeat. Possible values:
- `Y` — yes
- `N` — no ||
|| **IS_REPEATED_APPROACH**
[`char`](../../data-types.md) | Indicates whether the approach is repeated. Possible values:
- `Y` — yes
- `N` — no ||
|| **PROBABILITY**
[`integer`](../../data-types.md) | Probability, % ||
|| **CURRENCY_ID**
[`crm_currency`](../data-types.md#crm_currency) | Currency ||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Amount ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Indicates whether manual mode for calculating the amount is enabled. Possible values:
- `Y` — yes
- `N` — no ||
|| **TAX_VALUE**
[`double`](../../data-types.md) | Tax rate ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the company. 

To learn more about the company, you can use the method [crm.item.get](../universal/crm-item-get.md), passing `entityTypeId = 4` and `id = COMPANY_ID`
||
|| **CONTACT_ID**
[`crm_contact`](../data-types.md) | Identifier of the contact. Deprecated. 

To get a list of all contacts associated with the deal, use the method [crm.deal.contact.items.get](./contacts/crm-deal-contact-items-get.md) or the universal method [crm.item.get](../universal/crm-item-get.md) ||
|| **QUOTE_ID**
[`crm_quote`](../data-types.md) | Identifier of the estimate based on which the deal was created. 

To learn more about the estimate, you can use the method [crm.item.get](../universal/crm-item-get.md), passing `entityTypeId = 7` and `id = QUOTE_ID`
||
|| **BEGINDATE**
[`date`](../../data-types.md) | Start date ||
|| **CLOSEDATE**
[`date`](../../data-types.md) | Close date ||
|| **OPENED**
[`char`](../../data-types.md) | Indicates whether the deal is available to everyone. Possible values:
- `Y` — yes
- `N` — no ||
|| **CLOSED**
[`char`](../../data-types.md) | Indicates whether the deal is closed. Possible values:
- `Y` — yes
- `N` — no ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comment ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Responsible person ||
|| **CREATED_BY_ID**
[`user`](../../data-types.md) | Created by ||
|| **MODIFY_BY_ID**
[`user`](../../data-types.md) | Modified by ||
|| **MOVED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user who last changed the stage ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Creation date ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Modification date ||
|| **MOVED_TIME**
[`datetime`](../../data-types.md) | Date of the last stage change ||
|| **SOURCE_ID**
[`crm_status`](../data-types.md) | Source. 

To learn more about the obtained source, you can use the method [crm.status.list](../status/crm-status-list.md), passing the filter:

```
{
    ENTITY_ID: 'SOURCE',
    STATUS_ID: SOURCE_ID,
}
```
||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the source ||
|| **LEAD_ID**
[`crm_lead`](../data-types.md) | Identifier of the lead based on which the deal was created. 

To learn more about the lead, you can use the method [crm.item.get](../universal/crm-item-get.md), passing `entityTypeId = 1` and `id = LEAD_ID`
||
|| **ADDITIONAL_INFO**
[`string`](../../data-types.md) | Additional information ||
|| **LOCATION_ID**
[`location`](../../data-types.md) | Location. System field ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | External source ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the element in the external source ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Traffic type ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Advertising campaign designation ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Content of the campaign ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Search condition of the campaign ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../../data-types.md) | Date of the last activity in the timeline ||
|| **LAST_ACTIVITY_BY**
[`user`](../../data-types.md) | Author of the last activity in the timeline ||
|| **UF_CRM_...**
[`any`](../../data-types.md) | User-defined fields. For example, `UF_CRM_25534736`. 

Depending on the portal settings, deals may have a set of user-defined fields of specific types. Read more in the section [about user-defined fields](./user-defined-fields/index.md) ||
|| **PARENT_ID_...**
[`crm_entity`](../data-types.md) | Relationship fields. 

If there are smart processes associated with deals on the portal, for each such smart process, there is a field that stores the relationship between this smart process and the deal. The field itself stores the identifier of the element of that smart process. 

For example, the field `PARENT_ID_153` — relationship with the smart process `entityTypeId=153`, stores the identifier of the element of this smart process associated with the current deal ||
|#

## Error Handling

HTTP Status: **400**

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
|| `-`     | `ID is not defined or invalid` | The `id` parameter either has no value or is not a positive integer ||
|| `-`     | `Access denied` | The user does not have permission to "read" this deal ||
|| `-`     | `Not found`      | No deal exists with the provided `id` ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-add.md)
- [{#T}](./crm-deal-update.md)
- [{#T}](./crm-deal-list.md)
- [{#T}](./crm-deal-delete.md)
- [{#T}](./crm-deal-fields.md)