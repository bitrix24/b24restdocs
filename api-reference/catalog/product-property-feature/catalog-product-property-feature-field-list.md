# Get List of Property Parameter Fields v 3.0 catalog.product.property.feature.field.list

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

The `catalog.product.property.feature.field.list` method returns the list of available property parameter fields.

A property parameter defines how the property is used in the catalog: for example, whether it is shown in the product list and whether its value is added to the shopping cart.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | Array of strings with the names of the description properties to return for each field.

Available values:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required groups
- `filterable` — availability in the filter
- `sortable` — availability in sorting
- `editable` — editability flag
- `editableGroups` — groups of operations in which the field can be edited
- `multiple` — multiple value flag
- `elementType` — element type for composite fields

Default: all description properties ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.feature.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","requiredGroups","editableGroups"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.feature.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","requiredGroups","editableGroups"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.feature.field.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FeatureFieldListResult = {
      items: Array<{
        name?: string
        type?: string
        requiredGroups?: string[] | null
        editableGroups?: string[] | null
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<FeatureFieldListResult>({
        method: 'catalog.product.property.feature.field.list',
        params: {
            select: ['name', 'type', 'requiredGroups', 'editableGroups'],
          },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Fields:', result.items.map((field) => field.name))
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
      async function listFeatureFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.feature.field.list',
            params: {
                  select: ['name', 'type', 'requiredGroups', 'editableGroups'],
                },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Fields:', result.items.map((field) => field.name))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listFeatureFields)
    </script>
    ```

- Python

    The `catalog.product.property.feature.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.feature.field.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "select": ["name", "type", "requiredGroups", "editableGroups"],
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
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
                    'catalog.product.property.feature.field.list',
                    [
                        'select' => ['name', 'type', 'requiredGroups', 'editableGroups'],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.product.property.feature.field.list',
        {
            select: ['name', 'type', 'requiredGroups', 'editableGroups'],
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
        'catalog.product.property.feature.field.list',
        [
            'select' => ['name', 'type', 'requiredGroups', 'editableGroups'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.property.feature.field.list", b24.Params{
    	"select": []string{"name", "type", "requiredGroups", "editableGroups"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.property.feature.field.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		Name           string   `json:"name"`
    		Type           string   `json:"type"`
    		EditableGroups []string `json:"editableGroups"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.Name, item.Type, item.EditableGroups)
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
                "requiredGroups": null,
                "editableGroups": null
            },
            {
                "name": "propertyId",
                "type": "int",
                "requiredGroups": [
                    "add"
                ],
                "editableGroups": [
                    "add"
                ]
            },
            {
                "name": "moduleId",
                "type": "string",
                "requiredGroups": [
                    "add"
                ],
                "editableGroups": [
                    "add"
                ]
            },
            {
                "name": "featureId",
                "type": "string",
                "requiredGroups": [
                    "add"
                ],
                "editableGroups": [
                    "add"
                ]
            },
            {
                "name": "isEnabled",
                "type": "bool",
                "requiredGroups": [
                    "add"
                ],
                "editableGroups": [
                    "add",
                    "update"
                ]
            }
        ]
    },
    "time": {
        "start": 1794923355,
        "finish": 1794923355.498935,
        "duration": 0.4989349842071533,
        "processing": 0,
        "date_start": "2026-11-17T16:49:15+02:00",
        "date_finish": "2026-11-17T16:49:15+02:00",
        "operating_reset_at": 1794923940,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | Array of parameter field descriptions. The description content depends on `select`. [Description properties](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

The table lists all properties that can be requested in `select`.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field name: `id`, `propertyId`, `moduleId`, `featureId`, `isEnabled` ||
|| **type**
[`string`](../../data-types.md) | Type in the metadata: `int`, `string`, `bool` ||
|| **title**
[`string`](../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../data-types.md) or `null` | Field description. For the fields of this object — `null` ||
|| **validationRules**
[`array`](../../data-types.md) | Array of validation rules. For the fields of this object — `[]` ||
|| **requiredGroups**
[`array`](../../data-types.md) or `null` | Array of operations in which the field is required. For `propertyId`, `moduleId`, `featureId`, `isEnabled` — `["add"]`, for `id` — `null` ||
|| **filterable**
[`boolean`](../../data-types.md) | `true` — the field is available for filtering. For all fields — `true` ||
|| **sortable**
[`boolean`](../../data-types.md) | `true` — the field is available for sorting. For `isEnabled` — `false`, for the other fields — `true` ||
|| **editable**
[`boolean`](../../data-types.md) | `true` — the field can be passed in `fields` of the [catalog.product.property.feature.add](./catalog-product-property-feature-add-rest-v3.md) and [catalog.product.property.feature.update](./catalog-product-property-feature-update-rest-v3.md) methods, `false` — the field is read-only. For `id` — `false`, for the other fields — `true` ||
|| **editableGroups**
[`array`](../../data-types.md) or `null` | Array of operations in which the field can be set:

- `propertyId`, `moduleId`, `featureId` — `["add"]`
- `isEnabled` — `["add", "update"]`
- `id` — `null` ||
|| **multiple**
[`boolean`](../../data-types.md) | Multiple value flag. For all fields — `false` ||
|| **elementType**
[`string`](../../data-types.md) or `null` | Element type of a composite field. For all fields — `null` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field \"foo\" found in entity \"DtoFieldDto\"."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Errors in the select Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | Pass only the values from the `select` parameter list ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check that the application or webhook has the `catalog` scope ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-feature-add-rest-v3.md)
- [{#T}](./catalog-product-property-feature-list-rest-v3.md)
- [{#T}](./catalog-product-property-feature-field-get.md)
