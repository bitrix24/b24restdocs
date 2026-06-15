# Create Inventory Document catalog.document.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Create and edit" permission for the required document type

The method `catalog.document.add` creates a new inventory document.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](#fields) | Document fields ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **docType***
[`char`](../../data-types.md) | Document type. Possible values:
- `A` — receipt,
- `S` — write-off,
- `M` — transfer between warehouses,
- `R` — return,
- `D` — disposal.

Current document types can be obtained using the method [catalog.enum.getStoreDocumentTypes](../enum/catalog-enum-get-store-document-types.md) ||
|| **currency***
[`crm_currency.CURRENCY`](../../crm/data-types.md#crm_currency) | Document currency in ISO 4217 format, for example `USD`. The value cannot be changed after creation.

To get a list of currencies with codes, use the method [crm.currency.list](../../crm/currency/crm-currency-list.md) ||
|| **responsibleId***
[`user.id`](../../data-types.md) | Identifier of the responsible person ||
|| **siteId**
[`char`](../../data-types.md) | Site code to which the document relates. Default is `s1`.

This parameter is relevant for on-premise Bitrix, for cloud Bitrix the standard value is `s1` ||
|| **dateDocument**
[`datetime`](../../data-types.md) | Document date in ISO 8601 format ||
|| **title**
[`string`](../../data-types.md) | Document title ||
|| **commentary**
[`char`](../../data-types.md) | Document commentary ||
|| **total**
[`double`](../../data-types.md) | Total amount for the document items. The value is calculated automatically after processing but can be set manually ||
|| **docNumber**
[`string`](../../data-types.md) | Internal document number. If not provided, it is generated automatically ||
|| **createdBy**
[`user.id`](../../data-types.md) | Identifier of the user who created the document. An administrator can specify any value; by default, it is filled with the current user ||
|#

{% note info "" %}

To fill in supplier data, use the method [catalog.documentcontractor.add](../documentcontractor/catalog-documentcontractor-add.md).
To fill in product data, use the method [catalog.document.element.add](./document-element/catalog-document-element-add.md).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docType":"A","currency":"USD","responsibleId":29,"docNumber":"IN-00042","title":"Receipt from Supplier-1","commentary":"Planned warehouse replenishment"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docType":"A","currency":"USD","responsibleId":29,"docNumber":"IN-00042","title":"Receipt from Supplier-1","commentary":"Planned warehouse replenishment"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentAddResult = {
      document: {
        commentary: string | null,
        createdBy: number,
        currency: string,
        dateCreate: ISODate | null,
        dateDocument: ISODate | null,
        dateModify: ISODate | null,
        dateStatus: ISODate | null,
        docNumber: string,
        docType: string,
        id: number,
        modifiedBy: number,
        responsibleId: number,
        siteId: string,
        status: string,
        statusBy: number | null,
        title: string | null,
        total: number | null,
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<DocumentAddResult>({
        method: 'catalog.document.add',
        params: {
          fields: {
            docType: 'A',
            currency: 'USD',
            responsibleId: 29,
            docNumber: 'IN-00042',
            title: 'Goods receipt from Supplier-1',
            commentary: 'Planned warehouse replenishment',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created document with ID:', result.document.id, 'status:', result.document.status)
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
      async function addDocument() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.document.add',
            params: {
              fields: {
                docType: 'A',
                currency: 'USD',
                responsibleId: 29,
                docNumber: 'IN-00042',
                title: 'Goods receipt from Supplier-1',
                commentary: 'Planned warehouse replenishment',
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
          console.info('Created document with ID:', result.document.id, 'status:', result.document.status)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addDocument)
    </script>
    ```

- PHP

    ```php
    try {
    $response = $b24Service
        ->core
        ->call(
            'catalog.document.add',
            [
                'fields' => [
                    'docType' => 'A',
                    'currency' => 'USD',
                    'responsibleId' => 29,
                    'docNumber' => 'IN-00042',
                    'title' => 'Receipt from Supplier-1',
                    'commentary' => 'Planned warehouse replenishment'
                ]
            ]
        );

    $result = $response
        ->getResponseData()
        ->getResult();

    echo 'Success: ' . print_r($result, true);
    processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding product row: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.add',
        {
            fields: {
                docType: 'A',
                currency: 'USD',
                responsibleId: 29,
                docNumber: 'IN-00042',
                title: 'Receipt from Supplier-1',
                commentary: 'Planned warehouse replenishment'
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
        'catalog.document.add',
        [
            'fields' => [
                'docType' => 'A',
                'currency' => 'USD',
                'responsibleId' => 29,
                'docNumber' => 'IN-00042',
                'title' => 'Receipt from Supplier-1',
                'commentary' => 'Planned warehouse replenishment'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP code: **200**

```json
{
    "result": {
        "document": {
            "commentary": "Planned warehouse replenishment",
            "createdBy": 29,
            "currency": "USD",
            "dateCreate": "2025-10-30T11:19:38+02:00",
            "dateDocument": null,
            "dateModify": "2025-10-30T11:19:38+02:00",
            "dateStatus": "2025-10-30T11:19:38+02:00",
            "docNumber": "IN-00042",
            "docType": "A",
            "id": 11,
            "modifiedBy": 29,
            "responsibleId": 29,
            "siteId": "s1",
            "status": "N",
            "statusBy": null,
            "title": "Receipt from Supplier-1",
            "total": null
        }
    },
    "time": {
        "start": 1761805178,
        "finish": 1761805178.991429,
        "duration": 0.9914290904998779,
        "processing": 0,
        "date_start": "2025-10-30T09:19:38+02:00",
        "date_finish": "2025-10-30T09:19:38+02:00",
        "operating_reset_at": 1761805778,
        "operating": 0.2595658302307129
    }
}
```

{% note info "" %}

The document is created with status `N` — draft. To process the document, use the method [catalog.document.conduct](./catalog-document-conduct.md) or [catalog.document.conductList](./catalog-document-conduct-list.md).

{% endnote %}

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md#catalog_document) | Root element of the response ||
|| **document**
[`catalog_document`](../data-types.md#catalog_document) | Object with data of the created document ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

HTTP code: **400**

```json
{
    "error": "0",
    "error_description": "DOC_TYPE isn't available"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Insufficient permissions to save the document | The user does not have permission to create the required document type ||
|| `0` | DOC_TYPE isn't available | An invalid document type was provided ||
|| `0` | Inventory accounting is not available on your plan | Inventory accounting is not available on your plan ||
|| `0` | -  | Other internal errors when adding the document ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./document-element/catalog-document-element-add.md)
- [{#T}](../documentcontractor/catalog-documentcontractor-add.md)
- [{#T}](./catalog-document-update.md)
- [{#T}](./catalog-document-conduct.md)