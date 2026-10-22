# Add List Value to Custom Field forum.message.field.custom.enum.add

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

The `forum.message.field.custom.enum.add` method adds a value to a forum message custom field of the List type and returns the value data.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object with the fields of the new list value. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **fieldId***
[`integer`](../../data-types.md) | Identifier of the custom field to add the value to. The field must be of the `enumeration` type.

The identifier can be retrieved using the [forum.message.field.custom.list](./forum-message-field-custom-list.md) method or from the response of the [forum.message.field.custom.add](./forum-message-field-custom-add.md) method ||
|| **value***
[`string`](../../data-types.md) | Value text that the user sees.

Pass a non-empty string. With an empty string, no value is created, but the method responds successfully and returns another value of this field — the one with the highest identifier ||
|| **sort**
[`integer`](../../data-types.md) | Sort order of the value in the list.

Default: `500` ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the value. It must be unique within the field: a duplicate code causes an internal error with HTTP status 500.

If the code is not passed, Bitrix24 generates it automatically — a 32-character string, for example `cd9b47f0dad282fb4332d61d14a31222` ||
|| **isDefault**
[`boolean`](../../data-types.md) | Default value flag.

Possible values:

- `true` — the value is selected by default
- `false` — a regular value

The method does not clear the flag on other values of the field. If a default value already exists, after adding there will be several of them, even for a single-choice field. To clear the flag, pass `false` for the required value using the [forum.message.field.custom.enum.update](./forum-message-field-custom-enum-update.md) method.

Default: `false` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/forum.message.field.custom.enum.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fieldId":6008093,"value":"High","sort":10,"xmlId":"HIGH"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/forum.message.field.custom.enum.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"fieldId":6008093,"value":"High","sort":10,"xmlId":"HIGH"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/forum.message.field.custom.enum.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EnumValueAddResult = {
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
      const response = await $b24.actions.v3.call.make<EnumValueAddResult>({
        method: 'forum.message.field.custom.enum.add',
        params: {
          fields: {
            fieldId: 6008093,
            value: 'High',
            sort: 10,
            xmlId: 'HIGH',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Enum value created:', result.item.id)
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
      async function addEnumValue() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'forum.message.field.custom.enum.add',
            params: {
              fields: {
                fieldId: 6008093,
                value: 'High',
                sort: 10,
                xmlId: 'HIGH',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Enum value created:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addEnumValue)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.forum.message.field.custom.enum.add(
            fields={
                "fieldId": 6008093,
                "value": "High",
                "sort": 10,
                "xmlId": "HIGH",
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
                'forum.message.field.custom.enum.add',
                [
                    'fields' => [
                        'fieldId' => 6008093,
                        'value' => 'High',
                        'sort' => 10,
                        'xmlId' => 'HIGH',
                    ],
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
        'forum.message.field.custom.enum.add',
        {
            fields: {
                fieldId: 6008093,
                value: 'High',
                sort: 10,
                xmlId: 'HIGH',
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
        'forum.message.field.custom.enum.add',
        [
            'fields' => [
                'fieldId' => 6008093,
                'value' => 'High',
                'sort' => 10,
                'xmlId' => 'HIGH',
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
    res, err := client.Core().Call(ctx, "forum.message.field.custom.enum.add", b24.Params{
    	"fields": b24.Params{
    		"fieldId": 6008093,
    		"value":   "High",
    		"sort":    10,
    		"xmlId":   "HIGH",
    	},
    })
    if err != nil {
    	return fmt.Errorf("forum.message.field.custom.enum.add: %w", err)
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
    fmt.Println(item.ID, item.Value, item.XMLID)
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
            "value": "High",
            "isDefault": false,
            "sort": 10,
            "xmlId": "HIGH"
        }
    },
    "time": {
        "start": 1792670734,
        "finish": 1792670734.756363,
        "duration": 0.7563629150390625,
        "processing": 0,
        "date_start": "2026-10-22T15:05:34+02:00",
        "date_finish": "2026-10-22T15:05:34+02:00",
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
[`object`](../../data-types.md) | Created list value. [Field description](#item) ||
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

HTTP status: **400**, **404**, **500**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION",
        "message": "Field \"999999999\" was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Field Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fieldId` | Required field `fieldId` is empty | Add `fieldId` to `fields` ||
|| `value` | Required field `value` is empty | Add `value` to `fields` ||
|#

#### Field Not Found Error

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Field `#FIELD_ID#` was not found | Forum messages have no custom field with this `fieldId`, or its type is not `enumeration`. Check the field identifier and type using the [forum.message.field.custom.list](./forum-message-field-custom-list.md) method ||
|#

#### Internal Error

HTTP status: **500**

```json
{
    "error": "INTERNAL_SERVER_ERROR",
    "error_description": "Internal server error"
}
```

#|
|| **Reason** | **How to Fix** ||
|| The field already has a value with this `xmlId` | Pass a unique `xmlId` or omit it so that the code is generated automatically ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./forum-message-field-custom-enum-update.md)
- [{#T}](./forum-message-field-custom-enum-get.md)
- [{#T}](./forum-message-field-custom-enum-list.md)
- [{#T}](./forum-message-field-custom-enum-delete.md)
- [{#T}](./forum-message-field-custom-add.md)
- [{#T}](./forum-message-field-custom-list.md)
- [{#T}](./index.md)
