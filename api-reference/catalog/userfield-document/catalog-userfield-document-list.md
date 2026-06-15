# Get a List of Custom Field Values for Inventory Accounting Documents catalog.userfield.document.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UserfieldDocumentListResult = {
      documents: Array<{
        documentId: number
        documentType: string
        field7097: string
      }>
    }

    // catalog.userfield.document.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<UserfieldDocumentListResult>({
        method: 'catalog.userfield.document.list',
        params: {
          select: ['documentType', 'documentId', 'field7097'],
          filter: { documentType: 'A', documentId: 81 },
          order: { documentId: 'ASC' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.documents, `page size: ${result.documents.length}`)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listUserfieldDocuments() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.userfield.document.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.userfield.document.list',
            params: {
              select: ['documentType', 'documentId', 'field7097'],
              filter: { documentType: 'A', documentId: 81 },
              order: { documentId: 'ASC' },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.documents, `page size: ${result.documents.length}`)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listUserfieldDocuments)
    </script>
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