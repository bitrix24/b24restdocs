# Get Document Type Field catalog.document.type.field.get

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

The `catalog.document.type.field.get` method returns the description of an inventory document type field by its name.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the field whose description you need to retrieve.

Available fields:

- `id` — document type code
- `name` — document type name ||
|| **select**
[`array`](../../data-types.md) | An array of strings with the names of description properties to return in the response.

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

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.type.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"name"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.type.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"name","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.type.field.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldDescription = {
      name?: string
      type?: string
      title?: string
      description?: string | null
      validationRules?: unknown[]
      requiredGroups?: string[] | null
      filterable?: boolean
      sortable?: boolean
      editable?: boolean
      editableGroups?: string[] | null
      multiple?: boolean
      elementType?: string | null
    }

    type TypeFieldGetResult = {
      item: FieldDescription
    }

    try {
      const response = await $b24.actions.v3.call.make<TypeFieldGetResult>({
        method: 'catalog.document.type.field.get',
        params: {
          name: 'name',
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field:', result.item)
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
      async function getTypeField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.type.field.get',
            params: {
              name: 'name',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTypeField)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.type.field.get(
            name="name",
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
                'catalog.document.type.field.get',
                [
                    'name' => 'name',
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
        'catalog.document.type.field.get',
        {
            name: 'name',
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
        'catalog.document.type.field.get',
        [
            'name' => 'name',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.type.field.get", b24.Params{
    	"name": "name",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.document.type.field.get: %w", err)
    }

    var result struct {
    	Item struct {
    		Name string `json:"name"`
    		Type string `json:"type"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.Name, result.Item.Type)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "name": "name",
            "type": "string",
            "title": "name",
            "description": null,
            "validationRules": [],
            "requiredGroups": null,
            "filterable": false,
            "sortable": false,
            "editable": false,
            "editableGroups": null,
            "multiple": false,
            "elementType": null
        }
    },
    "time": {
        "start": 1792665221,
        "finish": 1792665221.108471,
        "duration": 0.10847091674804688,
        "processing": 0,
        "date_start": "2026-10-22T13:33:41+02:00",
        "date_finish": "2026-10-22T13:33:41+02:00",
        "operating_reset_at": 1792665780,
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
[`object`](../../data-types.md) | Field description. [Description properties](#item) depend on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Field Description Object {#item}

The set of description properties depends on `select`. The table contains all properties that can be requested.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field name: `id`, `name` ||
|| **type**
[`string`](../../data-types.md) | Data type: `string` ||
|| **title**
[`string`](../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../data-types.md) or `null` | Field description. For the fields of this object — `null` ||
|| **validationRules**
[`array`](../../data-types.md) | Validation rules. For the fields of this object — an empty array ||
|| **requiredGroups**
[`array`](../../data-types.md) or `null` | Operations in which the field is required. For the fields of this object — `null` ||
|| **filterable**
[`boolean`](../../data-types.md) | `true` — the field is available in the filter, `false` — it is not. For the fields of this object — `false` ||
|| **sortable**
[`boolean`](../../data-types.md) | `true` — the field is available for sorting, `false` — it is not. For the fields of this object — `false` ||
|| **editable**
[`boolean`](../../data-types.md) | `true` — the field can be passed when writing, `false` — it cannot. For the fields of this object — `false` ||
|| **editableGroups**
[`array`](../../data-types.md) or `null` | Operations in which the field is editable. For the fields of this object — `null` ||
|| **multiple**
[`boolean`](../../data-types.md) | `true` — multiple value, `false` — single value. For the fields of this object — `false` ||
|| **elementType**
[`string`](../../data-types.md) or `null` | Element type of a composite field. For the fields of this object — `null` ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION",
        "message": "Field \"code\" was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is missing | Pass the `name` parameter with the field name ||
|#

#### Field Not Found Error

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Field `#FIELD#` was not found | Specify a field name from the list: `id`, `name` ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | Pass only fields from the description of the `select` parameter ||
|#

#### `select` Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-type-field-list.md)
- [{#T}](./catalog-document-type-list.md)
