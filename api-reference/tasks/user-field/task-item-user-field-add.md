# Add Custom Field task.item.userfield.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `task.item.userfield.add` creates a custom field for a task.

When creating a custom field, it is mandatory to use the prefix `UF_` in the field name `FIELD_NAME`. If the prefix is not specified, the system will automatically add it to the beginning of the name.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PARAMS***
[`object`](../../data-types.md) | Set of parameters for the created field [(detailed description)](#params) ||
|#

### Parameter PARAMS {#params}

#|
|| **Name**
`type` | **Description** ||
|| **USER_TYPE_ID***
[`string`](../../data-types.md) | Data type of the custom field.

Supported values:
- `string` — string
- `double` — number
- `datetime` — date and time
- `boolean` — yes/no ||
|| **FIELD_NAME***
[`string`](../../data-types.md) | Code of the custom field ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier ||
|| **EDIT_FORM_LABEL**
[`object`](../../data-types.md) | Label in the edit form [(detailed description)](#edit_form_label) ||
|| **LABEL**
[`string`](../../data-types.md) | Name of the custom field ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting
||
|| **MULTIPLE**
[`string`](../../data-types.md) | Multiple value. Possible values:
- `Y` — multiple
- `N` — single

Applies to types `string`, `double`, `datetime`. For type `boolean`, `N` is always used ||
|| **MANDATORY**
[`string`](../../data-types.md) | Mandatory value. Possible values:
- `Y` — mandatory
- `N` — optional
||
|| **SETTINGS**
[`object`](../../data-types.md) | Additional settings for the field type [(detailed description)](#settings) ||
|#

### Parameter EDIT_FORM_LABEL {#edit_form_label}

#|
|| **Name**
`type` | **Description** ||
|| **de**
[`string`](../../data-types.md) | Label in German ||
|| **en**
[`string`](../../data-types.md) | Label in English ||
|#

### Parameter SETTINGS {#settings}

The fields of the `SETTINGS` object depend on the `USER_TYPE_ID` type.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../data-types.md) | Default value ||
    || **ROWS**
    [`integer`](../../data-types.md) | Number of rows in the input field ||
    |#

- double

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`double`](../../data-types.md) | Default value ||
    |#

- datetime

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
     | Default value. Described as an object with two parameters:
    1. `TYPE` [`string`](../../data-types.md) — mode for filling the default value
        - `NONE` — default value is not set
        - `FIXED` — uses the value from `VALUE`
        - `NOW` — uses the current time
    2. `VALUE` [`datetime`](../../data-types.md) — value for type `FIXED`
    
    ```js
    DEFAULT_VALUE: {
        TYPE: 'NOW',
        VALUE: ''
    },
    ```
    
    ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../data-types.md) | Default value:
    - `0` — no
    - `1` — yes ||
    || **DISPLAY**
    [`string`](../../data-types.md) | Display option for the value:
    - `CHECKBOX` — checkbox
    - `RADIO` — radio buttons
    - `DROPDOWN` — dropdown list ||
    |#

