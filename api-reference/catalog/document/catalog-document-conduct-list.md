# Process Multiple Inventory Documents catalog.document.conductList

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" and "View Inventory Management area" permissions and the "Process inventory objects" permission for the required document type. The document must be available to the user under the "View" permission

The method `catalog.document.conductList` processes multiple inventory documents.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **documentIds***
[`catalog_document.id[]`](../data-types.md#catalog_document) | Array of document IDs, for example `[142, 143, 144]`.

Document IDs can be retrieved with the [catalog.document.list](./catalog-document-list.md) method. The method accepts an empty array without an error and returns `true` ||
|#

The method processes documents one by one, like [catalog.document.conduct](./catalog-document-conduct.md). If a document cannot be processed, the method moves on to the next one: the processed documents stay processed, and the response contains an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentIds":[142,143,144]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.conductList
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentIds":[142,143,144],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.conductList
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'catalog.document.conductList',
        params: {
          documentIds: [142, 143, 144],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Documents conducted:', result)
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
      async function conductDocumentList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.document.conductList',
            params: {
              documentIds: [142, 143, 144],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Documents conducted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', conductDocumentList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.conduct_list(
            document_ids=[
                142,
                143,
                144,
            ],
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.conductList',
                [
                    'documentIds' => [142, 143, 144],
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

    ```js
    BX24.callMethod(
        'catalog.document.conductList',
        { documentIds: [142, 143, 144] },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.conductList',
        [
            'documentIds' => [142, 143, 144],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.conductList", b24.Params{
    	"documentIds": []int{142, 143, 144},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.conductList: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1793883405,
        "finish": 1793883406.059175,
        "duration": 1.0591750144958496,
        "processing": 1,
        "date_start": "2026-11-05T15:56:45+03:00",
        "date_finish": "2026-11-05T15:56:46+03:00",
        "operating_reset_at": 1793883960,
        "operating": 0.12781405448913574
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response. Contains `true` if all documents are processed. If at least one document cannot be processed, the method returns an error, while the other documents are processed ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Error processing inventory object \"Receipt from vendor\": Vendor is not specified.."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Insufficient permissions to save the inventory object. | The user does not have the "View product catalog" or "View Inventory Management area" permission ||
|| `400` | `0` | Error processing inventory object: Insufficient permissions to save the inventory object.. | The user does not have the "Process inventory objects" permission for documents of this type ||
|| `400` | `100` | Could not find value for parameter {documentIds} | The `documentIds` parameter is not passed ||
|| `400` | `100` | Invalid value {abc} to match with parameter {documentIds}. Should be value of type array. | `documentIds` is not an array ||
|| `400` | `0` | Error processing "#DOC_TITLE#". | The document is not found or is unavailable to the user. The text comes with an unfilled `#DOC_TITLE#` ||
|| `400` | `0` | Error processing inventory object "Receipt from vendor": Vendor is not specified.. | The document could not be processed. The text contains the document name or, if it has none, the document type name, and the reason, for example, no vendor or no products ||
|| `400` | `0` | Inventory Management is not available on your plan. | Inventory Management is not available on the account plan ||
|| `400` | `0` | You have to enable inventory management to process the inventory object. | Inventory Management is disabled on the account. You can check the status with the [catalog.document.mode.status](./catalog-document-mode-status.md) method ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-list.md)
- [{#T}](./catalog-document-conduct.md)
- [{#T}](./catalog-document-cancel-list.md)
