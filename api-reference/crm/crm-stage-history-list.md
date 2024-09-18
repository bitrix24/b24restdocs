# Get Stage Movement History crm.stagehistory.list

> Scope: [`crm`](../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method supports retrieving records from the stage movement history for leads, deals, and invoices.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`][1] | Identifier for the object type. Can take the following values:
- `1` — lead
- `2` — deal
- `5` — invoice
||
|| **order**
[`object`][1]| Sorting list, where the key is the field and the value is `ASC` or `DESC` ||
|| **filter**
[`object`][1] | Filtering list. The filter supports the use of exact values, arrays of values, as well as modifiers:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `=` — equal, exact match
- `!=` — not equal
||
|| **select**
[`object`][1]| List of fields to retrieve ||
|| **start**
[`integer`][1] | Offset for pagination. The pagination logic is standard for [list methods](../how-to-call-rest-api/list-methods-pecularities.md) REST ||
|#

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

Get stage movement history for the deal with `ID=1`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"order":{"ID":"ASC"},"filter":{"OWNER_ID":1},"select":["ID","STAGE_ID","CREATED_TIME"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.stagehistory.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"order":{"ID":"ASC"},"filter":{"OWNER_ID":1},"select":["ID","STAGE_ID","CREATED_TIME"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.stagehistory.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.stagehistory.list",
        {
            entityTypeId: 2,
            order: { "ID": "ASC" },
            filter: { "OWNER_ID": 1 },
            select: [ "ID", "STAGE_ID", "CREATED_TIME" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.stagehistory.list',
        [
            'entityTypeId' => 2,
            'order' => ['ID' => 'ASC'],
            'filter' => ['OWNER_ID' => 1],
            'select' => ['ID', 'STAGE_ID', 'CREATED_TIME']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

The method will return an array of records from the history:

```json
{
    "result": {
        "items": [
        {
            "ID": "35",
            "TYPE_ID": "1",
            "OWNER_ID": "21",
            "CREATED_TIME": "2024-04-25T14:59:11+00:00",
            "CATEGORY_ID": "0",
            "STAGE_SEMANTIC_ID": "P",
            "STAGE_ID": "NEW"
        }
        ]
    },
    "total": 1,
    "time": {
        "start": 1724106224.858572,
        "finish": 1724106225.344968,
        "duration": 0.48639607429504395,
        "processing": 0.11864185333251953,
        "date_start": "2024-08-15T22:15:44+00:00",
        "date_finish": "2024-08-15T22:15:45+00:00",
        "operating": 0.11855506896972656
    }
}
```

### Returned Data

Each element of the array is an array with keys.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`int`][1] | Record identifier ||
|| **TYPE_ID**
[`int`][1] | Record type. Can take the following values:
- `1` — object creation
- `2` — transition to an intermediate stage
- `3` — transition to a final stage ||
|| **OWNER_ID**
[`int`][1] | Identifier of the object in which the stage changed ||
|| **CREATED_TIME**
[`datetime`][1] | Identifier of the created element ||
|#

In addition, there are specific fields for different object types:

- for leads and invoices

#|
|| **Name**
`type` | **Description** ||
|| **STATUS_SEMANTIC_ID**
[`int`][1] | Status semantics (stage):
  - `P` — intermediate stage
  - `S` — successful stage
  - `F` — failed stage ||
|| **STATUS_ID**
[`int`][1] | Status identifier (stage) ||
|#

- for deals

#|
|| **Name**
`type` | **Description** ||
|| **CATEGORY_ID**
[`int`][1] | Identifier of the direction (funnel) ||
|| **STAGE_SEMANTIC_ID**
[`int`][1] | Status semantics (stage):
- `P` — intermediate stage
- `S` — successful stage
- `F` — failed stage ||
|| **STAGE_ID**
[`int`][1] | Stage identifier ||
|#

## Error Handling

HTTP Status: **401**, **400**

```json
{
    "error": 0,
    "error_description": "SHIPMENT_DOCUMENT entity is not supported"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code**                           | **Description**                                                       | **Value**                                                                                    ||
|| `400`      | `0`                               | "`entity_name`" Object is not supported                         | Occurs when an invalid `entityTypeId` is passed                                              ||
|| `400`      | `ACCESS_DENIED`                   | Access denied                                                    | The user does not have permission to add elements of type `entityTypeId`                             ||
|| `401`      | `INVALID_CREDENTIALS`             | Invalid authorization data for the request                            | Incorrect user ID and/or code in the request path                                       ||
|#

{% include [system errors](./../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./main-entities-fields.md)

[1]: ../data-types.md