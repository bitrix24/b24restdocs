# Get Document Field note.document.field.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.document.field.get` method returns a description of a document field by name.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | The name of the field whose description needs to be retrieved.

Available fields:

- `id` — document identifier
- `collectionId` — knowledge base identifier
- `parentId` — parent document identifier
- `title` — document title
- `markdown` — document content in Markdown
- `position` — document position among neighboring pages
- `createdBy` — document author identifier
- `createdAt` — creation date and time
- `updatedBy` — last editor identifier
- `updatedAt` — last modification date and time
- `contentUpdatedAt` — last content modification date and time
- `isArchived` — archive flag
- `isTrashed` — trash flag
- `isOrphan` — indicates that the document has no knowledge base ||
|| **select**
[`array`](../../data-types.md) | An array of description property names to return in the response.

By default, all description properties are returned. An empty array `[]` also returns all properties. `["*"]` is not supported.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — mandatory groups
- `filterable` — filter availability flag
- `sortable` — sorting availability flag
- `editable` — editability flag
- `editableGroups` — groups of operations in which the field is editable
- `multiple` — multiple value flag
- `elementType` — item type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.document.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"title","select":["name","type","title"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.document.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"title","select":["name","type","title"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.document.field.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentFieldGetResult = {
      item: {
        name?: string
        type?: string
        title?: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentFieldGetResult>({
        method: 'note.document.field.get',
        params: {
          name: 'title',
          select: [
            'name',
            'type',
            'title',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field description:', result.item)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getDocumentField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.document.field.get',
            params: {
              name: 'title',
              select: [
                'name',
                'type',
                'title',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field description:', result.item)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDocumentField)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    select = [
        "name",
        "type",
        "title",
    ]

    try:
        bitrix_response = client.note.document.field.get(
            name='title',
            select=select,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.document.field.get',
                [
                    'name' => 'title',
                    'select' => [
                        'name',
                        'type',
                        'title'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.document.field.get',
        {
            name: 'title',
            select: [
                'name',
                'type',
                'title'
            ]
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'note.document.field.get',
        [
            'name' => 'title',
            'select' => [
                'name',
                'type',
                'title'
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
    res, err := client.Core().Call(ctx, "note.document.field.get", b24.Params{
    	"name":   "title",
    	"select": []string{"name", "type", "title"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("note.document.field.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	Name  string `json:"name"`
    	Type  string `json:"type"`
    	Title string `json:"title"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Name, item.Type)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "name": "title",
            "type": "string",
            "title": "title"
        }
    },
    "time": {
        "start": 1791205954,
        "finish": 1791205954.530845,
        "duration": 0.5308449268341064,
        "processing": 0,
        "date_start": "2026-10-05T16:12:34+03:00",
        "date_finish": "2026-10-05T16:12:34+03:00",
        "operating_reset_at": 1791206554,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data ||
|| **item**
[`object`](../../data-types.md) | Field description in `result.item`. [Object properties](#item) depend on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

The description depends on `select`. The table lists all properties that can be requested.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field name: `id`, `collectionId`, `parentId`, `title`, `markdown`, `position`, `createdBy`, `updatedBy`, `createdAt`, `updatedAt`, `contentUpdatedAt`, `isArchived`, `isTrashed`, `isOrphan` ||
|| **type**
[`string`](../../data-types.md) | Metadata type: `int`, `string`, `object`, `bool`. Date fields return `object`, although method responses contain ISO 8601 strings ||
|| **title**
[`string`](../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../data-types.md) or `null` | Field description. `null` for these fields ||
|| **validationRules**
[`array`](../../data-types.md) | Array of validation rule objects. For `title`, two objects `[{}, {}]` are returned; for other fields, `[]`. Rule parameters are not serialized. Title and Markdown constraints are described in [note.document.add](./note-document-add.md) ||
|| **requiredGroups**
[`array`](../../data-types.md) or `null` | Array of operation names for which the field is required. For `collectionId` and `title`, `["add"]`; for other fields, `null` ||
|| **filterable**
[`boolean`](../../data-types.md) | `true` if filtering is supported; `false` otherwise. For all fields, `false` ||
|| **sortable**
[`boolean`](../../data-types.md) | `true` if sorting is supported; `false` otherwise. `false` for all fields ||
|| **editable**
[`boolean`](../../data-types.md) | `true` if the field can be passed in operations from `editableGroups`; `false` otherwise. Does not replace a user permission check ||
|| **editableGroups**
[`array`](../../data-types.md) or `null` | Array of operation names in which the field can be set. For `collectionId` and `parentId`, `["add"]`; for `title` and `markdown`, `["add", "update"]`; for other fields, `null` ||
|| **multiple**
[`boolean`](../../data-types.md) | `true` for multiple values; `false` for a single value. For all fields, `false` ||
|| **elementType**
[`string`](../../data-types.md) or `null` | Type of an element in a composite field. For all fields, `null` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "Mandatory field `name` is missing",
                "field": "name"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check user permissions and scope `note` ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

HTTP status: **404**.

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Field `#FIELD#` not found | Specify an existing field name ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Mandatory field `name` is not specified | Pass the `name` parameter with an existing field name ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for entity `DtoFieldDto` | Only pass fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `editableGroups`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings, e.g., `["name","type"]` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-field-list.md)
- [{#T}](./note-document-get.md)
- [{#T}](./index.md)
