# Get a List of Custom Field Values for Inventory Accounting Documents catalog.userfield.document.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" access permission

The method `catalog.userfield.document.list` returns a paginated list of custom field values for inventory accounting documents.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select***
[`array`](../../data-types.md) | An array containing the list of fields to select (see the fields of the [catalog_userfield_document](../data-types.md#catalog_userfield_document) object).

Make sure to include `documentType` — [the type of inventory accounting document](../enum/catalog-enum-get-store-document-types.md) ||
|| **filter***
[`object`](../../data-types.md) | An object for filtering the selected records in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_userfield_document](../data-types.md#catalog_userfield_document) object.

You must specify `documentType` — [the type of inventory accounting document](../enum/catalog-enum-get-store-document-types.md).

You can add an additional prefix to the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value
- `=%` — LIKE, substring search. The `%` symbol should be included in the value
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match, used by default
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | A sorting object in the format `{"field_1": "order_1", ..., "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_userfield_document](../data-types.md#catalog_userfield_document) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order

If the parameter is not provided, the sorting `documentId ASC` is applied ||
|| **start** 
[`integer`](../../data-types.md)| This parameter is used to manage pagination.

The page size for results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["documentType","documentId","field7097"],"filter":{"documentType":"A","documentId":81},"order":{"documentId":"ASC"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.userfield.document.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["documentType","documentId","field7097"],"filter":{"documentType":"A","documentId":81},"order":{"documentId":"ASC"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.userfield.document.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.

    try {
    const response = await $b24.callListMethod(
        'catalog.userfield.document.list',
        {
        select: ['documentType', 'documentId', 'field7097'],
        filter: { documentType: 'A', documentId: 81 },
        order: { documentId: 'ASC' },
        start: 0
        },
        (progress) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Retrieves data in parts using an iterator. Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('catalog.userfield.document.list', {
        select: ['documentType', 'documentId', 'field7097'],
        filter: { documentType: 'A', documentId: 81 },
        order: { documentId: 'ASC' },
        start: 0
    }, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error) {
    console.error('Request failed', error)
    }

    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.

    try {
    const response = await $b24.callMethod('catalog.userfield.document.list', {
        select: ['documentType', 'documentId', 'field7097'],
        filter: { documentType: 'A', documentId: 81 },
        order: { documentId: 'ASC' },
        start: 0
    }, 0);
    const result = response.getData().result || [];
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
                'catalog.userfield.document.list',
                [
                    'select' => ['documentType', 'documentId', 'field7097'],
                    'filter' => ['documentType' => 'A', 'documentId' => 81],
                    'order' => ['documentId' => 'ASC'],
                    'start' => 0
                ]
            );

        print_r($response->getResponseData()->getResult());
    } catch (\Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.userfield.document.list',
        {
            select: ['documentType', 'documentId', 'field7097'],
            filter: { documentType: 'A', documentId: 81 },
            order:  { documentId: 'ASC' }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());

                if (result.more()) {
                    result.next();
                }
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.userfield.document.list',
        [
            'select' => ['documentType', 'documentId', 'field7097'],
            'filter' => ['documentType' => 'A', 'documentId' => 81],
            'order' => ['documentId' => 'ASC'],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "documents": [
        {
            "documentId": 81,
            "documentType": "A",
            "field7097": "Test Field"
        }
        ]
    },
    "total": 1,
    "time": {
        "start": 1774343822,
        "finish": 1774343822.822166,
        "duration": 0.8221659660339355,
        "processing": 0,
        "date_start": "2026-03-24T12:17:02+01:00",
        "date_finish": "2026-03-24T12:17:02+01:00",
        "operating_reset_at": 1774344422,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **documents**
[`catalog_userfield_document[]`](../data-types.md#catalog_userfield_document) | A list of objects with values of custom fields for documents. The composition of fields depends on the `select` parameter ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. This field is returned if there are more records ||
|| **total**
[`integer`](../../data-types.md) | The total number of records. This field is not returned if the request is executed with `start = -1` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "The documentType field is not specified in the filter parameter"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | The documentType field is not specified in the select parameter | The `documentType` is not specified in the `select` parameter ||
|| `0` | The documentType field is not specified in the filter parameter | The `documentType` is not specified in the `filter` parameter ||
|| `0` | Access Denied | Insufficient rights to read the values of custom fields for documents ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-userfield-document-update.md)
- [{#T}](../enum/catalog-enum-get-store-document-types.md)
- [{#T}](../../crm/universal/userfieldconfig/userfieldconfig-list.md)