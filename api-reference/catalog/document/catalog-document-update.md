# Update Inventory Document catalog.document.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View and select warehouses" permission and the "Create and edit" permission for the required document type

The method `catalog.document.update` updates the fields of an inventory document that has not been processed yet.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_document.id`](../data-types.md#catalog_document) | Document ID.

Document IDs can be retrieved with the [catalog.document.list](./catalog-document-list.md) method ||
|| **fields***
[`object`](../../data-types.md) | Values of the document fields to update ([detailed description](#fields)).

The method changes only the passed fields, the others keep their previous values. The method accepts an empty object `{}` without an error and updates only the modification date ||
|#

### Fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Document name. An empty string clears the name, and `title` in the response is `null` ||
|| **docNumber**
[`string`](../../data-types.md) | Document number ||
|| **dateDocument**
[`datetime`](../../data-types.md) | Document date, for example `2026-11-06T10:00:00+03:00`. You can pass a date without time, for example `2026-11-06`.

The method skips `null` and a string that cannot be parsed as a date without an error: the previous date is kept ||
|| **commentary**
[`string`](../../data-types.md) | Document comment ||
|| **responsibleId**
[`user.id`](../../data-types.md) | Responsible user ID. The method does not check that the user exists. The method saves a non-numeric value as `null` ||
|| **total**
[`double`](../../data-types.md) | Document total. The method saves a non-numeric value as `null` ||
|| **siteId**
[`string`](../../data-types.md) | Site code ||
|| **statusBy**
[`user.id`](../../data-types.md) | ID of the user who changed the document status ||
|#

The `docType` and `currency` fields cannot be changed after creation. The method skips them, as well as the `id`, `status`, `createdBy`, `modifiedBy`, `dateCreate`, `dateModify`, `dateStatus` fields and unknown fields, without an error. After the update, `modifiedBy` equals the ID of the current user, and `dateModify` equals the update time.

A processed document cannot be updated: first cancel processing with the [catalog.document.cancel](./catalog-document-cancel.md) method.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":381,"fields":{"commentary":"Corrected the document date","dateDocument":"2026-11-06T10:00:00+03:00"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":381,"fields":{"commentary":"Corrected the document date","dateDocument":"2026-11-06T10:00:00+03:00"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentUpdateResult = {
      // An empty array comes back when no document has the given id
      document: {
        commentary: string | null
        createdBy: number
        currency: string
        dateCreate: ISODate
        dateDocument: ISODate | null
        dateModify: ISODate
        dateStatus: ISODate
        docNumber: string | null
        docType: string
        id: number
        modifiedBy: number
        responsibleId: number | null
        siteId: string
        status: string
        statusBy: number | null
        title: string | null
        total: number | null
      } | []
    }

    try {
      const response = await $b24.actions.v2.call.make<DocumentUpdateResult>({
        method: 'catalog.document.update',
        params: {
          id: 381,
          fields: {
            commentary: 'Corrected the document date',
            dateDocument: '2026-11-06T10:00:00+03:00',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        if (Array.isArray(result.document)) {
          console.warn('Document not found')
        } else {
          console.info('Updated document:', result.document.id, result.document.dateDocument)
        }
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
      async function updateDocument() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.document.update',
            params: {
              id: 381,
              fields: {
                commentary: 'Corrected the document date',
                dateDocument: '2026-11-06T10:00:00+03:00',
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
          // An empty array comes back when no document has the given id
          if (Array.isArray(result.document)) {
            console.warn('Document not found')
            return
          }
          console.info('Updated document:', result.document.id, result.document.dateDocument)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateDocument)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.update(
            bitrix_id=381,
            fields={
                "commentary": "Corrected the document date",
                "dateDocument": "2026-11-06T10:00:00+03:00",
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
                'catalog.document.update',
                [
                    'id'     => 381,
                    'fields' => [
                        'commentary'   => 'Corrected the document date',
                        'dateDocument' => '2026-11-06T10:00:00+03:00',
                    ],
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
        'catalog.document.update',
        {
            id: 381,
            fields: {
                commentary: 'Corrected the document date',
                dateDocument: '2026-11-06T10:00:00+03:00',
            },
        },
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
        'catalog.document.update',
        [
            'id' => 381,
            'fields' => [
                'commentary' => 'Corrected the document date',
                'dateDocument' => '2026-11-06T10:00:00+03:00',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.update", b24.Params{
    	"id": 381,
    	"fields": b24.Params{
    		"commentary":   "Corrected the document date",
    		"dateDocument": "2026-11-06T10:00:00+03:00",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.update: %w", err)
    }

    // The method wraps the response in an object with the "document" key.
    // If the document does not exist, the key contains an empty array [].
    raw, ok := b24.Unwrap(res.Result, "document")
    if !ok || string(raw) == "[]" {
    	return fmt.Errorf("document not found")
    }

    // Optional fields can be null, so pointers are used for them.
    var item struct {
    	ID           b24.ID  `json:"id"`
    	Status       string  `json:"status"`
    	Commentary   *string `json:"commentary"`
    	DateDocument *string `json:"dateDocument"`
    	ModifiedBy   int     `json:"modifiedBy"`
    	DateModify   string  `json:"dateModify"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.DateModify)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "document": {
            "commentary": "Corrected the document date",
            "createdBy": 1269,
            "currency": "USD",
            "dateCreate": "2026-11-05T15:58:42+03:00",
            "dateDocument": "2026-11-06T10:00:00+03:00",
            "dateModify": "2026-11-05T15:59:21+03:00",
            "dateStatus": "2026-11-05T15:58:42+03:00",
            "docNumber": "OP-00042",
            "docType": "S",
            "id": 381,
            "modifiedBy": 1269,
            "responsibleId": 1269,
            "siteId": "s1",
            "status": "N",
            "statusBy": null,
            "title": "Surplus stock adjustment",
            "total": null
        }
    },
    "time": {
        "start": 1793883561,
        "finish": 1793883561.476519,
        "duration": 0.4765191078186035,
        "processing": 0,
        "date_start": "2026-11-05T15:59:21+03:00",
        "date_finish": "2026-11-05T15:59:21+03:00",
        "operating_reset_at": 1793884140,
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
[`catalog_document`](../data-types.md#catalog_document) | Object with all document fields after the update.

If there is no document with the passed `id`, the method does not return an error: `result.document` contains an empty array `[]`. You can check that the document exists with the [catalog.document.list](./catalog-document-list.md) method with a filter by `id` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Cannot modify processed item"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Cannot complete action because you don't have sufficient permissions to view and select warehouses. | The user does not have the "View and select warehouses" permission ||
|| `400` | `0` | Insufficient permissions to save the inventory object. | The user does not have the "Create and edit" permission for documents of this type or has no catalog permissions ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter is not passed ||
|| `400` | `100` | Invalid value {abc} to match with parameter {id}. Should be value of type int. | `id` is not a number ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not passed ||
|| `400` | `100` | Invalid value {abc} to match with parameter {fields}. Should be value of type array. | `fields` is not an object ||
|| `400` | `0` | Cannot modify processed item | The document is processed. First cancel processing with the [catalog.document.cancel](./catalog-document-cancel.md) method ||
|| `400` | `0` | Wrong format of field `title`. | The field contains an array or an object instead of a value. The text contains the field name ||
|| `400` | `0` | Inventory Management is not available on your plan. | Inventory Management is not available on the account plan ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-add.md)
- [{#T}](./catalog-document-list.md)
- [{#T}](./catalog-document-cancel.md)
- [{#T}](./catalog-document-get-fields.md)
- [{#T}](./document-element/catalog-document-element-update.md)
