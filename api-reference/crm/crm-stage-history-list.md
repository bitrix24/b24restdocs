# Get Stage Movement History with crm.stagehistory.list

> Scope: [`crm`](../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method `crm.stagehistory.list` returns records of stage movement history for the following elements:
- [leads](./leads/index.md),
- [deals](./deals/index.md),
- [old invoices](./outdated/invoice/index.md),
- [new invoices](./universal/invoice.md),
- [SPA](./universal/user-defined-object-types/index.md).

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`][1] | Identifier of the object type. Can take the following values:
- `1` — lead,
- `2` — deal,
- `5` — invoice (old),
- `31` - invoice (new),
- numeric identifier of a [custom type](./universal/user-defined-object-types/index.md#id)
||
|| **order**
[`object`][1]| Sorting list, where the key is the field and the value is `ASC` or `DESC` ||
|| **filter**
[`object`][1] | Filtering list. The filter supports the use of exact values, arrays of values, and modifiers:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` character must be included in the value. Examples:
	- `"mol%"` — searches for values starting with "mol"
	- `"%mol"` — searches for values ending with "mol"
	- `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character should not be included in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` character must be included in the value. Examples:
	- `"mol%"` — searches for values not starting with "mol"
	- `"%mol"` — searches for values not ending with "mol"
	- `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **select**
[`object`][1]| List of fields to retrieve ||
|| **start**
[`integer`][1] | Offset for pagination. The pagination logic is standard for [list methods](../how-to-call-rest-api/list-methods-pecularities.md) ||
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
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    const parameters = {
        entityTypeId: 2,
        order: { "ID": "ASC" },
        filter: { "OWNER_ID": 1 },
        select: [ "ID", "STAGE_ID", "CREATED_TIME" ]
    };
    
    try {
      const response = await $b24.callListMethod(
        'crm.stagehistory.list',
        parameters,
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative fetching using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.stagehistory.list', parameters, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.stagehistory.list', parameters, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.stagehistory.list',
                [
                    'entityTypeId' => 2,
                    'order' => ['ID' => 'ASC'],
                    'filter' => ['OWNER_ID' => 1],
                    'select' => ['ID', 'STAGE_ID', 'CREATED_TIME'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
        if ($result->more()) {
            $result->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing stage history: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

```json
{
    "result": {
        "items": [
        {
            "ID": 35,
            "TYPE_ID": 1,
            "OWNER_ID": 21,
            "CREATED_TIME": "2024-04-25T14:59:11+00:00",
            "CATEGORY_ID": 0,
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

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Root element of the response, contains an array of [items](#items). Each object is an array with keys ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

#### Items Object {#items}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`int`][1] | Identifier of the record ||
|| **TYPE_ID**
[`int`][1] | Type of the record. Can take the following values:
- `1` — creation of an element,
- `2` — transition to an intermediate stage,
- `3` — transition to the final stage,
- `5` — change of funnel ||
|| **OWNER_ID**
[`int`][1] | Identifier of the object in which the stage changed ||
|| **CREATED_TIME**
[`datetime`][1] | Identifier of the created element, equal to the time of the element's transition to the stage ||
|#

Additionally, there are fields specific to different object types:

{% list tabs %}

- for leads and old invoices

    #|
    || **Name**
    `type` | **Description** ||
    || **STATUS_SEMANTIC_ID**
    [`int`][1] | Stage semantics:
    - `P` — intermediate stage,
    - `S` — successful stage,
    - `F` — failed stage ||
    || **STATUS_ID**
    [`int`][1] | Identifier of the stage ||
    |#

- for deals, new invoices, and SPAs

    #|
    || **Name**
    `type` | **Description** ||
    || **CATEGORY_ID**
    [`int`][1] | Identifier of the funnel ||
    || **STAGE_SEMANTIC_ID**
    [`int`][1] | Stage semantics:
    - `P` — intermediate stage,
    - `S` — successful stage,
    - `F` — failed stage ||
    || **STAGE_ID**
    [`int`][1] | Identifier of the stage ||
    |#

{% endlist %}    

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
|| **Status** | **Code**                           | **Description**                                                       | **Value**                                      ||
|| `403`      | `allowed_only_intranet_user`      | Action allowed only for intranet users                               | User is not an intranet user                   ||
|| `400`      | `ENTITY_TYPE_NOT_SUPPORTED`       | Entity type `TYPE` is not supported                                  | Occurs when an invalid `entityTypeId` is passed ||
|| `400`      | `INVALID_ARG_VALUE`               | Invalid filter: field '`field`' is not allowed in filter           | The field `field` passed in `filter` is not available for filtering ||
|| `400`      | `INVALID_ARG_VALUE`               | Invalid filter: field '`field`' has invalid value                  | The value passed for field `field` in `filter` is incorrect ||
|| `400`      | `INVALID_ARG_VALUE`               | Invalid order: field '`field`' is not allowed in order             | The field `field` passed in `order` is not available for sorting ||
|| `400`      | `INVALID_ARG_VALUE`               | Invalid order: allowed sort directions are `ASC, DESC`. But got '`orderValue`' for field '`field`' | The value `orderValue` passed for field `field` in the `order` parameter is incorrect ||

|#

{% include [system errors](./../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./main-entities-fields.md)

[1]: ../data-types.md