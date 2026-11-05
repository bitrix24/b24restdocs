# Add Vendor to Inventory Document catalog.documentcontractor.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View" and "Create and edit" permissions for stock receipt documents and the "View Inventory Management area" and "View product catalog" permissions

The method `catalog.documentcontractor.add` creates a binding of a vendor, contact, or company to an inventory document.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Binding fields [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **documentId***
[`catalog_document.id`](../data-types.md#catalog_document) | Identifier of the inventory document of type "Incoming" `A`.
Can be obtained using the method [catalog.document.list](../document/catalog-document-list.md) ||
|| **entityTypeId***
[`integer`](../../crm/data-types.md#object_type) | Type of CRM object:
- `3` — contact
- `4` — company ||
|| **entityId***
[`integer`](../../data-types.md) | Identifier of the CRM object, contact, or company from the "Vendor" category. The method does not check that the contact or company exists: a nonexistent identifier is saved without an error.

To obtain vendor identifiers:
1. Get the category identifier with the code `CATALOG_CONTRACTOR_CONTACT` for contacts or `CATALOG_CONTRACTOR_COMPANY` for companies using the method [crm.category.list](../../crm/universal/category/crm-category-list.md).
2. Use the obtained `categoryId` in the filter of the request [crm.item.list](../../crm/universal/crm-item-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"documentId":73,"entityTypeId":3,"entityId":2185}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.documentcontractor.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"documentId":73,"entityTypeId":3,"entityId":2185},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.documentcontractor.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentContractorAddResult = {
      documentContractor: {
        documentId: number
        entityId: number
        entityTypeId: number
        id: number
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<DocumentContractorAddResult>({
        method: 'catalog.documentcontractor.add',
        params: {
          fields: {
            documentId: 73,
            entityTypeId: 3,
            entityId: 2185,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.documentContractor.id, result.documentContractor)
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
      async function addDocumentContractor() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.documentcontractor.add',
            params: {
              fields: {
                documentId: 73,
                entityTypeId: 3,
                entityId: 2185,
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
          console.info(result.documentContractor.id, result.documentContractor)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addDocumentContractor)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.documentcontractor.add(
            fields={
                "documentId": 73,
                "entityTypeId": 3,
                "entityId": 2185,
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
                'catalog.documentcontractor.add',
                [
                    'fields' => [
                        'documentId' => 73,
                        'entityTypeId' => 3,
                        'entityId' => 2185
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result) {
            echo 'Success: ' . print_r($result, true);
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding contractor binding: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.documentcontractor.add',
        {
            fields: {
                documentId: 73,
                entityTypeId: 3,
                entityId: 2185
            }
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
        'catalog.documentcontractor.add',
        [
            'fields' => [
                'documentId' => 73,
                'entityTypeId' => 3,
                'entityId' => 2185
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
    res, err := client.Core().Call(ctx, "catalog.documentcontractor.add", b24.Params{
    	"fields": b24.Params{
    		"documentId":   73,
    		"entityTypeId": 3,
    		"entityId":     2185,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.documentcontractor.add: %w", err)
    }

    // The method wraps the response in an object with the "documentContractor" key.
    raw, ok := b24.Unwrap(res.Result, "documentContractor")
    if !ok {
    	return fmt.Errorf("no documentContractor key in the response")
    }

    var item struct {
    	DocumentID   b24.ID `json:"documentId"`
    	EntityID     b24.ID `json:"entityId"`
    	EntityTypeID b24.ID `json:"entityTypeId"`
    	ID           b24.ID `json:"id"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.DocumentID, item.EntityID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "documentContractor": {
            "documentId": 73,
            "entityId": 2185,
            "entityTypeId": 3,
            "id": 15
        }
    },
    "time": {
        "start": 1766469835,
        "finish": 1766469835.824666,
        "duration": 0.8246660232543945,
        "processing": 0,
        "date_start": "2025-12-23T09:03:55+01:00",
        "date_finish": "2025-12-23T09:03:55+01:00",
        "operating_reset_at": 1766470435,
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
|| **result.documentContractor**
[`catalog_documentcontractor`](../data-types.md#catalog_documentcontractor) | Object with data of the created vendor binding to the inventory document ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Store document was not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {fields} | The required parameter `fields` was not provided ||
|| `400` | `0` | Required fields: documentId, entityTypeId, entityId | Required fields are missing in `fields`. The message lists only the missing fields ||
|| `400` | `0` | Store document was not found | There is no document with the specified `documentId`, or it is not available to the user ||
|| `400` | `0` | Type of store document is wrong | The document is not a stock receipt document `A` ||
|| `400` | `0` | Unable to edit conducted document | The document is already processed ||
|| `400` | `0` | Wrong entity type id | `entityTypeId` contains a value other than `3` and `4` ||
|| `400` | `0` | Wrong entity id | `entityId` is `0` ||
|| `400` | `0` | This contractor has been already bound to this document | Such a binding already exists ||
|| `400` | `0` | This document already has a Company contractor | A company is already bound to the document ||
|| `400` | `0` | Access denied | Insufficient permissions to modify the document ||
|| `400` | `0` | Contractors should be provided by CRM | Vendors in Bitrix24 are not managed in CRM ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-documentcontractor-list.md)
- [{#T}](./catalog-documentcontractor-delete.md)
- [{#T}](./catalog-documentcontractor-get-fields.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contractor.md)
- [{#T}](../../../tutorials/crm/how-to-get-lists/how-to-get-contractors.md)
