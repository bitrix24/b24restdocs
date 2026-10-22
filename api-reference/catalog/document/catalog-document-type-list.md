# Get Inventory Document Types catalog.document.type.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.document.type.list` method returns the list of inventory document types. The type code is passed in the `docType` field when creating a document.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of document type fields to return in the response.

Available fields:

- `id` — document type code
- `name` — document type name

Default: all fields ||
|#

The method returns the entire directory. The `filter`, `order`, and `pagination` parameters are not supported: if they are passed, the method returns an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter to the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.type.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.type.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.type.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentTypeListResult = {
      items: Array<{
        id: 'A' | 'S' | 'M' | 'R' | 'D'
        name: string
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentTypeListResult>({
        method: 'catalog.document.type.list',
        params: {
          select: ['id', 'name'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Document types:', result.items)
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
      async function listDocumentTypes() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.type.list',
            params: {
              select: ['id', 'name'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Document types:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listDocumentTypes)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.type.list(
            select=["id", "name"],
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
                'catalog.document.type.list',
                [
                    'select' => ['id', 'name'],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.document.type.list',
        {
            select: ['id', 'name'],
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
        'catalog.document.type.list',
        [
            'select' => ['id', 'name'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.type.list", b24.Params{
    	"select": []string{"id", "name"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.document.type.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID   string `json:"id"`
    		Name string `json:"name"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Name)
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
                "id": "A",
                "name": "Stock receipt"
            },
            {
                "id": "S",
                "name": "Stock adjustment"
            },
            {
                "id": "M",
                "name": "Stock transfer"
            },
            {
                "id": "R",
                "name": "Return"
            },
            {
                "id": "D",
                "name": "Write-off"
            }
        ]
    },
    "time": {
        "start": 1792665215,
        "finish": 1792665215.710361,
        "duration": 0.7103610038757324,
        "processing": 0,
        "date_start": "2026-10-22T13:33:35+02:00",
        "date_finish": "2026-10-22T13:33:35+02:00",
        "operating_reset_at": 1792665780,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | An array of document types ||
|| **result.items[].id**
[`string`](../../data-types.md) | Document type code.

Possible values:

- `A` — goods receipt
- `S` — goods acquisition
- `M` — goods transfer between warehouses
- `R` — goods return
- `D` — goods write-off ||
|| **result.items[].name**
[`string`](../../data-types.md) | Document type name ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

The same directory is returned by the [catalog.enum.getStoreDocumentTypes](../enum/catalog-enum-get-store-document-types.md) method in the previous API version.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "DTO \"DocumentTypeDto\" in field \"id\" requires attribute \"Filterable\" to perform this request.",
                "field": "id"
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
|| `id`, `name` | DTO `DocumentTypeDto` in field `#FIELD#` requires attribute `Filterable` to perform this request | Remove `filter`: the method does not filter the directory ||
|| `id`, `name` | DTO `DocumentTypeDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Remove `order`: the method does not sort the directory ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Remove `pagination`: the method returns the entire directory ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DocumentTypeDto` | In `select`, specify only `id` and `name` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-type-field-list.md)
- [{#T}](./catalog-document-add-rest-v3.md)
- [{#T}](./catalog-document-list-rest-v3.md)
