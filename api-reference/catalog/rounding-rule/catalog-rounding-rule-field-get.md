# Get Price Rounding Rule Field catalog.roundingrule.field.get

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

The method `catalog.roundingrule.field.get` returns the description of a price rounding rule field by its name.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the field whose description you need to retrieve.

Available fields:

- `id` — rounding rule identifier
- `catalogGroupId` — price type identifier
- `price` — minimum price from which the rule applies
- `roundType` — rounding type
- `roundPrecision` — rounding precision
- `createdBy` — identifier of the rule author
- `dateCreate` — creation date and time
- `modifiedBy` — identifier of the user who last modified the rule
- `dateModify` — date and time of the last modification ||
|| **select**
[`array`](../../data-types.md) | Array of strings with the names of the description properties to return in the response.

By default, all description properties are returned. The `["*"]` value is not supported.

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
- `editableGroups` — groups of operations in which the field is editable
- `multiple` — whether the field has multiple values
- `elementType` — element type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.roundingrule.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"roundType","select":["name","type","requiredGroups","editableGroups"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.roundingrule.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"roundType","select":["name","type","requiredGroups","editableGroups"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.roundingrule.field.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RoundingRuleFieldGetResult = {
      item: {
        name?: string
        type?: string
        requiredGroups?: string[] | null
        editableGroups?: string[] | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<RoundingRuleFieldGetResult>({
        method: 'catalog.roundingrule.field.get',
        params: {
          name: 'roundType',
          select: ['name', 'type', 'requiredGroups', 'editableGroups'],
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
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.roundingrule.field.get',
            params: {
              name: 'roundType',
              select: ['name', 'type', 'requiredGroups', 'editableGroups'],
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
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getField)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.roundingrule.field.get(
            name="roundType",
            select=["name", "type", "requiredGroups", "editableGroups"],
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
                'catalog.roundingrule.field.get',
                [
                    'name' => 'roundType',
                    'select' => ['name', 'type', 'requiredGroups', 'editableGroups'],
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
        'catalog.roundingrule.field.get',
        {
            name: 'roundType',
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
        'catalog.roundingrule.field.get',
        [
            'name' => 'roundType',
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
    res, err := client.Core().Call(ctx, "catalog.roundingrule.field.get", b24.Params{
    	"name":   "roundType",
    	"select": []string{"name", "type", "requiredGroups", "editableGroups"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.roundingrule.field.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
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
            "name": "roundType",
            "type": "int",
            "requiredGroups": [
                "add"
            ],
            "editableGroups": [
                "add",
                "update"
            ]
        }
    },
    "time": {
        "start": 1792668082,
        "finish": 1792668082.274764,
        "duration": 0.2747640609741211,
        "processing": 0,
        "date_start": "2026-10-22T14:21:22+02:00",
        "date_finish": "2026-10-22T14:21:22+02:00",
        "operating_reset_at": 1792668660,
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
|| **result.item**
[`object`](../../data-types.md) | Field description. [Object properties](#item) depend on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

The content of the description depends on `select`. The table lists all properties that can be requested.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field name: `id`, `catalogGroupId`, `price`, `roundType`, `roundPrecision`, `createdBy`, `dateCreate`, `modifiedBy`, `dateModify` ||
|| **type**
[`string`](../../data-types.md) | Type in the metadata: `int`, `float`, `object`. For `dateCreate` and `dateModify`, `object` is returned, although method responses pass the date as an ISO 8601 string ||
|| **title**
[`string`](../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../data-types.md) or `null` | Field description. For the fields of this object — `null` ||
|| **validationRules**
[`array`](../../data-types.md) | Validation rules. For the fields of this object — `[]`. The allowed values of `roundType` and `roundPrecision` are checked on write and are not passed in the metadata ||
|| **requiredGroups**
[`array`](../../data-types.md) or `null` | Array of strings with the names of operations in which the field is required. For `catalogGroupId`, `price`, `roundType`, `roundPrecision` — `["add"]`; for the other fields — `null` ||
|| **filterable**
[`boolean`](../../data-types.md) | `true` — the field is available for filtering, `false` — not available. For all fields — `true` ||
|| **sortable**
[`boolean`](../../data-types.md) | `true` — the field is available for sorting, `false` — not available. For all fields — `true` ||
|| **editable**
[`boolean`](../../data-types.md) | `true` — the field can be passed in the operations from `editableGroups`, `false` — it cannot. For `id`, `createdBy`, `dateCreate`, `modifiedBy`, `dateModify` — `false` ||
|| **editableGroups**
[`array`](../../data-types.md) or `null` | Array of strings with the names of operations in which the field can be set. For `catalogGroupId`, `price`, `roundType`, `roundPrecision` — `["add", "update"]`; for `id`, `createdBy`, `dateCreate`, `modifiedBy`, `dateModify` — `null` ||
|| **multiple**
[`boolean`](../../data-types.md) | Multiple value flag: `true` — multiple, `false` — single. For all fields — `false` ||
|| **elementType**
[`string`](../../data-types.md) or `null` | Element type of a composite field. For all fields — `null` ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION",
        "message": "Field \"foo\" was not found."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is missing | Pass the `name` parameter with an existing field name ||
|#

#### Field Not Found Error

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

HTTP status: **404**.

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Field `#FIELD#` was not found | Specify a field name from the list: `id`, `catalogGroupId`, `price`, `roundType`, `roundPrecision`, `createdBy`, `dateCreate`, `modifiedBy`, `dateModify` ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | Pass only fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `editableGroups`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example, `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-rounding-rule-field-list.md)
- [{#T}](./catalog-rounding-rule-add-rest-v3.md)
- [{#T}](./catalog-rounding-rule-get-rest-v3.md)
