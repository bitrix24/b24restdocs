# Get Forum Message Custom Field forum.message.field.custom.get

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

The `forum.message.field.custom.get` method returns a custom field for forum messages by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Custom field identifier.

The identifier can be retrieved using the [forum.message.field.custom.list](./forum-message-field-custom-list.md) method ||
|| **select**
[`array`](../../data-types.md) | An array of fields to return in the response.

Available fields:

- `id` — field identifier
- `entityId` — object identifier
- `name` — field code
- `userTypeId` — field type
- `xmlId` — external code
- `sort` — sort order
- `isMultiple` — multiple field flag
- `isMandatory` — required field flag
- `showFilter` — display in the list filter
- `showInList` — display in the list
- `editInList` — editability
- `isSearchable` — inclusion in search
- `settings` — field type settings
- `editFormLabel` — edit form label
- `listColumnLabel` — list column label
- `listFilterLabel` — list filter label
- `errorMessage` — error message
- `helpMessage` — help text

Default: all fields ||
|#

Labels are returned in the response language. The language can be set with the `X-Bitrix-Rest-Response-Language` header, for example `X-Bitrix-Rest-Response-Language: en`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/forum.message.field.custom.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6008091}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/forum.message.field.custom.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6008091,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/forum.message.field.custom.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CustomFieldGetResult = {
      item: {
        id: number
        entityId: string
        name: string
        userTypeId: string
        xmlId: string | null
        sort: number
        isMultiple: boolean
        isMandatory: boolean
        showFilter: string
        showInList: boolean
        editInList: boolean
        isSearchable: boolean
        settings: Record<string, unknown>
        editFormLabel: string | null
        listColumnLabel: string | null
        listFilterLabel: string | null
        errorMessage: string | null
        helpMessage: string | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<CustomFieldGetResult>({
        method: 'forum.message.field.custom.get',
        params: {
          id: 6008091,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.name, result.item.userTypeId)
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
      async function getCustomField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'forum.message.field.custom.get',
            params: {
              id: 6008091,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.name, result.item.userTypeId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCustomField)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.forum.message.field.custom.get(
            id=6008091,
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
                'forum.message.field.custom.get',
                [
                    'id' => 6008091,
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
        'forum.message.field.custom.get',
        {
            id: 6008091,
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
        'forum.message.field.custom.get',
        [
            'id' => 6008091,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "forum.message.field.custom.get", b24.Params{
    	"id": 6008091,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("forum.message.field.custom.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID            int     `json:"id"`
    	Name          string  `json:"name"`
    	UserTypeID    string  `json:"userTypeId"`
    	IsMandatory   bool    `json:"isMandatory"`
    	EditFormLabel *string `json:"editFormLabel"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name, item.UserTypeID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 6008091,
            "entityId": "FORUM_MESSAGE",
            "name": "UF_EXTERNAL_ID",
            "userTypeId": "string",
            "xmlId": "EXTERNAL_ID",
            "sort": 150,
            "isMultiple": false,
            "isMandatory": true,
            "showFilter": "N",
            "showInList": true,
            "editInList": true,
            "isSearchable": true,
            "settings": {
                "size": 20,
                "rows": 1,
                "regexp": "",
                "minLength": 0,
                "maxLength": 0,
                "defaultValue": ""
            },
            "editFormLabel": "External system code",
            "listColumnLabel": null,
            "listFilterLabel": null,
            "errorMessage": null,
            "helpMessage": null
        }
    },
    "time": {
        "start": 1792670694,
        "finish": 1792670694.618115,
        "duration": 0.6181149482727051,
        "processing": 0,
        "date_start": "2026-10-22T15:04:54+02:00",
        "date_finish": "2026-10-22T15:04:54+02:00",
        "operating_reset_at": 1792671240,
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
[`object`](../../data-types.md) | The custom field. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Custom field identifier ||
|| **entityId**
[`string`](../../data-types.md) | Identifier of the object the field belongs to. For forum messages, it is always `FORUM_MESSAGE` ||
|| **name**
[`string`](../../data-types.md) | Field code ||
|| **userTypeId**
[`string`](../../data-types.md) | Field type, for example `string`, `integer`, `enumeration`, `disk_file` ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the field. `null` if not set ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **isMultiple**
[`boolean`](../../data-types.md) | `true` — multiple field, `false` — single field ||
|| **isMandatory**
[`boolean`](../../data-types.md) | `true` — required field, `false` — optional field ||
|| **showFilter**
[`string`](../../data-types.md) | How the field is shown in the list filter:

- `N` — do not show
- `I` — exact match
- `E` — mask search
- `S` — substring search ||
|| **showInList**
[`boolean`](../../data-types.md) | `true` — the field is shown in the list ||
|| **editInList**
[`boolean`](../../data-types.md) | `true` — the field value can be edited ||
|| **isSearchable**
[`boolean`](../../data-types.md) | `true` — the field values are included in search ||
|| **settings**
[`object`](../../data-types.md) | Field type settings. Keys are returned in camelCase, flags as `true` and `false`. The set of keys depends on the type: for `string` they are `size`, `rows`, `regexp`, `minLength`, `maxLength`, `defaultValue`, for `enumeration` — `display`, `listHeight`, `captionNoValue`, `showNoValue` ||
|| **editFormLabel**
[`string`](../../data-types.md) | Edit form label in the response language. `null` if the label is not set ||
|| **listColumnLabel**
[`string`](../../data-types.md) | List column label in the response language. `null` if not set ||
|| **listFilterLabel**
[`string`](../../data-types.md) | List filter label in the response language. `null` if not set ||
|| **errorMessage**
[`string`](../../data-types.md) | Error message in the response language. `null` if not set ||
|| **helpMessage**
[`string`](../../data-types.md) | Help text in the response language. `null` if not set ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 999999999 was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the custom field `id` ||
|#

#### Field Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no forum message custom field with this `id`. Check the identifier using the [forum.message.field.custom.list](./forum-message-field-custom-list.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `CustomDto` | Pass in `select` only the fields from the [item object description](#item) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./forum-message-field-custom-list.md)
- [{#T}](./forum-message-field-custom-update.md)
- [{#T}](./forum-message-field-custom-add.md)
- [{#T}](./forum-message-field-custom-delete.md)
- [{#T}](./forum-message-field-custom-enum-list.md)
- [{#T}](../message/index.md)
- [{#T}](./index.md)
