# Add Forum Message Custom Field forum.message.field.custom.add

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

The `forum.message.field.custom.add` method adds a custom field for forum messages and returns its data.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object with the fields of the new custom field. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Field code. It must start with `UF_`, may contain only uppercase Latin letters, digits, and `_`, and must be 4 to 50 characters long, for example `UF_EXTERNAL_ID`.

The code must be unique among message custom fields. Codes of existing fields can be retrieved using the [forum.message.field.custom.list](./forum-message-field-custom-list.md) method.

The code cannot be changed after creation ||
|| **userTypeId***
[`string`](../../data-types.md) | Field type, for example:

- `string` — string
- `integer` — integer
- `double` — number
- `boolean` — yes/no
- `date` — date
- `datetime` — date and time
- `enumeration` — list. List values are added using the [forum.message.field.custom.enum.add](./forum-message-field-custom-enum-add.md) method
- `file` — file
- `url` — link

The type cannot be changed after creation ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the field ||
|| **sort**
[`integer`](../../data-types.md) | Sort order.

Default: `100` ||
|| **isMultiple**
[`boolean`](../../data-types.md) | Multiple field flag.

Possible values:

- `true` — the field stores several values
- `false` — the field stores one value

Default: `false` ||
|| **isMandatory**
[`boolean`](../../data-types.md) | Required field flag.

Possible values:

- `true` — the field is required
- `false` — the field is optional

Default: `false` ||
|| **showFilter**
[`string`](../../data-types.md) | How the field is shown in the list filter.

Possible values:

- `N` — do not show
- `I` — exact match
- `E` — mask search
- `S` — substring search

Default: `N` ||
|| **showInList**
[`boolean`](../../data-types.md) | Whether to show the field in the list.

Possible values:

- `true` — show
- `false` — do not show

Default: `true` ||
|| **editInList**
[`boolean`](../../data-types.md) | Whether the field value can be edited.

Possible values:

- `true` — allowed
- `false` — not allowed

Default: `true` ||
|| **isSearchable**
[`boolean`](../../data-types.md) | Whether the field values are included in search.

Possible values:

- `true` — included
- `false` — not included

Default: `false` ||
|| **settings**
[`object`](../../data-types.md) | Field type settings.

