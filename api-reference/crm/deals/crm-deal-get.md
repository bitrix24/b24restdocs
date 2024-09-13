# Get deal by Id crm.deal.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

> Method name: **crm.deal.get**
> 
> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "read" access permission for deals.

The method `crm.deal.get` returns a deal by its identifier.


## Method parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** || 
|| **id^*^**
[`integer`](../../data-types.md) | Deal identifier

Can be obtained using the method [`crm.deal.list`](crm-deal-list.md) or [`crm.deal.add`](crm-deal-add.md) ||
|#

{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-get.md)

{% endnote %}


## Code examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    todo
    ```

- cURL (OAuth)

    ```bash
    todo
    ```

- JS

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

- PHP

    ```php
    todo
    ```

{% endlist %}


## Response handling

HTTP status: **200**

```json
{
  "result": {
    "ID": "410",
    "TITLE": "New deal #1",
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

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`deal`](#deal) | Root element of the response. Contains information about the deal fields. The structure is described [below](#deal) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Deal type {#deal}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Deal identifier ||
|| **TITLE**
[`string`](../../data-types.md) | Title ||
|| **TYPE_ID**
[`crm_status`](../data-types.md) | String identifier of the deal type 

You can learn more about the obtained deal type using [`crm.status.list`](../status/crm-status-list.md) by passing in the filter
```
{
    ENTITY_ID: 'DEAL_TYPE',
    STATUS_ID: TYPE_ID,
}
```
||
|| **CATEGORY_ID**
[`crm_category`](../data-types.md) | Sales Funnel. You can learn more about this funnel using [`crm.category.get`](../universal/category/crm-category-get.md) by passing `entityTypeId = 2` and `id = CATEGORY_ID` ||
|| **STAGE_ID**
[`crm_status`](../data-types.md) | String identifier of the deal stage. 

You can learn more about the obtained stage using [`crm.status.list`](../status/crm-status-list.md) by passing in the filter
```
{
    ENTITY_ID: entityId,
    STATUS_ID: statusId,
}
```

where:
* `entityId` is:
  * `DEAL_STAGE` if the deal is in the general funnel (`CATEGORY_ID = 0`)
  * `DEAL_STAGE_{categoryId}`, where `categoryId = CATEGORY_ID`
* `statusId` is `STAGE_ID`

||
|| **STAGE_SEMANTIC_ID**
[`string`](../../data-types.md) | Stage group 

Possible values:
- `P` — in progress
- `S` — successful
- `F` — unsuccessful

||
|| **IS_NEW**
[`char`](../../data-types.md) | Is the deal new (`Y` - Yes / `N` - No) ||
|| **IS_RECURRING**
[`char`](../../data-types.md) | Recurring deal (`Y` - Yes / `N` - No) ||
|| **IS_RETURN_CUSTOMER**
[`char`](../../data-types.md) | Repeat deal (`Y` - Yes / `N` - No) ||
|| **IS_REPEATED_APPROACH**
[`char`](../../data-types.md) | Repeat approach (`Y` - Yes / `N` - No) ||
|| **PROBABILITY**
[`integer`](../../data-types.md) | Probability, % ||
|| **CURRENCY_ID**
[`crm_currency`](../data-types.md#crm_currency) | Currency ||
|| **OPPORTUNITY**
[`double`](../../data-types.md) | Amount ||
|| **IS_MANUAL_OPPORTUNITY**
[`char`](../../data-types.md) | Is manual mode for calculating the amount enabled (`Y` - Yes / `N` - No) ||
|| **TAX_VALUE**
[`double`](../../data-types.md) | Tax rate ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Company identifier 

You can learn more about the company using the method [`crm.item.get`](../universal/crm-item-get.md) by passing `entityTypeId = 4` and `id = COMPANY_ID`
||
|| **CONTACT_ID**
[`crm_contact`](../data-types.md) | Contact identifier. Deprecated ||
|| **CONTACT_IDS**
[`crm_contact[]`](../data-types.md) | List of contact identifiers 

You can learn more about the list of contacts using the method [`crm.item.list`](../universal/crm-item-list.md) by passing `entityTypeId = 3` and filter `{ '@id': CONTACT_IDS }`
||
|| **QUOTE_ID**
[`crm_quote`](../data-types.md) | Identifier of the estimate based on which the deal was created 

You can learn more about the estimate using the method [`crm.item.get`](../universal/crm-item-get.md) by passing `entityTypeId = 7` and `id = QUOTE_ID`
||
|| **BEGINDATE**
[`date`](../../data-types.md) | Start date ||
|| **CLOSEDATE**
[`date`](../../data-types.md) | Close date ||
|| **OPENED**
[`char`](../../data-types.md) | Available to everyone (`Y` - Yes / `N` - No) ||
|| **CLOSED**
[`char`](../../data-types.md) | Closed (`Y` - Yes / `N` - No) ||
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
[`crm_status`](../data-types.md) | Source 

You can learn more about the obtained source using [`crm.status.list`](../status/crm-status-list.md) by passing in the filter
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
[`crm_lead`](../data-types.md) | Identifier of the lead based on which the deal was created 

You can learn more about the lead using the method [`crm.item.get`](../universal/crm-item-get.md) by passing `entityTypeId = 1` and `id = LEAD_ID`
||
|| **ADDITIONAL_INFO**
[`string`](../../data-types.md) | Additional information ||
|| **LOCATION_ID**
[`location`](../../data-types.md) | Location. System field ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | External source ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the item in the external source ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Traffic type ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Advertising campaign designation ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Campaign content ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Campaign search term ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../../data-types.md) | Date of the last activity in the timeline. ||
|| **LAST_ACTIVITY_BY**
[`user`](../../data-types.md) | Author of the last activity in the timeline. ||
|| **UF_CRM_...**
[`any`](../../data-types.md) | User-defined fields. For example, `UF_CRM_25534736`. Depending on the account settings, deals may have a set of user-defined fields of specific types. [Learn more](user-defined-fields/index.md) ||
|| **PARENT_ID_...**
[`crm_entity`](../data-types.md) | Relationship fields. If there are SPAs related to deals in the account, for each such SPA there is a field that stores the relationship between this SPA and the deal. The field itself stores the identifier of the item of that SPA. For example, the field `PARENT_ID_153` - relationship with the SPA `entityTypeId=153`, stores the identifier of the item of that SPA related to the current deal. ||
|#


## Error handling

HTTP status: **400**

```json
{
	"error": "",
	"error_description": "Parameter 'fields' must be array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | ID is not defined or invalid. | The `id` parameter either has no value or is not a positive integer. ||
|| `-`     | Access denied. | The user does not have permission to "read" this deal. ||
|| `-`     | Not found      | The deal with the provided `id` does not exist. ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}


## Continue exploring

TODO