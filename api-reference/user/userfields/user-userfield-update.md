# Update custom field user.userfield.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `user.userfield.update` method updates a custom field.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md)| Custom field identifier.

To obtain custom field identifiers, use the [user.userfield.list](./user-userfield-list.md) method.
 ||
|| **fields**
[`object`](../../data-types.md)| Field values for updating the custom field ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **XML_ID**
[`string`](../../data-types.md)| External code ||
|| **SORT**
[`integer`](../../data-types.md)| Sort order ||
|| **MANDATORY**
[`boolean`](../../data-types.md)| Whether the custom field is mandatory. Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_FILTER**
[`boolean`](../../data-types.md)| Whether to show the field in the list filter. Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_IN_LIST**
[`boolean`](../../data-types.md)| Whether to show the field in the list. Possible values:
- `Y` — yes
- `N` — no ||
- || **EDIT_IN_LIST**
[`boolean`](../../data-types.md)| Whether to allow editing the field in the list. Possible values:
- `Y` — yes
- `N` — no ||
- || **IS_SEARCHABLE**
[`boolean`](../../data-types.md)| Whether the field is included in the search. Possible values:
- `Y` — yes
- `N` — no ||
|| **SETTINGS**
[`object`](../../data-types.md)| An object in `{"field_1": "value_1", ... "field_N": "value_N"}` format to pass additional custom field settings. Settings are described [below](#settings) ||
|| **EDIT_FORM_LABEL**
[`string`](../../data-types.md)| Caption in the edit form. You can pass a string or an object with language-specific captions in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be applied to all languages ||
|| **LIST_COLUMN_LABEL**
[`string`](../../data-types.md)| Column header in the list. You can pass a string or an object with language-specific captions in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be applied to all languages ||
|| **LIST_FILTER_LABEL**
[`string`](../../data-types.md)| Filter header in the list. You can pass a string or an object with language-specific captions in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be applied to all languages ||
|| **ERROR_MESSAGE**
[`string`](../../data-types.md)| Error message for invalid input. You can pass a string or an object with language-specific texts in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be applied to all languages ||
|| **HELP_MESSAGE**
[`string`](../../data-types.md)| Tooltip text for the field. You can pass a string or an object with language-specific texts in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be applied to all languages ||
|| **LABEL**
[`string`](../../data-types.md)| Default custom field name. 

The value will be set in fields `LIST_FILTER_LABEL`, `LIST_COLUMN_LABEL`, `EDIT_FORM_LABEL`, `ERROR_MESSAGE`, `HELP_MESSAGE` if no value is provided for them ||
|#

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

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`object`](../../data-types.md)  | Default value.

    Object format:

    ```
    {
        VALUE: datetime|date,
        TYPE: 'NONE'|'NOW'|'FIXED',
    }
    ```

    where:
    - `VALUE` — default value of type `datetime` or `date`
    - `TYPE` — type of default value:
      - `NONE` — do not set a default value
      - `NOW` — use current time/date
      - `FIXED` — use time/date from `VALUE`

    Default value:

    ```
    {
        VALUE: '',
        TYPE: 'NONE',
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
    || **LIST_HEIGHT** | List height. Must be greater than 0.

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
    [`boolean`](../../data-types.md) | Whether to show items with the activity flag enabled. Possible values:
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

    If none of the following options are passed, linking to leads (`LEAD = Y`) will be enabled by default.

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`boolean`](../../data-types.md) | Whether binding to [Leads](../../crm/leads/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **CONTACT**
    [`boolean`](../../data-types.md) | Whether binding to [Contacts](../../crm/contacts/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **COMPANY**
    [`boolean`](../../data-types.md) | Whether binding to [Companies](../../crm/companies/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **DEAL**
    [`boolean`](../../data-types.md) | Whether binding to [Deals](../../crm/deals/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    |#

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    bitrix_id = 176
    fields = {
        "XML_ID": "UF_USR_SKILLS_PROFILE_V2",
        "SORT": 200,
        "MANDATORY": "N",
        "SHOW_FILTER": "Y",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "Y",
        "SETTINGS": {
            "DEFAULT_VALUE": "Senior Python integration engineer",
            "ROWS": 4,
        },
        "EDIT_FORM_LABEL": {
            "en": "Skills profile",
        },
        "LIST_COLUMN_LABEL": {
            "en": "Skills profile",
        },
        "LIST_FILTER_LABEL": {
            "en": "Skills profile",
        },
        "ERROR_MESSAGE": {
            "en": "Skills profile is invalid",
        },
        "HELP_MESSAGE": {
            "en": "Update the short integration skills summary.",
        },
        "LABEL": "Skills profile",
    }

    try:
        bitrix_response = client.user.userfield.update(
            bitrix_id=bitrix_id,
            fields=fields,
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
        print("Bitrix SDK error", error.message, sep="\n")
    except Exception as error:
        print("Unexpected error", error, sep="\n")
    ```
{% endlist %}


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

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start":1747311864.008399,
        "finish":1747311865.063292,
        "duration":1.0548930168151855,
        "processing":0.17107510566711426,
        "date_start":"2025-05-15T14:24:24+02:00",
        "date_finish":"2025-05-15T14:24:25+02:00",
        "operating":0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Contains `true` in case of a successful custom field update||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"",
    "error_description":"Access denied."
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | A field with this `id` does not exist or access is denied ||
|| Empty string | ID is not defined or invalid | Not specified or an invalid `id` was entered ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-userfield-add.md)
- [{#T}](./user-userfield-list.md)
- [{#T}](./user-userfield-delete.md)