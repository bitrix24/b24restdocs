# Update Forum Message Custom Field forum.message.field.custom.update

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

The `forum.message.field.custom.update` method updates a custom field for forum messages and returns its data.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Custom field identifier.

The identifier can be retrieved using the [forum.message.field.custom.list](./forum-message-field-custom-list.md) method ||
|| **fields***
[`object`](../../data-types.md) | An object with the custom field properties to change. Pass only the properties you change. [Field description](#fields)

If you pass an empty object `{}`, the method returns the field unchanged ||
|#

{% note warning "" %}

If you pass at least one label — `editFormLabel`, `listColumnLabel`, `listFilterLabel`, `errorMessage`, or `helpMessage` — all labels that are not in the request are cleared. To change one label, pass it together with the current values of the others.

{% endnote %}

### Parameter fields {#fields}

The `name` code and the `userTypeId` type cannot be changed: if you pass them in `fields`, the method ignores them.

#|
|| **Name**
`type` | **Description** ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the field ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **isMultiple**
[`boolean`](../../data-types.md) | Multiple field flag.

Possible values:

- `true` — the field stores several values
- `false` — the field stores one value ||
|| **isMandatory**
[`boolean`](../../data-types.md) | Required field flag.

Possible values:

- `true` — the field is required
- `false` — the field is optional ||
|| **showFilter**
[`string`](../../data-types.md) | How the field is shown in the list filter.

Possible values:

- `N` — do not show
- `I` — exact match
- `E` — mask search
- `S` — substring search ||
|| **showInList**
[`boolean`](../../data-types.md) | Whether to show the field in the list.

Possible values:

- `true` — show
- `false` — do not show ||
|| **editInList**
[`boolean`](../../data-types.md) | Whether the field value can be edited.

Possible values:

- `true` — allowed
- `false` — not allowed ||
|| **isSearchable**
[`boolean`](../../data-types.md) | Whether the field values are included in search.

Possible values:

- `true` — included
- `false` — not included ||
|| **settings**
[`object`](../../data-types.md) | Field type settings.

Bitrix24 does not apply the passed settings: they remain unchanged ||
|| **editFormLabel**
[`object`](../../data-types.md) | Field label in the edit form. Pass an object where the key is the language code and the value is the label, for example `{"de":"Code im externen System","en":"External system code"}`. A string value is not saved ||
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

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/forum.message.field.custom.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6008091,"fields":{"sort":150,"isMandatory":true,"editFormLabel":{"de":"Code im externen System","en":"External system code"}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/forum.message.field.custom.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6008091,"fields":{"sort":150,"isMandatory":true,"editFormLabel":{"de":"Code im externen System","en":"External system code"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/forum.message.field.custom.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CustomFieldUpdateResult = {
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
      const response = await $b24.actions.v3.call.make<CustomFieldUpdateResult>({
        method: 'forum.message.field.custom.update',
        params: {
          id: 6008091,
          fields: {
            sort: 150,
            isMandatory: true,
            editFormLabel: { de: 'Code im externen System', en: 'External system code' },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Custom field updated:', result.item.id)
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
      async function updateCustomField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'forum.message.field.custom.update',
            params: {
              id: 6008091,
              fields: {
                sort: 150,
                isMandatory: true,
                editFormLabel: { de: 'Code im externen System', en: 'External system code' },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Custom field updated:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCustomField)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.forum.message.field.custom.update(
            id=6008091,
            fields={
                "sort": 150,
                "isMandatory": True,
                "editFormLabel": {"de": "Code im externen System", "en": "External system code"},
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
                'forum.message.field.custom.update',
                [
                    'id' => 6008091,
                    'fields' => [
                        'sort' => 150,
                        'isMandatory' => true,
                        'editFormLabel' => ['de' => 'Code im externen System', 'en' => 'External system code'],
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
        'forum.message.field.custom.update',
        {
            id: 6008091,
            fields: {
                sort: 150,
                isMandatory: true,
                editFormLabel: { de: 'Code im externen System', en: 'External system code' },
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
        'forum.message.field.custom.update',
        [
            'id' => 6008091,
            'fields' => [
                'sort' => 150,
                'isMandatory' => true,
                'editFormLabel' => ['de' => 'Code im externen System', 'en' => 'External system code'],
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
    res, err := client.Core().Call(ctx, "forum.message.field.custom.update", b24.Params{
    	"id": 6008091,
    	"fields": b24.Params{
    		"sort":          150,
    		"isMandatory":   true,
    		"editFormLabel": b24.Params{"de": "Code im externen System", "en": "External system code"},
    	},
    })
    if err != nil {
    	return fmt.Errorf("forum.message.field.custom.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID            int     `json:"id"`
    	Sort          int     `json:"sort"`
    	IsMandatory   bool    `json:"isMandatory"`
    	EditFormLabel *string `json:"editFormLabel"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Sort, item.IsMandatory)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The request in the example passes only the `editFormLabel` label, so the `helpMessage` help text set when the field was created has been cleared.

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
        "start": 1792670693,
        "finish": 1792670693.237927,
        "duration": 0.23792695999145508,
        "processing": 0,
        "date_start": "2026-10-22T15:04:53+02:00",
        "date_finish": "2026-10-22T15:04:53+02:00",
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
[`object`](../../data-types.md) | The custom field after the change. The fields are described in the [forum.message.field.custom.get](./forum-message-field-custom-get.md#item) method ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the custom field `id` ||
|| `-` | Only one of the following values must be provided: id, filter | Pass the custom field `id` ||
|#

#### Field Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `showFilter` | Field value is not one of the possible values: N, I, E, S | Pass one of the values `N`, `I`, `E`, `S` ||
|#

#### Field Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no forum message custom field with this `id`. Check the identifier using the [forum.message.field.custom.list](./forum-message-field-custom-list.md) method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./forum-message-field-custom-add.md)
- [{#T}](./forum-message-field-custom-get.md)
- [{#T}](./forum-message-field-custom-list.md)
- [{#T}](./forum-message-field-custom-delete.md)
- [{#T}](../message/index.md)
- [{#T}](./index.md)
