# Delete Multiple Inventory Documents v 3.0 catalog.document.deletelist

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Delete inventory object" permission for the required document type and the "View and select warehouses" permission for all warehouses of the document

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.document.deletelist` method deletes multiple inventory documents.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **documentIds***
[`array`](../../data-types.md) | An array of document identifiers, for example `[345, 347]`. Zero and negative values are skipped.

The identifiers can be retrieved using the [catalog.document.list](./catalog-document-list-rest-v3.md) method.

The method deletes the documents one by one. If some documents cannot be deleted, the method returns an error with the list of failed documents, but the remaining documents are deleted ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.deletelist`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentIds":[345,347]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.deletelist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentIds":[345,347],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.deletelist
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentDeletelistResult = {
      result: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentDeletelistResult>({
        method: 'catalog.document.deletelist',
        params: {
          documentIds: [345, 347],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Deleted:', result.result)
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
      async function callDocumentDeletelist() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.deletelist',
            params: {
              documentIds: [345, 347],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Deleted:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', callDocumentDeletelist)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.deletelist(
            documentIds=[345, 347],
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
                'catalog.document.deletelist',
                [
                    'documentIds' => [345, 347],
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
        'catalog.document.deletelist',
        {
            documentIds: [345, 347],
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
        'catalog.document.deletelist',
        [
            'documentIds' => [345, 347],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.deletelist", b24.Params{
    	"documentIds": []int{345, 347},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.deletelist: %w", err)
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
        "start": 1792665282,
        "finish": 1792665282.935256,
        "duration": 0.9352560043334961,
        "processing": 0,
        "date_start": "2026-10-22T13:34:42+02:00",
        "date_finish": "2026-10-22T13:34:42+02:00",
        "operating_reset_at": 1792665840,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object with the `result` key and the `true` value if the documents are deleted ||
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
                "message": "Error deleting inventory object \"#345\": Inventory object was not found."
            },
            {
                "message": "Error deleting inventory object \"#999999\": Inventory object was not found."
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

A separate message comes in the `validation` array for each document that could not be deleted.

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `documentIds` | Required field `documentIds` is missing | Pass a non-empty array of positive identifiers ||
|| `documentIds` | This request requires data type `array` in field `documentIds` | Pass the identifiers as an array, for example `[345, 347]` ||
|| `-` | Error deleting inventory object "#ID#": Inventory object was not found | Check the identifier using the [catalog.document.list](./catalog-document-list-rest-v3.md) method ||
|| `-` | You cannot delete inventory object "#DOC_TITLE#" because it has already been processed | Cancel processing using the [catalog.document.cancel](./catalog-document-cancel-rest-v3.md) method and repeat the deletion ||
|| `-` | Inventory Management is not available on your plan | The method works only on plans with inventory management ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the "Delete inventory object" permission for the document type and access to all warehouses of the document ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-add-rest-v3.md)
- [{#T}](./catalog-document-update-rest-v3.md)
- [{#T}](./catalog-document-get.md)
- [{#T}](./catalog-document-list-rest-v3.md)
- [{#T}](./catalog-document-delete-rest-v3.md)
- [{#T}](./catalog-document-field-list.md)
- [{#T}](./catalog-document-field-get.md)
