# Get a List of Documents crm.documentgenerator.document.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for document generator documents

The method `crm.documentgenerator.document.list` returns a list of documents based on the filter.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | A list of fields that should be populated in the documents returned.

You can use:
- `'*'` — to select all standard fields of the document
- an explicit list of fields, for example, `['id','title','number','entityId','createTime']`

Refer to the [Document Type](#document) section for the list of fields.

By default, `['*']` is used. ||
|| **filter**
[`object`](../../data-types.md) | An object in the following format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — the name of the field for filtering
- `value_n` — the filter value

You can add prefixes to the keys `field_n`:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `=` — equal (default)
- `!=` or `!` — not equal

Refer to the [Document Type](#document) section for the list of available fields for filtering. ||
|| **order**
[`object`](../../data-types.md) | An object in the following format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — the name of the sorting field
- `value_n` — the sorting direction: `ASC` or `DESC`

Refer to the [Document Type](#document) section for the list of fields for sorting.

Example: `{"id":"DESC","createTime":"ASC"}` ||
|| **start**
[`integer`](../../data-types.md) | Pagination parameter.

The page size is fixed: `50` records.

The formula for obtaining the N-th page:
`start = (N - 1) * 50`

For more details, refer to the article [Features of List Methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of retrieving a list of documents where:
- fields `id`, `title`, `number`, `entityId`, `createTime` are selected
- sorted by `id` in descending order
- filtered by `entityTypeId = 2` and `entityId = 101`
- starting offset — `0`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","number","entityId","createTime"],"order":{"id":"desc"},"filter":{"entityTypeId":2,"entityId":101},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.document.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","number","entityId","createTime"],"order":{"id":"desc"},"filter":{"entityTypeId":2,"entityId":101},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.document.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentItem = {
      id: string
      title: string
      number: string
      templateId: string
      entityTypeId: string
      entityId: string
      fileId: string
      imageId: string
      pdfId: string
      createTime: ISODate
      updateTime: ISODate
      createdBy: string | null
      updatedBy: string | null
      values: Record<string, unknown> | null
      downloadUrl: string
      imageUrl: string
      pdfUrl: string
      stampsEnabled: boolean
      downloadUrlMachine: string
      pdfUrlMachine: string
      imageUrlMachine: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentListResult = {
      documents: DocumentItem[]
    }

    try {
      // crm.documentgenerator.document.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<DocumentListResult>({
        method: 'crm.documentgenerator.document.list',
        params: {
          select: ['id', 'title', 'number', 'entityId', 'createTime'],
          order: { id: 'desc' },
          filter: { entityTypeId: 2, entityId: 101 },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Documents on this page:', result.documents.length, result.documents)
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
      async function fetchDocumentList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.documentgenerator.document.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.document.list',
            params: {
              select: ['id', 'title', 'number', 'entityId', 'createTime'],
              order: { id: 'desc' },
              filter: { entityTypeId: 2, entityId: 101 },
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
          console.info('Documents on this page:', result.documents.length, result.documents)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchDocumentList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.documentgenerator.document.list',
                [
                    'select' => ['id', 'title', 'number', 'entityId', 'createTime'],
                    'order' => ['id' => 'desc'],
                    'filter' => ['entityTypeId' => 2, 'entityId' => 101],
                    'start' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting documents list: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.documentgenerator.document.list(
            select=["id", "title", "number", "entityId", "createTime"],
            order={"id": "desc"},
            filter={
                "entityTypeId": 2,
                "entityId": 101,
            },
            start=0,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    `as_list` Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.documentgenerator.document.list(
            select=["id", "title", "number", "entityId", "createTime"],
            order={"id": "desc"},
            filter={
                "entityTypeId": 2,
                "entityId": 101,
            },
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    `as_list_fast` Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.documentgenerator.document.list(
            select=["id", "title", "number", "entityId", "createTime"],
            order={"id": "desc"},
            filter={
                "entityTypeId": 2,
                "entityId": 101,
            },
        ).as_list_fast(descending=True).response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.document.list',
        {
            select: ['id', 'title', 'number', 'entityId', 'createTime'],
            order: { id: 'desc' },
            filter: { entityTypeId: 2, entityId: 101 },
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.info(result.data());

            if (result.more()) {
                result.next();
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.documentgenerator.document.list',
        [
            'select' => ['id', 'title', 'number', 'entityId', 'createTime'],
            'order' => ['id' => 'desc'],
            'filter' => ['entityTypeId' => 2, 'entityId' => 101],
            'start' => 0,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "documents": [
            {
                "id": "61",
                "title": "Demonstration implementation of product 2026-001",
                "number": "2026-002",
                "templateId": "39",
                "fileId": "283",
                "imageId": "285",
                "pdfId": "287",
                "createTime": "2026-03-20T13:51:45+03:00",
                "updateTime": "2026-03-20T14:42:38+03:00",
                "values": {
                    "_creationMethod": "rest",
                    "stampsEnabled": true
                },
                "createdBy": "577",
                "updatedBy": "577",
                "downloadUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.download&SITE_ID=s1&id=61",
                "pdfUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.getPdf&SITE_ID=s1&id=61",
                "imageUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.getImage&SITE_ID=s1&id=61",
                "stampsEnabled": true,
                "entityId": "101",
                "entityTypeId": "2",
                "downloadUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.download.json?auth=***&token=***",
                "pdfUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.getPdf.json?auth=***&token=***",
                "imageUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.getImage.json?auth=***&token=***"
            },
            {
                "id": "59",
                "title": "Demonstration implementation of product 2026-001",
                "number": "2026-001",
                "templateId": "39",
                "fileId": "271",
                "imageId": "273",
                "pdfId": "275",
                "createTime": "2026-03-20T13:28:26+03:00",
                "updateTime": "2026-03-20T13:28:26+03:00",
                "values": {
                    "_creationMethod": "rest",
                    "stampsEnabled": true
                },
                "createdBy": "577",
                "updatedBy": null,
                "downloadUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.download&SITE_ID=s1&id=59",
                "pdfUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.getPdf&SITE_ID=s1&id=59",
                "imageUrl": "https://bitrix.bitrix24.com/bitrix/services/main/ajax.php?action=crm.documentgenerator.document.getImage&SITE_ID=s1&id=59",
                "stampsEnabled": true,
                "entityId": "101",
                "entityTypeId": "2",
                "downloadUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.download.json?auth=***&token=***",
                "pdfUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.getPdf.json?auth=***&token=***",
                "imageUrlMachine": "https://bitrix.bitrix24.com/rest/crm.documentgenerator.document.getImage.json?auth=***&token=***"
            }
        ]
    },
    "total": 4,
    "time": {
        "start": 1774009414,
        "finish": 1774009414.09833,
        "duration": 0.09833002090454102,
        "processing": 0,
        "date_start": "2026-03-20T15:23:34+03:00",
        "date_finish": "2026-03-20T15:23:34+03:00",
        "operating_reset_at": 1774010014,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response. Contains the structure [`result`](#result) ||
|| **total**
[`integer`](../../data-types.md) | The total number of documents matching the filter ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. Returned if there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Type {#result}

#|
|| **Name**
`type` | **Description** ||
|| **documents**
[`object[]`](../../data-types.md) | An array of documents. The structure of each element is described in the [`document`](#document) type ||
|#

#### Document Type {#document}

The composition of fields depends on the `select` parameter.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Document identifier ||
|| **title**
[`string`](../../data-types.md) | Document name ||
|| **number**
[`string`](../../data-types.md) | Document number ||
|| **templateId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the document template ||
|| **entityTypeId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the CRM object type ||
|| **entityId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Identifier of the CRM object ||
|| **fileId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Document DOCX file identifier ||
|| **imageId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Document image identifier ||
|| **pdfId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Document PDF file identifier ||
|| **createTime**
[`datetime`](../../data-types.md) | Date of document creation ||
|| **updateTime**
[`datetime`](../../data-types.md) | Document update date ||
|| **createdBy**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the user who created the document ||
|| **updatedBy**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the user who updated the document ||
|| **values**
[`object`](../../data-types.md) \| [`null`](../../data-types.md) | Field values of the document ||
|| **downloadUrl**
[`string`](../../data-types.md) | Link to download the document ||
|| **imageUrl**
[`string`](../../data-types.md) | Link to the document image ||
|| **pdfUrl**
[`string`](../../data-types.md) | Link to the PDF document ||
|| **downloadUrlMachine**
[`string`](../../data-types.md) | Link to download the document for machine access ||
|| **pdfUrlMachine**
[`string`](../../data-types.md) | Link to the PDF document for machine access ||
|| **imageUrlMachine**
[`string`](../../data-types.md) | Link to the document image for machine access ||
|| **stampsEnabled**
[`boolean`](../../data-types.md) | Stamp and signature inclusion flag ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | Unknown field definition ENTITY_TYPE_ID (ENTITY_TYPE_ID) for \Bitrix\DocumentGenerator\Model\Document Entity | The `select` field passed in the `entityTypeId` parameter is not a direct field of the document model. Use `select: ['*']` ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to read documents based on the selected CRM object filter ||
|| Empty value | You do not have permissions to view documents | Insufficient permissions to view document generator documents ||
|| Empty value | Module documentgenerator is not installed | The `documentgenerator` module is not available ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-document-add.md)
- [{#T}](./crm-document-generator-document-get.md)
- [{#T}](./crm-document-generator-document-get-fields.md)
- [{#T}](./crm-document-generator-document-update.md)
- [{#T}](./crm-document-generator-document-delete.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)