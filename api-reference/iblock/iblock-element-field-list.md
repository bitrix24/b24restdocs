# Get List of Information Block Element Fields iblock.element.field.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`iblock`](../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

The method `iblock.element.field.list` returns a list of available information block element fields.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../data-types.md) | Array of strings with the names of the description properties to return in the response.

Default: all description properties.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required groups
- `filterable` — whether the field is available in the filter
- `sortable` — whether the field is available for sorting
- `editable` — whether the field is editable
- `editableGroups` — operation groups in which the field is editable
- `multiple` — whether the field is multiple
- `elementType` — element type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/iblock.element.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","filterable","sortable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/iblock.element.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","filterable","sortable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/iblock.element.field.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ElementFieldListResult = {
      items: Array<{
        name: string
        type: string
        filterable: boolean
        sortable: boolean
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<ElementFieldListResult>({
        method: 'iblock.element.field.list',
        params: {
          select: ['name', 'type', 'filterable', 'sortable'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach((field) => console.info(field.name, field.type))
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
      async function getElementFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'iblock.element.field.list',
            params: {
              select: ['name', 'type', 'filterable', 'sortable'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach((field) => console.info(field.name, field.type))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getElementFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.iblock.element.field.list(
            select=["name", "type", "filterable", "sortable"],
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
                'iblock.element.field.list',
                [
                    'select' => ['name', 'type', 'filterable', 'sortable'],
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
        'iblock.element.field.list',
        {
            select: ['name', 'type', 'filterable', 'sortable'],
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
        'iblock.element.field.list',
        [
            'select' => ['name', 'type', 'filterable', 'sortable'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "iblock.element.field.list", b24.Params{
    	"select": []string{"name", "type", "filterable", "sortable"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("iblock.element.field.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		Name       string `json:"name"`
    		Type       string `json:"type"`
    		Filterable bool   `json:"filterable"`
    		Sortable   bool   `json:"sortable"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, field := range result.Items {
    	fmt.Println(field.Name, field.Type)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "name": "id",
                "type": "int",
                "filterable": true,
                "sortable": true
            },
            {
                "name": "iblockId",
                "type": "int",
                "filterable": true,
                "sortable": false
            },
            {
                "name": "name",
                "type": "string",
                "filterable": true,
                "sortable": true
            },
            {
                "name": "iblockSectionId",
                "type": "int",
                "filterable": true,
                "sortable": true
            },
            {
                "name": "properties",
                "type": "array",
                "filterable": false,
                "sortable": false
            }
        ]
    },
    "time": {
        "start": 1792670785,
        "finish": 1792670785.424705,
        "duration": 0.42470502853393555,
        "processing": 0,
        "date_start": "2026-10-22T15:06:25+02:00",
        "date_finish": "2026-10-22T15:06:25+02:00",
        "operating_reset_at": 1792671360,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../data-types.md) | Array of information block element field descriptions. The [element properties](#items) depend on `select` ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

The set of description properties depends on `select`. The table contains all properties that can be requested.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../data-types.md) | Field name:

- `id` — element identifier
- `iblockId` — information block identifier
- `name` — element name
- `iblockSectionId` — information block section identifier
- `properties` — element property values. The object keys are property symbolic codes ||
|| **type**
[`string`](../data-types.md) | Type in the metadata: `int`, `string`, `array` ||
|| **title**
[`string`](../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../data-types.md) or `null` | Field description. For the fields of this object — `null` ||
|| **validationRules**
[`array`](../data-types.md) | Array of validation rules. Contains two rules for `iblockId` and `[]` for the other fields ||
|| **requiredGroups**
[`array`](../data-types.md) or `null` | Array of strings with the names of operations in which the field is required. For the fields of this object — `null` ||
|| **filterable**
[`boolean`](../data-types.md) | `true` — the field is available for filtering, `false` — it is not. `false` for `properties`, `true` for the other fields ||
|| **sortable**
[`boolean`](../data-types.md) | `true` — the field is available for sorting, `false` — it is not. `false` for `iblockId` and `properties`, `true` for the other fields ||
|| **editable**
[`boolean`](../data-types.md) | `true` — the field is editable, `false` — read-only. `false` for all fields: the information block element methods only read data ||
|| **editableGroups**
[`array`](../data-types.md) or `null` | Array of strings with the names of operations in which the field can be set. `null` for all fields ||
|| **multiple**
[`boolean`](../data-types.md) | Multiple value flag: `true` — multiple, `false` — single. `false` for all fields ||
|| **elementType**
[`string`](../data-types.md) or `null` | Element type of a composite field. `null` for all fields ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field \"bad\" found in entity \"DtoFieldDto\"."
    }
}
```

{% include notitle [Error handling](../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the webhook or app has the `iblock` scope ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | Pass only the fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `editableGroups`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./iblock-element-field-get.md)
- [{#T}](./iblock-element-get.md)
- [{#T}](./iblock-element-list.md)
- [{#T}](./index.md)
