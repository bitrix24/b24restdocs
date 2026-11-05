# Update User Field Values for Inventory Accounting Documents catalog.userfield.document.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View" and "Create and edit" permissions for documents of this type, as well as the "View product catalog" and "View Inventory Management area" permissions

The method `catalog.userfield.document.update` updates the values of user fields in inventory accounting documents.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **documentId*** 
[`catalog_document.id`](../data-types.md#catalog_document) | Identifier of the inventory accounting document. The identifier can be obtained using the [catalog.document.list](../document/catalog-document-list.md) method ||
|| **fields*** 
[`object`](#fields) | Fields to be updated ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **documentType*** 
[`string`](../../data-types.md) | Inventory accounting document type. It must match the type of the `documentId` document, otherwise the method returns an error.

Allowed values: [types of inventory accounting documents](../enum/catalog-enum-get-store-document-types.md) ||
|| **fieldN** 
[`any`](../../data-types.md) | Value of the user field, where `N` is the identifier of the user field, for example `field287`.

You can retrieve custom field identifiers and settings using the [userfieldconfig.list](../../crm/universal/userfieldconfig/userfieldconfig-list.md) method. The [catalog.userfield.document.list](./catalog-userfield-document-list.md) method also returns the `fieldN` keys for documents of the required type.

To clear a value, pass an empty string or `null`. For a single-value field, pass a string or a number: an array in such a field causes the `Internal server error.` error ||
|#

The method changes only the custom fields that are passed. The method skips keys in another format, for example `UF_*` codes, and fields that documents of this type do not have, without an error. Values can also be changed in a posted document.


## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentId":351,"fields":{"documentType":"A","field7097":"Delivery under contract No. 15"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.userfield.document.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentId":351,"fields":{"documentType":"A","field7097":"Delivery under contract No. 15"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.userfield.document.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UpdateUserfieldDocumentResult = {
      document: {
        documentId: number
        documentType: string
        [field: string]: unknown
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<UpdateUserfieldDocumentResult>({
        method: 'catalog.userfield.document.update',
        params: {
          documentId: 351,
          fields: {
            documentType: 'A',
            field7097: 'Delivery under contract No. 15',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.document)
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
      async function updateUserfieldDocument() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.userfield.document.update',
            params: {
              documentId: 351,
              fields: {
                documentType: 'A',
                field7097: 'Delivery under contract No. 15',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.document)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateUserfieldDocument)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.userfield.document.update(
            document_id=351,
            fields={
                "documentType": "A",
                "field7097": "Delivery under contract No. 15",
            },
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
                'catalog.userfield.document.update',
                [
                    'documentId' => 351,
                    'fields' => [
                        'documentType' => 'A',
                        'field7097' => 'Delivery under contract No. 15',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result['document']);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating document user fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.userfield.document.update',
        {
            documentId: 351,
            fields: {
                documentType: 'A',
                field7097: 'Delivery under contract No. 15'
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.userfield.document.update',
        [
            'documentId' => 351,
            'fields' => [
                'documentType' => 'A',
                'field7097' => 'Delivery under contract No. 15',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result['result']['document']);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.userfield.document.update", b24.Params{
    	"documentId": 351,
    	"fields": b24.Params{
    		"documentType": "A",
    		"field7097":    "Delivery under contract No. 15",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.userfield.document.update: %w", err)
    }

    // The method wraps the response in an object with the "document" key.
    raw, ok := b24.Unwrap(res.Result, "document")
    if !ok {
    	return fmt.Errorf("no document key in the response")
    }

    var item struct {
    	DocumentID   b24.ID  `json:"documentId"`
    	DocumentType string  `json:"documentType"`
    	Field7097    *string `json:"field7097"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.DocumentID, item.DocumentType)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "document": {
            "documentId": 351,
            "documentType": "A",
            "field7097": "Delivery under contract No. 15"
        }
    },
    "time": {
        "start": 1793875710,
        "finish": 1793875710.096168,
        "duration": 0.09616804122924805,
        "processing": 0,
        "date_start": "2026-11-05T13:48:30+03:00",
        "date_finish": "2026-11-05T13:48:30+03:00",
        "operating_reset_at": 1793876280,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.document**
[`catalog_userfield_document`](../data-types.md#catalog_userfield_document) | Object with updated values of the document's user fields ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "The specified document does not exist"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Access Denied | The user lacks the "Create and edit" or "View" permission for documents of this type, or the "View product catalog" or "View Inventory Management area" permission ||
|| `400` | `0` | The specified document does not exist | The document with the specified `documentId` was not found. The method returns the same error if `documentId` is not a number ||
|| `400` | `100` | Could not find value for parameter {documentId} | The `documentId` parameter is not passed ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not passed ||
|| `400` | `0` | Required fields: documentType | `fields` does not contain `documentType`, including when `fields` is empty ||
|| `400` | `0` | The DOC_TYPE field does not match the document | The `documentType` value does not match the document type ||
|| `400` | `0` | Internal server error. | An array is passed to a single-value custom field ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-userfield-document-list.md)
- [{#T}](../enum/catalog-enum-get-store-document-types.md)
- [{#T}](../../crm/universal/userfieldconfig/userfieldconfig-list.md)
