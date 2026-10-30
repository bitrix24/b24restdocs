# Update custom field user.userfield.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `user.userfield.update` method changes the settings and labels of a custom profile field. The `FIELD_NAME` code, the `USER_TYPE_ID` type, and the `MULTIPLE` flag cannot be changed after the field is created.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md)| Custom field identifier.

To obtain custom field identifiers, use the [user.userfield.list](./user-userfield-list.md) method.
 ||
|| **fields**
[`object`](../../data-types.md)| Field settings to change [(detailed description)](#fields). Settings and labels that are not passed keep their current values. If `fields` is not passed, the method changes nothing and returns `true` ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **XML_ID**
[`string`](../../data-types.md)| External code ||
|| **SORT**
[`integer`](../../data-types.md)| Sort order ||
|| **SHOW_FILTER**
[`string`](../../data-types.md)| Whether to show the field in the list filter. Possible values:
- `Y` — yes. The method saves the value `E`, a mask filter, which is returned by [user.userfield.list](./user-userfield-list.md)
- `N` — no ||
|| **SETTINGS**
[`object`](../../data-types.md)| Additional field settings. The set depends on the field type [(detailed description)](#settings).

Pass only the settings you need to change: the method merges them into the saved settings rather than replacing them entirely ||
|| **LIST**
[`object[]`](../../data-types.md)| Value options for an `enumeration` field [(detailed description)](#list) ||
|| **EDIT_FORM_LABEL**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) | Caption in the edit form. You can pass a string or an object with language-specific captions in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be applied to all languages ||
|| **LIST_COLUMN_LABEL**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) | Column header in the list. You can pass a string or an object with language-specific captions in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be applied to all languages ||
|| **LIST_FILTER_LABEL**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) | Filter header in the list. You can pass a string or an object with language-specific captions in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be applied to all languages ||
|| **ERROR_MESSAGE**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) | Error message for invalid input. You can pass a string or an object with language-specific texts in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be applied to all languages ||
|| **HELP_MESSAGE**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) | Tooltip text for the field. You can pass a string or an object with language-specific texts in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be applied to all languages ||
|| **LABEL**
[`string`](../../data-types.md)| Default custom field name. 

The value will be set in fields `LIST_FILTER_LABEL`, `LIST_COLUMN_LABEL`, `EDIT_FORM_LABEL`, `ERROR_MESSAGE`, `HELP_MESSAGE` if no value is provided for them ||
|#

The method ignores, without an error, parameters that cannot be changed: the `FIELD_NAME` code, the `USER_TYPE_ID` type, the `MULTIPLE` flag, as well as `MANDATORY`, `SHOW_IN_LIST`, `EDIT_IN_LIST`, and `IS_SEARCHABLE`.

### Parameter SETTINGS {#settings}

Each type of custom field has its own set of additional configurations.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../data-types.md) | Default value.

    Default `''` ||
    || **ROWS**
    [`integer`](../../data-types.md) | Number of lines in the input field. Must be greater than 0.

    Default `1` ||
    |#

- integer

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../data-types.md) | Default value ||
    |#

- double

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`double`](../../data-types.md) | Default value ||
    || **PRECISION**
    [`integer`](../../data-types.md) | Number precision. Must be greater than or equal to 0.

    Default `2` ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../data-types.md) | Default value, where `1` is yes, `0` is no.

    Possible values:
    - `>= 1` -> 1
    - `<= 0` -> 0

    Default `0` ||
    || **DISPLAY**
    [`string`](../../data-types.md) | Appearance. Possible values:
    - `CHECKBOX` — checkbox
    - `RADIO` — radio buttons
    - `DROPDOWN` — dropdown list

    Default `CHECKBOX` ||
    |#

- datetime

    For a `date` field, the method does not change `SETTINGS`: the default value can be set only on creation with the [user.userfield.add](./user-userfield-add.md) method.

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`object`](../../data-types.md)  | Default value.

    Object format:

    ```json
    {
        "VALUE": "2026-01-15",
        "TYPE": "FIXED"
    }
    ```

    where:
    - `VALUE` — default value of type `datetime` or `date`
    - `TYPE` — type of default value:
      - `NONE` — do not set a default value
      - `NOW` — use current time/date
      - `FIXED` — use time/date from `VALUE`. Currently, the `FIXED` value is saved incorrectly: the method returns the date `1970-01-01`. Use `NOW` or `NONE`

    Default value:

    ```json
    {
        "VALUE": "",
        "TYPE": "NONE"
    }
    ```
    ||
    |#

- enumeration

    #|
    || **Name**
    `type` | **Description** ||
    || **DISPLAY**
    [`string`](../../data-types.md) | Appearance. Possible values:
    - `LIST` — list
    - `UI` — editable list
    - `CHECKBOX` — checkboxes
    - `DIALOG` — entity selection dialog

    Default `LIST` ||
    || **LIST_HEIGHT**
    [`integer`](../../data-types.md) | List height. Must be greater than 0.

    Available only when `DISPLAY = LIST` or `DISPLAY = UI`.

    Default `1` ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **IBLOCK_TYPE_ID**
    [`string`](../../data-types.md) | Iblock type identifier.

    Default `''` ||
    || **IBLOCK_ID**
    [`string`](../../data-types.md) | Information block identifier.

    Default `0` ||
    || **DEFAULT_VALUE**
    [`string`](../../data-types.md) | Default value.

    Default `''` ||
    || **DISPLAY**
    [`string`](../../data-types.md) | Appearance. Possible values:
    - `DIALOG` — dialog
    - `UI` — editable list
    - `LIST` — list
    - `CHECKBOX` — checkboxes

    Default `LIST` ||
    || **LIST_HEIGHT**
    [`integer`](../../data-types.md) | List height. Must be greater than 0.

    Default `1` ||
    || **ACTIVE_FILTER**
    [`string`](../../data-types.md) | Whether to show items with the activity flag enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
    || **ENTITY_TYPE**
    [`string`](../../data-types.md) | Directory type identifier.

    Use [`crm.status.entity.types`](../../crm/status/crm-status-entity-types.md) to find possible values.

    Default `''` ||
    |#

