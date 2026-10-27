# Get List of Inventory Document Custom Field Values v 3.0 catalog.document.userfield.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View" permission for documents of this type and the "View product catalog" and "View Inventory Management area" permissions

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.document.userfield.list` returns the custom field values of inventory documents of one type.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../data-types.md) | Filter in REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`, for example `[["docType", "=", "A"], ["id", "=", 325]]`. The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter).

The condition `["docType", "=", "<type>"]` with the `=` operator is required. Possible type values are returned by the [catalog.document.type.list](../document/catalog-document-type-list.md) method. For a type that does not exist, the method returns an empty list.

Besides `docType`, you can filter by `id`. Filtering by `userFields`, condition groups with `or` logic, and negation are not supported ||
|| **select**
[`array`](../../data-types.md) | Array of fields to return in the response.

Available fields:

- `id` — document identifier
- `docType` — document type
- `userFields` — custom field values

The value `["*"]` is not supported.

By default: all fields ||
|| **order**
[`object`](../../data-types.md) | Sorting in the format `{"field": "direction"}`, for example `{"id": "desc"}`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

You can sort by `id` and `docType` ||
|| **pagination**
[`object`](../../data-types.md) | Pagination. [Object description](#pagination) ||
|#

### Parameter Pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of records per page. Values greater than `1000` are reduced to `1000`, the value `0` causes an error.

By default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. If passed, it replaces `offset`. A page beyond the end of the list returns an empty array ||
|| **offset**
[`integer`](../../data-types.md) | Number of records to skip ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.userfield.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["docType","=","A"],["id","=",325]],"select":["id","docType","userFields"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.userfield.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["docType","=","A"],["id","=",325]],"select":["id","docType","userFields"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.userfield.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentUserfieldListResult = {
      items: Array<{
        id?: number
        docType?: string
        userFields?: Record<string, unknown> | null
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentUserfieldListResult>({
        method: 'catalog.document.userfield.list',
        params: {
          filter: [
            ['docType', '=', 'A'],
            ['id', '=', 325],
          ],
          select: ['id', 'docType', 'userFields'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Documents:', result.items)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function documentUserfieldList() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.userfield.list',
            params: {
              filter: [
                ['docType', '=', 'A'],
                ['id', '=', 325],
              ],
              select: ['id', 'docType', 'userFields'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Documents:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', documentUserfieldList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.userfield.list(
            filter=[
                ["docType", "=", "A"],
                ["id", "=", 325],
            ],
            select=["id", "docType", "userFields"],
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.userfield.list',
                [
                    'filter' => [
                        ['docType', '=', 'A'],
                        ['id', '=', 325],
                    ],
                    'select' => ['id', 'docType', 'userFields'],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.document.userfield.list',
        {
            filter: [
                ['docType', '=', 'A'],
                ['id', '=', 325],
            ],
            select: ['id', 'docType', 'userFields'],
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.userfield.list',
        [
            'filter' => [
                ['docType', '=', 'A'],
                ['id', '=', 325],
            ],
            'select' => ['id', 'docType', 'userFields'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.userfield.list", b24.Params{
    	"filter": []any{
    		[]any{"docType", "=", "A"},
    		[]any{"id", "=", 325},
    	},
    	"select": []string{"id", "docType", "userFields"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.document.userfield.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID         int                        `json:"id"`
    		DocType    string                     `json:"docType"`
    		UserFields map[string]json.RawMessage `json:"userFields"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.DocType, len(item.UserFields))
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 325,
                "docType": "A",
                "userFields": {
                    "UF_CAT_STORE_DOCUMENT_A_REST_TEST": "1"
                }
            }
        ]
    },
    "time": {
        "start": 1792665383,
        "finish": 1792665383.478756,
        "duration": 0.4787559509277344,
        "processing": 0,
        "date_start": "2026-10-22T13:36:23+02:00",
        "date_finish": "2026-10-22T13:36:23+02:00",
        "operating_reset_at": 1792665960,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | Array of documents. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Document identifier ||
|| **docType**
[`string`](../../data-types.md) | Document type ||
|| **userFields**
[`object`](../../data-types.md) or `null` | Custom field values of the document. The keys are field codes of the form `UF_*`, for example `UF_CAT_STORE_DOCUMENT_A_REST_TEST`.

`null` if documents of this type have no custom fields ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "The docType field is not specified in the filter parameter"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | The docType field is not specified in the filter parameter | Add the condition `["docType", "=", "<type>"]` to `filter`. The `in` operator is not supported for `docType` ||
|| `filter` | Filter OR groups are not supported | Remove groups with `or` logic from `filter` ||
|| `userFields` | DTO `UserfieldDocumentDto` in field `userFields` requires attribute `Filterable` to perform this request | Remove `userFields` from `filter` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `UserfieldDocumentDto` | Specify only fields from the `select` parameter description ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass a number from `1` to `1000` in `limit` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for documents of this type and for the Inventory Management section ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-userfield-document-update-rest-v3.md)
- [{#T}](./catalog-userfield-document-field-list.md)
- [{#T}](../document/catalog-document-list-rest-v3.md)
- [{#T}](./index.md)
