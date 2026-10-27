# Get List of Forum Message Custom Fields forum.message.field.custom.list

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

The `forum.message.field.custom.list` method returns all custom fields for forum messages, including Bitrix24 system fields such as `UF_FORUM_MESSAGE_DOC`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
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

{% note info "" %}

The method always returns all fields in a single list: `filter` and `pagination` are not applied, and `order` causes a validation error. Find the field you need in the response by `name`, or retrieve it by `id` using the [forum.message.field.custom.get](./forum-message-field-custom-get.md) method.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/forum.message.field.custom.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","userTypeId","isMultiple","editFormLabel"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/forum.message.field.custom.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","userTypeId","isMultiple","editFormLabel"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/forum.message.field.custom.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CustomFieldListResult = {
      items: Array<{
        id: number
        name: string
        userTypeId: string
        isMultiple: boolean
        editFormLabel: string | null
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<CustomFieldListResult>({
        method: 'forum.message.field.custom.list',
        params: {
          select: ['id', 'name', 'userTypeId', 'isMultiple', 'editFormLabel'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(field => console.info(field.id, field.name, field.userTypeId))
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
      async function listCustomFields() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'forum.message.field.custom.list',
            params: {
              select: ['id', 'name', 'userTypeId', 'isMultiple', 'editFormLabel'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(field => console.info(field.id, field.name, field.userTypeId))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listCustomFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.forum.message.field.custom.list(
            select=["id", "name", "userTypeId", "isMultiple", "editFormLabel"],
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
                'forum.message.field.custom.list',
                [
                    'select' => ['id', 'name', 'userTypeId', 'isMultiple', 'editFormLabel'],
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
        'forum.message.field.custom.list',
        {
            select: ['id', 'name', 'userTypeId', 'isMultiple', 'editFormLabel'],
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
        'forum.message.field.custom.list',
        [
            'select' => ['id', 'name', 'userTypeId', 'isMultiple', 'editFormLabel'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "forum.message.field.custom.list", b24.Params{
    	"select": []string{"id", "name", "userTypeId", "isMultiple", "editFormLabel"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("forum.message.field.custom.list: %w", err)
    }

    // The method wraps the list in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("no items key in the response")
    }

    var items []struct {
    	ID            int     `json:"id"`
    	Name          string  `json:"name"`
    	UserTypeID    string  `json:"userTypeId"`
    	IsMultiple    bool    `json:"isMultiple"`
    	EditFormLabel *string `json:"editFormLabel"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, field := range items {
    	fmt.Println(field.ID, field.Name, field.UserTypeID)
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
                "id": 31,
                "name": "UF_FORUM_MESSAGE_DOC",
                "userTypeId": "disk_file",
                "isMultiple": true,
                "editFormLabel": null
            },
            {
                "id": 33,
                "name": "UF_FORUM_MESSAGE_VER",
                "userTypeId": "disk_version",
                "isMultiple": false,
                "editFormLabel": null
            },
            {
                "id": 35,
                "name": "UF_FORUM_MES_URL_PRV",
                "userTypeId": "url_preview",
                "isMultiple": false,
                "editFormLabel": null
            },
            {
                "id": 433,
                "name": "UF_TASK_COMMENT_TYPE",
                "userTypeId": "integer",
                "isMultiple": false,
                "editFormLabel": null
            },
            {
                "id": 6008091,
                "name": "UF_EXTERNAL_ID",
                "userTypeId": "string",
                "isMultiple": false,
                "editFormLabel": "External system code"
            }
        ]
    },
    "time": {
        "start": 1792670697,
        "finish": 1792670697.444028,
        "duration": 0.4440279006958008,
        "processing": 0,
        "date_start": "2026-10-22T15:04:57+02:00",
        "date_finish": "2026-10-22T15:04:57+02:00",
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
|| **result.items**
[`array`](../../data-types.md) | An array of forum message custom fields. The set of fields in an element depends on `select`. The fields are described in the [forum.message.field.custom.get](./forum-message-field-custom-get.md#item) method ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

In addition to the fields created through the API, the list contains system fields that Bitrix24 creates itself:

- `UF_FORUM_MESSAGE_DOC` — a field of the `disk_file` type for Drive files in the message
- `UF_FORUM_MESSAGE_VER` — a field of the `disk_version` type for Drive file versions
- `UF_FORUM_MES_URL_PRV` — a field of the `url_preview` type for link previews
- `UF_TASK_COMMENT_TYPE` — a system field of the Tasks module

Do not change or delete system fields: comments in Bitrix24 rely on them.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "DTO \"CustomDto\" in field \"id\" requires attribute \"Sortable\" to perform this request.",
                "field": "id"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | DTO `CustomDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | The method does not support sorting. Remove `order` from the request ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `CustomDto` | Pass in `select` only the fields listed in the parameter description ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./forum-message-field-custom-get.md)
- [{#T}](./forum-message-field-custom-add.md)
- [{#T}](./forum-message-field-custom-update.md)
- [{#T}](./forum-message-field-custom-delete.md)
- [{#T}](./forum-message-field-custom-enum-list.md)
- [{#T}](../message/index.md)
- [{#T}](./index.md)