Bitrix24 does not apply the passed settings: the field is created with the default type settings. The current settings are returned in the response ||
|| **editFormLabel**
[`object`](../../data-types.md) | Field label in the edit form. Pass an object where the key is the language code and the value is the label, for example `{"de":"Externe ID","en":"External ID"}`. A string value is not saved ||
|| **listColumnLabel**
[`object`](../../data-types.md) | List column label. The format is the same as for `editFormLabel` ||
|| **listFilterLabel**
[`object`](../../data-types.md) | List filter label. The format is the same as for `editFormLabel` ||
|| **errorMessage**
[`object`](../../data-types.md) | Error message shown for an invalid field value. The format is the same as for `editFormLabel` ||
|| **helpMessage**
[`object`](../../data-types.md) | Help text for the field. The format is the same as for `editFormLabel` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/forum.message.field.custom.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"UF_EXTERNAL_ID","userTypeId":"string","xmlId":"EXTERNAL_ID","sort":200,"isMandatory":false,"isSearchable":true,"editFormLabel":{"de":"Externe ID","en":"External ID"},"helpMessage":{"de":"Nachrichtencode im externen System","en":"Message code in the external system"}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/forum.message.field.custom.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"UF_EXTERNAL_ID","userTypeId":"string","xmlId":"EXTERNAL_ID","sort":200,"isMandatory":false,"isSearchable":true,"editFormLabel":{"de":"Externe ID","en":"External ID"},"helpMessage":{"de":"Nachrichtencode im externen System","en":"Message code in the external system"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/forum.message.field.custom.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CustomFieldAddResult = {
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
      const response = await $b24.actions.v3.call.make<CustomFieldAddResult>({
        method: 'forum.message.field.custom.add',
        params: {
          fields: {
            name: 'UF_EXTERNAL_ID',
            userTypeId: 'string',
            xmlId: 'EXTERNAL_ID',
            sort: 200,
            isMandatory: false,
            isSearchable: true,
            editFormLabel: { de: 'Externe ID', en: 'External ID' },
            helpMessage: { de: 'Nachrichtencode im externen System', en: 'Message code in the external system' },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Custom field created:', result.item.id)
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
      async function addCustomField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'forum.message.field.custom.add',
            params: {
              fields: {
                name: 'UF_EXTERNAL_ID',
                userTypeId: 'string',
                xmlId: 'EXTERNAL_ID',
                sort: 200,
                isMandatory: false,
                isSearchable: true,
                editFormLabel: { de: 'Externe ID', en: 'External ID' },
                helpMessage: { de: 'Nachrichtencode im externen System', en: 'Message code in the external system' },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Custom field created:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCustomField)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.forum.message.field.custom.add(
            fields={
                "name": "UF_EXTERNAL_ID",
                "userTypeId": "string",
                "xmlId": "EXTERNAL_ID",
                "sort": 200,
                "isMandatory": False,
                "isSearchable": True,
                "editFormLabel": {"de": "Externe ID", "en": "External ID"},
                "helpMessage": {"de": "Nachrichtencode im externen System", "en": "Message code in the external system"},
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
                'forum.message.field.custom.add',
                [
                    'fields' => [
                        'name' => 'UF_EXTERNAL_ID',
                        'userTypeId' => 'string',
                        'xmlId' => 'EXTERNAL_ID',
                        'sort' => 200,
                        'isMandatory' => false,
                        'isSearchable' => true,
                        'editFormLabel' => ['de' => 'Externe ID', 'en' => 'External ID'],
                        'helpMessage' => ['de' => 'Nachrichtencode im externen System', 'en' => 'Message code in the external system'],
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
        'forum.message.field.custom.add',
        {
            fields: {
                name: 'UF_EXTERNAL_ID',
                userTypeId: 'string',
                xmlId: 'EXTERNAL_ID',
                sort: 200,
                isMandatory: false,
                isSearchable: true,
                editFormLabel: { de: 'Externe ID', en: 'External ID' },
                helpMessage: { de: 'Nachrichtencode im externen System', en: 'Message code in the external system' },
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
        'forum.message.field.custom.add',
        [
            'fields' => [
                'name' => 'UF_EXTERNAL_ID',
                'userTypeId' => 'string',
                'xmlId' => 'EXTERNAL_ID',
                'sort' => 200,
                'isMandatory' => false,
                'isSearchable' => true,
                'editFormLabel' => ['de' => 'Externe ID', 'en' => 'External ID'],
                'helpMessage' => ['de' => 'Nachrichtencode im externen System', 'en' => 'Message code in the external system'],
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
    res, err := client.Core().Call(ctx, "forum.message.field.custom.add", b24.Params{
    	"fields": b24.Params{
    		"name":          "UF_EXTERNAL_ID",
    		"userTypeId":    "string",
    		"xmlId":         "EXTERNAL_ID",
    		"sort":          200,
    		"isMandatory":   false,
    		"isSearchable":  true,
    		"editFormLabel": b24.Params{"de": "Externe ID", "en": "External ID"},
    		"helpMessage":   b24.Params{"de": "Nachrichtencode im externen System", "en": "Message code in the external system"},
    	},
    })
    if err != nil {
    	return fmt.Errorf("forum.message.field.custom.add: %w", err)
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
            "sort": 200,
            "isMultiple": false,
            "isMandatory": false,
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
            "editFormLabel": "External ID",
            "listColumnLabel": null,
            "listFilterLabel": null,
            "errorMessage": null,
            "helpMessage": "Message code in the external system"
        }
    },
    "time": {
        "start": 1792670680,
        "finish": 1792670680.614779,
        "duration": 0.614778995513916,
        "processing": 0,
        "date_start": "2026-10-22T15:04:40+02:00",
        "date_finish": "2026-10-22T15:04:40+02:00",
        "operating_reset_at": 1792670760,
        "operating": 0.23125314712524414
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
[`object`](../../data-types.md) | The created custom field. [Field description](#item) ||
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
[`string`](../../data-types.md) | Field type ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the field. `null` if not set ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **isMultiple**
[`boolean`](../../data-types.md) | `true` — multiple field, `false` — single field ||
|| **isMandatory**
[`boolean`](../../data-types.md) | `true` — required field, `false` — optional field ||
|| **showFilter**
[`string`](../../data-types.md) | How the field is shown in the list filter: `N`, `I`, `E`, or `S`. The values are explained in the [fields parameter description](#fields) ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYALREADYEXISTSEXCEPTION",
        "message": "Record with ID UF_EXTERNAL_ID already exists."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Field Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is empty | Pass `name` in `fields` ||
|| `userTypeId` | Required field `userTypeId` is empty | Pass `userTypeId` in `fields` ||
|| `showFilter` | Field value is not one of the possible values: N, I, E, S | Pass one of the values `N`, `I`, `E`, `S` ||
|#

#### Field Already Exists Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYALREADYEXISTSEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Record with ID `#NAME#` already exists | A field with this code already exists. Specify a different `name` ||
|#

#### Internal Error

HTTP status: **500**

The response comes in the format of the previous API version: `{"error":"INTERNAL_SERVER_ERROR","error_description":"Internal server error"}`.

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Internal server error | Invalid field code or type. Make sure that `name` starts with `UF_`, contains only uppercase Latin letters, digits, and `_`, and is 4 to 50 characters long, and that `userTypeId` is an existing field type ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./forum-message-field-custom-update.md)
- [{#T}](./forum-message-field-custom-get.md)
- [{#T}](./forum-message-field-custom-list.md)
- [{#T}](./forum-message-field-custom-delete.md)
- [{#T}](./forum-message-field-custom-enum-list.md)
- [{#T}](../message/index.md)
- [{#T}](./index.md)
