# Get Custom Field List Values forum.message.field.custom.enum.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`forum`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `forum.message.field.custom.enum.list` method returns all values of a forum message custom field of the List type.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../data-types.md) | Identifier of the custom field whose values you need to retrieve. Pass a single condition in the `["fieldId", identifier]` format, for example `["fieldId", 6008093]`.

The method accepts the filter only in this form. The standard REST 3.0 format — an array of conditions `[["fieldId", 6008093]]` or `[["fieldId", "=", 6008093]]` — causes an error.

The field must be of the `enumeration` type. The identifier can be retrieved using the [forum.message.field.custom.list](./forum-message-field-custom-list.md) method ||
|| **select**
[`array`](../../data-types.md) | Array of value fields to return in the response.

Available fields:

- `id` — value identifier
- `fieldId` — custom field identifier
- `value` — value text
- `isDefault` — default value flag
- `sort` — sort order
- `xmlId` — external code of the value

Default: all fields ||
|#

{% note info "" %}

The method returns all values of the field in a single request, sorted by `sort` in ascending order. The `order` and `pagination` parameters do not affect the result. Sorting by any field other than `id` causes a validation error.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/forum.message.field.custom.enum.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":["fieldId",6008093],"select":["id","value","isDefault","sort","xmlId"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/forum.message.field.custom.enum.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":["fieldId",6008093],"select":["id","value","isDefault","sort","xmlId"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/forum.message.field.custom.enum.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EnumValueListResult = {
      items: {
        id: number
        value: string
        isDefault: boolean
        sort: number
        xmlId: string
      }[]
    }

    try {
      const response = await $b24.actions.v3.call.make<EnumValueListResult>({
        method: 'forum.message.field.custom.enum.list',
        params: {
          filter: ['fieldId', 6008093],
          select: ['id', 'value', 'isDefault', 'sort', 'xmlId'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(item => console.info(item.id, item.value))
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
      async function listEnumValues() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'forum.message.field.custom.enum.list',
            params: {
              filter: ['fieldId', 6008093],
              select: ['id', 'value', 'isDefault', 'sort', 'xmlId'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(item => console.info(item.id, item.value))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listEnumValues)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.forum.message.field.custom.enum.list(
            filter=["fieldId", 6008093],
            select=["id", "value", "isDefault", "sort", "xmlId"],
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
                'forum.message.field.custom.enum.list',
                [
                    'filter' => ['fieldId', 6008093],
                    'select' => ['id', 'value', 'isDefault', 'sort', 'xmlId'],
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
        'forum.message.field.custom.enum.list',
        {
            filter: ['fieldId', 6008093],
            select: ['id', 'value', 'isDefault', 'sort', 'xmlId'],
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
        'forum.message.field.custom.enum.list',
        [
            'filter' => ['fieldId', 6008093],
            'select' => ['id', 'value', 'isDefault', 'sort', 'xmlId'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "forum.message.field.custom.enum.list", b24.Params{
    	"filter": []any{"fieldId", 6008093},
    	"select": []string{"id", "value", "isDefault", "sort", "xmlId"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("forum.message.field.custom.enum.list: %w", err)
    }

    // The method wraps the response in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("no items key in the response")
    }

    var items []struct {
    	ID        int    `json:"id"`
    	Value     string `json:"value"`
    	IsDefault bool   `json:"isDefault"`
    	Sort      int    `json:"sort"`
    	XMLID     string `json:"xmlId"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range items {
    	fmt.Println(item.ID, item.Value)
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
                "id": 4077,
                "value": "Urgent",
                "isDefault": false,
                "sort": 5,
                "xmlId": "HIGH"
            },
            {
                "id": 4079,
                "value": "Normal",
                "isDefault": true,
                "sort": 20,
                "xmlId": "NORMAL"
            },
            {
                "id": 4081,
                "value": "Low",
                "isDefault": true,
                "sort": 30,
                "xmlId": "LOW"
            }
        ]
    },
    "time": {
        "start": 1792670773,
        "finish": 1792670773.884,
        "duration": 0.8840000629425049,
        "processing": 0,
        "date_start": "2026-10-22T15:06:13+02:00",
        "date_finish": "2026-10-22T15:06:13+02:00",
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
[`object`](../../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | Array of field values. The set of fields depends on `select`. If the field has no values, the array is empty. [Field description](#items) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | List value identifier ||
|| **fieldId**
[`integer`](../../data-types.md) | Identifier of the custom field the value belongs to ||
|| **value**
[`string`](../../data-types.md) | Value text ||
|| **isDefault**
[`boolean`](../../data-types.md) | `true` — default value, `false` — regular value. Several values of the field can have the flag ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the value ||
|#

## Error Handling

HTTP status: **400**, **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION",
        "message": "Error validating request object filters.",
        "validation": [
            {
                "message": "Filter for required field \"fieldId\" is required.",
                "field": "filter.fieldId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.fieldId` | Filter for required field `fieldId` is required | Pass the filter as a single condition `["fieldId", 6008093]`, without the outer array ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass the filter as an array `["fieldId", 6008093]`, not as an object ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Field value must not be empty | Pass `filter` with the field identifier ||
|| `fieldId` | This request requires data type `int` in field `fieldId` | Pass the field identifier as a number, not as a string ||
|| `#FIELD#` | DTO `EnumDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Do not pass `order`: the method does not apply it ||
|#

#### Field Not Found Error

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Field `#FIELD_ID#` was not found | Forum messages have no custom field with this identifier, or its type is not `enumeration`. Check the field using the [forum.message.field.custom.list](./forum-message-field-custom-list.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `EnumDto` | Pass in `select` only the fields from the [items element description](#items) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./forum-message-field-custom-enum-get.md)
- [{#T}](./forum-message-field-custom-enum-add.md)
- [{#T}](./forum-message-field-custom-enum-update.md)
- [{#T}](./forum-message-field-custom-enum-delete.md)
- [{#T}](./forum-message-field-custom-add.md)
- [{#T}](./forum-message-field-custom-list.md)
- [{#T}](./index.md)
