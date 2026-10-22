# Cancel Inventory Document Processing v 3.0 catalog.document.cancel

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Cancel processing" permission for the document type and the "View product catalog" and "View Inventory Management area" permissions

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.document.cancel` method cancels the processing of one inventory document by `id` or of several documents by `documentIds`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Inventory document identifier.

The identifier can be retrieved using the [catalog.document.list](./catalog-document-list-rest-v3.md) method.

Required if `documentIds` is not passed ||
|| **documentIds**
[`array`](../../data-types.md) | Array of inventory document identifiers, for example `[321, 323]`.

The identifiers can be retrieved using the [catalog.document.list](./catalog-document-list-rest-v3.md) method.

Pass it instead of `id` to cancel the processing of several documents. If both parameters are passed, the method uses `documentIds` ||
|#

{% note warning "" %}

Documents are handled one by one, and there is no overall rollback on error. If at least one document cannot be canceled, the method returns an error, but the other documents in the list are canceled. Check the document states in the `status` field using the [catalog.document.list](./catalog-document-list-rest-v3.md) method.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.cancel`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":321}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.cancel
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":321,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.cancel
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentCancelResult = {
      result: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentCancelResult>({
        method: 'catalog.document.cancel',
        params: {
          id: 321,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Document cancelled:', result.result)
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
      async function cancelDocument() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.cancel',
            params: {
              id: 321,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Document cancelled:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', cancelDocument)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.cancel(
            bitrix_id=321,
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
                'catalog.document.cancel',
                [
                    'id' => 321,
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
        'catalog.document.cancel',
        {
            id: 321,
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
        'catalog.document.cancel',
        [
            'id' => 321,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.cancel", b24.Params{
    	"id": 321,
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.cancel: %w", err)
    }

    var result struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true
    },
    "time": {
        "start": 1792665205,
        "finish": 1792665205.320535,
        "duration": 0.32053494453430176,
        "processing": 0,
        "date_start": "2026-10-22T13:33:25+02:00",
        "date_finish": "2026-10-22T13:33:25+02:00",
        "operating_reset_at": 1792665660,
        "operating": 0.6613998413085938
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response.

Contains an object with the `result` key set to `true` if the document processing is canceled ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
                "message": "Error canceling inventory object processing: The transaction is still pending."
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
|| `id` | Required field `id` is missing | Pass `id` as a positive integer ||
|| `id` | This request requires data type `int` in field `id` | Pass `id` as a number, not a string ||
|| `documentIds` | Required field `documentIds` is missing | Pass a non-empty array of positive identifiers. Zero and negative values are ignored ||
|| `documentIds` | This request requires data type `array` in field `documentIds` | Pass `documentIds` as an array, for example `[321, 323]` ||
|| `-` | Error canceling inventory object processing: The transaction is still pending | The document is not processed, so there is nothing to cancel. Check the document state in the `status` field using the [catalog.document.list](./catalog-document-list-rest-v3.md) method ||
|| `-` | Error canceling inventory object processing: #ERROR# | Fix the document according to the error text after the colon ||
|| `-` | Error canceling inventory object "#DOC_TITLE#": #ERROR# | Fix the specified document according to the error text. The other documents in the list have been canceled ||
|| `-` | Error canceling inventory object processing: Inventory object was not found | The document does not exist or is not available to the user. Check the identifiers using the [catalog.document.list](./catalog-document-list-rest-v3.md) method ||
|| `-` | Error canceling inventory object processing: Insufficient permissions to save the inventory object | The user does not have the "Cancel processing" permission for this document type ||
|| `-` | Inventory Management is not available on your plan | Inventory management is not included in your Bitrix24 plan. The document methods are unavailable ||
|| `-` | Inventory management must be enabled to process the document | Enable inventory management. Without it, canceling the processing of multiple documents is unavailable ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no document with this identifier. Check the identifier using the [catalog.document.list](./catalog-document-list-rest-v3.md) method ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog", "View Inventory Management area", and "Cancel processing" permissions for the document type ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-cancel-list-rest-v3.md)
- [{#T}](./catalog-document-conduct-rest-v3.md)
- [{#T}](./catalog-document-list-rest-v3.md)
- [{#T}](./catalog-document-delete-rest-v3.md)
