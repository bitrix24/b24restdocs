# Get a List of Documents crm.documentgenerator.document.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.documentgenerator.document.list',
    		{
    			select: ['id', 'title', 'number', 'entityId', 'createTime'],
    			order: { id: 'desc' },
    			filter: { entityTypeId: 2, entityId: 101 },
    			start: 0,
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
    {
    	console.error('Error:', error);
    }
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

HTTP Status: **200**

```json
{
    "result": {
        "documents": [
            {
                "id": "61",
                "title": "Demo Product Implementation 2026-001",
                "number": "2026-002",
                "templateId": "39",
                "fileId": "283",
                "imageId": "285",
                "pdfId": "287",
                "createTime": "2026-03-20T13:51:45+02:00",
                "updateTime": "2026-03-20T14:42:38+02:00",
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
                "title": "Demo Product Implementation 2026-001",
                "number": "2026-001",
                "templateId": "39",
                "fileId": "271",
                "imageId": "273",
                "pdfId": "275",
                "createTime": "2026-03-20T13:28:26+02:00",
                "updateTime": "2026-03-20T13:28:26+02:00",
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
        "date_start": "2026-03-20T15:23:34+02:00",
        "date_finish": "2026-03-20T15:23:34+02:00",
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
[`object`](../../data-types.md) | The root object of the response. Contains the structure of [`result`](#result) ||
|| **total**
[`integer`](../../data-types.md) | The total number of documents matching the filter ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. Returned if there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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
[`string`](../../data-types.md) | Document title ||
|| **number**
[`string`](../../data-types.md) | Document number ||
|| **templateId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Document template identifier ||
|| **entityTypeId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | CRM object type identifier ||
|| **entityId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | CRM object identifier ||
|| **fileId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Document DOCX file identifier ||
|| **imageId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Document image identifier ||
|| **pdfId**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Document PDF file identifier ||
|| **createTime**
[`datetime`](../../data-types.md) | Document creation date ||
|| **updateTime**
[`datetime`](../../data-types.md) | Document update date ||
|| **createdBy**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the user who created the document ||
|| **updatedBy**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the user who updated the document ||
|| **values**
[`object`](../../data-types.md) \| [`null`](../../data-types.md) | Document field values ||
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
[`boolean`](../../data-types.md) | Indicates whether stamps and signatures can be applied ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | Unknown field definition ENTITY_TYPE_ID (ENTITY_TYPE_ID) for \Bitrix\DocumentGenerator\Model\Document Entity | The `entityTypeId` field passed in the `select` parameter is not a direct field of the document model. Use `select: ['*']` ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to read documents based on the selected CRM object filter ||
|| `Empty value` | You do not have permissions to view documents | Insufficient permissions to view document generator documents ||
|| `Empty value` | Module documentgenerator is not installed | The `documentgenerator` module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-document-add.md)
- [{#T}](./crm-document-generator-document-get.md)
- [{#T}](./crm-document-generator-document-get-fields.md)
- [{#T}](./crm-document-generator-document-update.md)
- [{#T}](./crm-document-generator-document-delete.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)