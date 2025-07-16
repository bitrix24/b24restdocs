# Get CRM Status Entity Types

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.status.entity.types` returns a list of all supported types of directories, `ENTITY_ID` objects.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.status.entity.types",
        {},
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.status.entity.types
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.status.entity.types
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.status.entity.types',
        []
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
  "result": [
    {
      "ID": "STATUS",
      "NAME": "Lead Stages",
      "SEMANTIC_INFO": {
        "START_FIELD": "NEW",
        "FINAL_SUCCESS_FIELD": "CONVERTED",
        "FINAL_UNSUCCESS_FIELD": "JUNK",
        "FINAL_SORT": 0
      },
      "ENTITY_TYPE_ID": 1
    },
    {
      "ID": "SOURCE",
      "NAME": "Sources"
    },
    {
      "ID": "CONTACT_TYPE",
      "NAME": "Contact Type"
    },
    {
      "ID": "COMPANY_TYPE",
      "NAME": "Company Type"
    },
    {
      "ID": "EMPLOYEES",
      "NAME": "Number of Employees"
    },
    {
      "ID": "INDUSTRY",
      "NAME": "Industry"
    },
    {
      "ID": "DEAL_TYPE",
      "NAME": "Deal Type"
    },
    {
      "ID": "SMART_INVOICE_STAGE_5",
      "NAME": "Invoice Stages",
      "SEMANTIC_INFO": [],
      "PREFIX": "DT31_5",
      "FIELD_ATTRIBUTE_SCOPE": "category_5",
      "ENTITY_TYPE_ID": 31,
      "IS_ENABLED": true,
      "CATEGORY_ID": 5
    },
    {
      "ID": "DEAL_STAGE_1",
      "NAME": "Newest Deal Stages",
      "PARENT_ID": "DEAL_STAGE",
      "SEMANTIC_INFO": {
        "START_FIELD": "C1:NEW",
        "FINAL_SUCCESS_FIELD": "C1:WON",
        "FINAL_UNSUCCESS_FIELD": "C1:LOSE",
        "FINAL_SORT": 0
      },
      "PREFIX": "C1",
      "FIELD_ATTRIBUTE_SCOPE": "category_1",
      "ENTITY_TYPE_ID": 2,
      "CATEGORY_ID": "1"
    },
    {
      "ID": "DEAL_STAGE",
      "NAME": "General Deal Stages",
      "SEMANTIC_INFO": {
        "START_FIELD": "NEW",
        "FINAL_SUCCESS_FIELD": "WON",
        "FINAL_UNSUCCESS_FIELD": "LOSE",
        "FINAL_SORT": 0
      },
      "FIELD_ATTRIBUTE_SCOPE": "",
      "ENTITY_TYPE_ID": 2,
      "CATEGORY_ID": 0
    },
    {
      "ID": "QUOTE_STATUS",
      "NAME": "Estimate Stages",
      "SEMANTIC_INFO": {
        "START_FIELD": "DRAFT",
        "FINAL_SUCCESS_FIELD": "APPROVED",
        "FINAL_UNSUCCESS_FIELD": "DECLINED",
        "FINAL_SORT": 0
      },
      "ENTITY_TYPE_ID": 7
    },
    {
      "ID": "HONORIFIC",
      "NAME": "Honorifics"
    },
    {
      "ID": "CALL_LIST",
      "NAME": "Call Statuses"
    },
    {
      "ID": "SMART_DOCUMENT_STAGE_13",
      "NAME": "Document Stages",
      "SEMANTIC_INFO": [],
      "PREFIX": "DT36_13",
      "FIELD_ATTRIBUTE_SCOPE": "category_13",
      "ENTITY_TYPE_ID": 36,
      "IS_ENABLED": true,
      "CATEGORY_ID": 13
    },
    {
      "ID": "DYNAMIC_177_STAGE_7",
      "NAME": "Equipment Procurement (General)",
      "SEMANTIC_INFO": [],
      "PREFIX": "DT177_7",
      "FIELD_ATTRIBUTE_SCOPE": "category_7",
      "ENTITY_TYPE_ID": 177,
      "IS_ENABLED": true,
      "CATEGORY_ID": 7,
      "CATEGORY_NAME": "General",
      "CATEGORY_SORT": 500,
      "IS_DEFAULT_CATEGORY": true
    },
    {
      "ID": "DYNAMIC_177_STAGE_9",
      "NAME": "Equipment Procurement (Second Funnel)",
      "SEMANTIC_INFO": [],
      "PREFIX": "DT177_9",
      "FIELD_ATTRIBUTE_SCOPE": "category_9",
      "ENTITY_TYPE_ID": 177,
      "IS_ENABLED": true,
      "CATEGORY_ID": 9,
      "CATEGORY_NAME": "Second Funnel",
      "CATEGORY_SORT": 500,
      "IS_DEFAULT_CATEGORY": false
    }
  ],
  "time": {
    "start": 1752142616.128453,
    "finish": 1752142616.215683,
    "duration": 0.08722996711730957,
    "processing": 0.018637895584106445,
    "date_start": "2025-07-10T13:16:56+02:00",
    "date_finish": "2025-07-10T13:16:56+02:00",
    "operating_reset_at": 1752143216,
    "operating": 0
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of objects describing the types of directories [(detailed field description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Identifier of the object, use the value in the `ENTITY_ID` field of the methods [crm.status.*](./index.md) ||
|| **NAME**
[`string`](../../data-types.md) | Name ||
|| **ENTITY_TYPE_ID**
[`integer`](../../data-types.md) | [CRM object type](../data-types.md#object_type#) to which the status belongs ||
|| **SEMANTIC_INFO**
[`object`](../../data-types.md) | Information about the semantics of status stages ||
|| **PREFIX**
[`string`](../../data-types.md) | Prefix for the stage code in the funnel ||
|| **FIELD_ATTRIBUTE_SCOPE**
[`string`](../../data-types.md) | Field application scope, funnel ||
|| **IS_ENABLED**
[`boolean`](../../data-types.md) | Activity ||
|| **CATEGORY_ID**
[`integer`](../../data-types.md) | Funnel identifier ||
|| **PARENT_ID**
[`string`](../../data-types.md) | ID of the parent element ||
|| **CATEGORY_NAME**
[`string`](../../data-types.md) | Funnel name ||
|| **CATEGORY_SORT**
[`integer`](../../data-types.md) | Funnel sorting ||
|| **IS_DEFAULT_CATEGORY**
[`boolean`](../../data-types.md) | Default funnel ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-status-fields.md)
- [{#T}](./crm-status-list.md)
- [{#T}](./crm-status-add.md)
- [{#T}](./crm-status-update.md)
- [{#T}](./crm-status-delete.md)