# Update Custom Field List Value forum.message.field.custom.enum.update

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

The `forum.message.field.custom.enum.update` method updates a value of a forum message custom field of the List type and returns its data.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the list value to update.

The identifier can be retrieved using the [forum.message.field.custom.enum.list](./forum-message-field-custom-enum-list.md) method ||
|| **fields***
[`object`](../../data-types.md) | Object with the value fields to update. Pass only the fields you are changing. [Field description](#fields)

If you pass an empty object `{}`, the method returns the value unchanged ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **value**
[`string`](../../data-types.md) | New value text. An empty string deletes the value from the list ||
|| **sort**
[`integer`](../../data-types.md) | Sort order of the value in the list ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the value. It must be unique within the field ||
|| **isDefault**
[`boolean`](../../data-types.md) | Default value flag.

Possible values:

- `true` — the value is selected by default
- `false` — a regular value

The method does not clear the flag on other values of the field. To keep a single default value, pass `false` for the other values ||
|#

A value cannot be moved to another field: the method ignores `fieldId` in `fields` without an error.

{% note warning "" %}

If you pass `value` as an empty string, the method deletes the value from the list. The response is successful and returns the value with `"value": ""`, but the [forum.message.field.custom.enum.get](./forum-message-field-custom-enum-get.md) and [forum.message.field.custom.enum.list](./forum-message-field-custom-enum-list.md) methods no longer find it. To delete a value, use the [forum.message.field.custom.enum.delete](./forum-message-field-custom-enum-delete.md) method.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/forum.message.field.custom.enum.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4077,"fields":{"value":"Urgent","sort":5}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/forum.message.field.custom.enum.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4077,"fields":{"value":"Urgent","sort":5},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/forum.message.field.custom.enum.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EnumValueUpdateResult = {
      item: {
        id: number
        fieldId: number
        value: string
        isDefault: boolean
        sort: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<EnumValueUpdateResult>({
        method: 'forum.message.field.custom.enum.update',
        params: {
          id: 4077,
          fields: {
            value: 'Urgent',
            sort: 5,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Enum value updated:', result.item.value)
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
      async function updateEnumValue() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'forum.message.field.custom.enum.update',
            params: {
              id: 4077,
              fields: {
                value: 'Urgent',
                sort: 5,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Enum value updated:', result.item.value)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateEnumValue)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.forum.message.field.custom.enum.update(
            id=4077,
            fields={
                "value": "Urgent",
                "sort": 5,
            },
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
                'forum.message.field.custom.enum.update',
                [
                    'id' => 4077,
                    'fields' => [
                        'value' => 'Urgent',
                        'sort' => 5,
                    ],
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
        'forum.message.field.custom.enum.update',
        {
            id: 4077,
            fields: {
                value: 'Urgent',
                sort: 5,
            },
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
        'forum.message.field.custom.enum.update',
        [
            'id' => 4077,
            'fields' => [
                'value' => 'Urgent',
                'sort' => 5,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "forum.message.field.custom.enum.update", b24.Params{
    	"id": 4077,
    	"fields": b24.Params{
    		"value": "Urgent",
    		"sort":  5,
    	},
    })
    if err != nil {
    	return fmt.Errorf("forum.message.field.custom.enum.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID        int    `json:"id"`
    	FieldID   int    `json:"fieldId"`
    	Value     string `json:"value"`
    	IsDefault bool   `json:"isDefault"`
    	Sort      int    `json:"sort"`
    	XMLID     string `json:"xmlId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Value, item.Sort)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 4077,
            "fieldId": 6008093,
            "value": "Urgent",
            "isDefault": false,
            "sort": 5,
            "xmlId": "HIGH"
        }
    },
    "time": {
        "start": 1792670749,
        "finish": 1792670749.13941,
        "duration": 0.13941001892089844,
        "processing": 0,
        "date_start": "2026-10-22T15:05:49+02:00",
        "date_finish": "2026-10-22T15:05:49+02:00",
        "operating_reset_at": 1792671300,
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
[`object`](../../data-types.md) | List value after the update. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

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
[`boolean`](../../data-types.md) | `true` — default value, `false` — regular value ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the value ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 99999999 was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the `id` of the list value ||
|| `-` | Only one of the following values must be provided: id, filter | Pass the `id` of the list value ||
|#

#### Value Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | There is no value with this `id`, or it belongs to a field of another object. Retrieve the identifier using the [forum.message.field.custom.enum.list](./forum-message-field-custom-enum-list.md) method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./forum-message-field-custom-enum-add.md)
- [{#T}](./forum-message-field-custom-enum-get.md)
- [{#T}](./forum-message-field-custom-enum-list.md)
- [{#T}](./forum-message-field-custom-enum-delete.md)
- [{#T}](./forum-message-field-custom-add.md)
- [{#T}](./forum-message-field-custom-list.md)
- [{#T}](./index.md)