{% endlist %}

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "PARAMS": {
        "USER_TYPE_ID": "string",
        "FIELD_NAME": "UF_TASK_CLIENT_REQUEST",
        "XML_ID": "UF_TASK_CLIENT_REQUEST",
        "EDIT_FORM_LABEL": {
          "de": "Kundenanfrage",
          "en": "Client request"
        },
        "LABEL": "Client request",
        "SORT": 220,
        "MULTIPLE": "N",
        "MANDATORY": "Y",
        "SETTINGS": {
          "DEFAULT_VALUE": "Clarify the goal and expected result",
          "ROWS": 10
        }
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.userfield.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "PARAMS": {
        "USER_TYPE_ID": "string",
        "FIELD_NAME": "UF_TASK_CLIENT_REQUEST",
        "XML_ID": "UF_TASK_CLIENT_REQUEST",
        "EDIT_FORM_LABEL": {
          "de": "Kundenanfrage",
          "en": "Client request"
        },
        "LABEL": "Client request",
        "SORT": 220,
        "MULTIPLE": "N",
        "MANDATORY": "Y",
        "SETTINGS": {
          "DEFAULT_VALUE": "Clarify the goal and expected result",
          "ROWS": 10
        }
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/task.item.userfield.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'task.item.userfield.add',
        params: {
          PARAMS: {
            USER_TYPE_ID: 'string',
            FIELD_NAME: 'UF_TASK_CLIENT_REQUEST',
            XML_ID: 'UF_TASK_CLIENT_REQUEST',
            EDIT_FORM_LABEL: {
              de: 'Kundenanfrage',
              en: 'Client request',
            },
            LABEL: 'Client request',
            SORT: 220,
            MULTIPLE: 'N',
            MANDATORY: 'Y',
            SETTINGS: {
              DEFAULT_VALUE: 'Clarify the goal and expected result',
              ROWS: 10,
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created user field ID:', result)
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
      async function addTaskUserField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.item.userfield.add',
            params: {
              PARAMS: {
                USER_TYPE_ID: 'string',
                FIELD_NAME: 'UF_TASK_CLIENT_REQUEST',
                XML_ID: 'UF_TASK_CLIENT_REQUEST',
                EDIT_FORM_LABEL: {
                  de: 'Kundenanfrage',
                  en: 'Client request',
                },
                LABEL: 'Client request',
                SORT: 220,
                MULTIPLE: 'N',
                MANDATORY: 'Y',
                SETTINGS: {
                  DEFAULT_VALUE: 'Clarify the goal and expected result',
                  ROWS: 10,
                },
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
          console.info('Created user field ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTaskUserField)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.item.userfield.add',
                [
                    'PARAMS' => [
                        'USER_TYPE_ID' => 'string',
                        'FIELD_NAME' => 'UF_TASK_CLIENT_REQUEST',
                        'XML_ID' => 'UF_TASK_CLIENT_REQUEST',
                        'EDIT_FORM_LABEL' => [
                            'de' => 'Kundenanfrage',
                            'en' => 'Client request'
                        ],
                        'LABEL' => 'Client request',
                        'SORT' => 220,
                        'MULTIPLE' => 'N',
                        'MANDATORY' => 'Y',
                        'SETTINGS' => [
                            'DEFAULT_VALUE' => 'Clarify the goal and expected result',
                            'ROWS' => 10
                        ]
                    ]
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

    ```js
    BX24.callMethod(
        'task.item.userfield.add',
        {
            PARAMS: {
                USER_TYPE_ID: 'string',
                FIELD_NAME: 'UF_TASK_CLIENT_REQUEST',
                XML_ID: 'UF_TASK_CLIENT_REQUEST',
                LABEL: 'Client request',
                EDIT_FORM_LABEL: {
                    de: 'Kundenanfrage',
                    en: 'Client request'
                },
                SORT: 220,
                MULTIPLE: 'N',
                MANDATORY: 'Y',
                SETTINGS: {
                    DEFAULT_VALUE: 'Clarify the goal and expected result',
                    ROWS: 10
                }
            }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.item.userfield.add',
        [
            'PARAMS' => [
                'USER_TYPE_ID' => 'string',
                'FIELD_NAME' => 'UF_TASK_CLIENT_REQUEST',
                'XML_ID' => 'UF_TASK_CLIENT_REQUEST',
                'EDIT_FORM_LABEL' => [
                    'de' => 'Kundenanfrage',
                    'en' => 'Client request'
                ],
                'LABEL' => 'Client request',
                'SORT' => 220,
                'MULTIPLE' => 'N',
                'MANDATORY' => 'Y',
                'SETTINGS' => [
                    'DEFAULT_VALUE' => 'Clarify the goal and expected result',
                    'ROWS' => 10
                ]
            ]
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 1325,
    "time": {
        "start": 1772711476,
        "finish": 1772711476.284127,
        "duration": 0.28412699699401855,
        "processing": 0,
        "date_start": "2026-03-05T14:51:16+01:00",
        "date_finish": "2026-03-05T14:51:16+01:00",
        "operating_reset_at": 1772712076,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created custom field ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "The 'USER_TYPE_ID' field is not found."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | The 'USER_TYPE_ID' field is not found | The parameter `USER_TYPE_ID` is not provided or is empty ||
|| `400` | `ERROR_CORE` | Invalid user type specified. | An invalid or non-existent user field type is specified in the `USER_TYPE_ID` parameter ||
|| `400` | `ERROR_CORE` | The 'FIELD_NAME' field is not found | The parameter `FIELD_NAME` is not provided or is empty ||
|| `400` | `ERROR_CORE` | The field UF_TASK_CLIENT_REQUEST for the object TASKS_TASK already exists. | The `FIELD_NAME` parameter specifies a custom field name that already exists in the system ||
|| `400` | `ERROR_CORE` | Access denied | Insufficient permissions to create a custom field ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-item-user-field-update.md)
- [{#T}](./task-item-user-field-get.md)
- [{#T}](./task-item-user-field-get-list.md)
- [{#T}](./task-item-user-field-delete.md)
- [{#T}](./task-item-user-field-get-types.md)
- [{#T}](./task-item-user-field-get-fields.md)