- crm

    The method changes only the bindings that are passed; the rest keep their current values.

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`string`](../../data-types.md) | Whether binding to [Leads](../../crm/leads/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no ||
    || **CONTACT**
    [`string`](../../data-types.md) | Whether binding to [Contacts](../../crm/contacts/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no ||
    || **COMPANY**
    [`string`](../../data-types.md) | Whether binding to [Companies](../../crm/companies/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no ||
    || **DEAL**
    [`string`](../../data-types.md) | Whether binding to [Deals](../../crm/deals/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no ||
    |#

{% endlist %}

For `file`, `url`, `address`, `money`, and `employee` fields, the method does not change `SETTINGS`: the passed settings are ignored, and the saved field settings stay the same. For an `employee` field, the `SHOW_FILTER` = `Y` value is saved as `I`, an exact match filter.

### Parameter LIST {#list}

Value options for an `enumeration` field. Pass only the options you need to change, add, or delete: the other options are retained.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | ID of an existing option. It is returned by the [user.userfield.list](./user-userfield-list.md) method in the `LIST` array. An option without `ID` is added as a new one ||
|| **VALUE**
[`string`](../../data-types.md) | Option text ||
|| **SORT**
[`integer`](../../data-types.md) | Option order in the list ||
|| **DEF**
[`string`](../../data-types.md) | Default option:
- `Y` — yes
- `N` — no ||
|| **XML_ID**
[`string`](../../data-types.md) | External code of the option. If you do not pass `XML_ID` when changing an option, the method generates a new external code ||
|| **DEL**
[`string`](../../data-types.md) | Delete the option with the specified `ID`:
- `Y` — delete ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": 42,
        "fields": {
            "SORT": 150,
            "LIST_FILTER_LABEL": "New Title",
            "LIST_COLUMN_LABEL": "New List Title"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.userfield.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": 42,
        "fields": {
            "SORT": 150,
            "LIST_FILTER_LABEL": "New Title",
            "LIST_COLUMN_LABEL": "New List Title"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.userfield.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UpdateUserFieldResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<UpdateUserFieldResult>({
        method: 'user.userfield.update',
        params: {
          id: 42,
          fields: {
            SORT: 150,
            LIST_FILTER_LABEL: 'New Title',
            LIST_COLUMN_LABEL: 'New List Title',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User field updated:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function updateUserField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'user.userfield.update',
            params: {
              id: 42,
              fields: {
                SORT: 150,
                LIST_FILTER_LABEL: 'New Title',
                LIST_COLUMN_LABEL: 'New List Title',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('User field updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateUserField)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.userfield.update(
            bitrix_id=42,
            fields={
                "SORT": 150,
                "LIST_FILTER_LABEL": "New Title",
                "LIST_COLUMN_LABEL": "New List Title",
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'user.userfield.update',
                [
                    'id' => 42,
                    'fields' => [
                        'SORT' => 150,
                        'LIST_FILTER_LABEL' => 'New Title',
                        'LIST_COLUMN_LABEL' => 'New List Title',
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'user.userfield.update', 
        {
            id: 42,
            fields: {
                SORT: 150,
                LIST_FILTER_LABEL: 'New Title',
                LIST_COLUMN_LABEL: 'New List Title',
            },
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.userfield.update',
        [
            'id' => 42,
            'fields' => [
                'SORT' => 150,
                'LIST_FILTER_LABEL' => 'New Title',
                'LIST_COLUMN_LABEL' => 'New List Title',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "user.userfield.update", b24.Params{
    	"id": 42,
    	"fields": b24.Params{
    		"SORT":              150,
    		"LIST_FILTER_LABEL": "New Title",
    		"LIST_COLUMN_LABEL": "New List Title",
    	},
    })
    if err != nil {
    	return fmt.Errorf("user.userfield.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1793288729,
        "finish": 1793288730.001057,
        "duration": 1.0010569095611572,
        "processing": 1,
        "date_start": "2026-10-29T18:45:29+01:00",
        "date_finish": "2026-10-29T18:45:30+01:00",
        "operating_reset_at": 1793289300,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the field is updated ||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty string | Access denied. | The user does not have administrator permissions, the field with the passed `id` is not found, or it is a system profile field without the `UF_USR_` prefix. System fields cannot be changed via REST ||
|| `400` | Empty string | ID is not defined or invalid. | The `id` is not passed or the passed value is less than `1` ||
|| `401` | `insufficient_scope` | The request requires higher privileges than provided by the webhook token | The webhook or application does not have the `user.userfield` scope ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-userfield-add.md)
- [{#T}](./user-userfield-list.md)
- [{#T}](./user-userfield-delete.md)