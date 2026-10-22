# Get Inventory Document Field catalog.document.field.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.document.field.get` method returns the description of an inventory document field by its name.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the field whose description you need to retrieve.

Available fields:

- `id` — document identifier
- `docType` — document type
- `siteId` — site identifier
- `responsibleId` — identifier of the responsible person
- `dateModify` — modification date
- `dateCreate` — creation date
- `createdBy` — created by
- `modifiedBy` — modified by
- `currency` — currency
- `status` — processing status
- `dateStatus` — status change date
- `dateDocument` — document date
- `statusBy` — status changed by
- `total` — total amount
- `commentary` — comment
- `title` — title
- `docNumber` — incoming document number
- `contractorId` — vendor identifier ||
|| **select**
[`array`](../../data-types.md) | An array of description properties to return in the response.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — groups in which the field is required
- `filterable` — whether the field is available in the filter
- `sortable` — whether the field is available for sorting
- `editable` — whether the field is editable
- `editableGroups` — groups of operations in which the field is editable
- `multiple` — whether the field has multiple values
- `elementType` — element type for composite fields

Default: all description properties ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter to the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"docType","select":["name","type","title","requiredGroups","editableGroups"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"docType","select":["name","type","title","requiredGroups","editableGroups"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.field.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentFieldGetResult = {
      item: {
        name: string
        type: string
        title: string
        requiredGroups: string[] | null
        editableGroups: string[] | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentFieldGetResult>({
        method: 'catalog.document.field.get',
        params: {
          name: 'docType',
          select: ['name', 'type', 'title', 'requiredGroups', 'editableGroups'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field:', result.item.name, result.item.type)
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
      async function callDocumentFieldGet() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.field.get',
            params: {
              name: 'docType',
              select: ['name', 'type', 'title', 'requiredGroups', 'editableGroups'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field:', result.item.name, result.item.type)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', callDocumentFieldGet)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.field.get(
            name="docType",
            select=["name", "type", "title", "requiredGroups", "editableGroups"],
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
                'catalog.document.field.get',
                [
                    'name' => 'docType',
                    'select' => ['name', 'type', 'title', 'requiredGroups', 'editableGroups'],
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
        'catalog.document.field.get',
        {
            name: 'docType',
            select: ['name', 'type', 'title', 'requiredGroups', 'editableGroups'],
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
        'catalog.document.field.get',
        [
            'name' => 'docType',
            'select' => ['name', 'type', 'title', 'requiredGroups', 'editableGroups'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.field.get", b24.Params{
    	"name":   "docType",
    	"select": []string{"name", "type", "title", "requiredGroups", "editableGroups"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.document.field.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	Name           string   `json:"name"`
    	Type           string   `json:"type"`
    	RequiredGroups []string `json:"requiredGroups"`
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
            "name": "docType",
            "type": "string",
            "title": "docType",
            "requiredGroups": [
                "add"
            ],
            "editableGroups": [
                "add"
            ]
        }
    },
    "time": {
        "start": 1792665253,
        "finish": 1792665253.905826,
        "duration": 0.9058260917663574,
        "processing": 0,
        "date_start": "2026-10-22T13:34:13+02:00",
        "date_finish": "2026-10-22T13:34:13+02:00",
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
[`object`](../../data-types.md) | An object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Field description. The set of properties depends on `select`. [Property description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Document field name ||
|| **type**
[`string`](../../data-types.md) | Type in the metadata: `int`, `string`, `float`, `object`. For date fields, `object` is returned, although date values are passed as ISO 8601 strings in method responses ||
|| **title**
[`string`](../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../data-types.md) or `null` | Field description. For document fields — `null` ||
|| **validationRules**
[`array`](../../data-types.md) | Validation rules. For document fields — an empty array ||
|| **requiredGroups**
[`array`](../../data-types.md) or `null` | Operations in which the field is required. For `docType`, `currency`, and `responsibleId` — `["add"]` ||
|| **filterable**
[`boolean`](../../data-types.md) | `true` — the field is available in the filter, `false` — it is not ||
|| **sortable**
[`boolean`](../../data-types.md) | `true` — the field is available for sorting, `false` — it is not ||
|| **editable**
[`boolean`](../../data-types.md) | `true` — the field can be passed in the operations from `editableGroups` ||
|| **editableGroups**
[`array`](../../data-types.md) or `null` | Operations in which the field can be set. For `docType` and `currency` — `["add"]` ||
|| **multiple**
[`boolean`](../../data-types.md) | Multiple value flag. For document fields — `false` ||
|| **elementType**
[`string`](../../data-types.md) or `null` | Element type of a composite field. For document fields — `null` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION",
        "message": "Field \"foo\" was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Field Not Found Error

HTTP status: **404**

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Field `#FIELD#` was not found | Specify a field name from the description of the `name` parameter ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is missing | Pass the `name` parameter ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | In `select`, specify only properties from the description of the `select` parameter ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user permissions and the `catalog` scope ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-field-list.md)
- [{#T}](./catalog-document-add-rest-v3.md)
- [{#T}](./catalog-document-get.md)
