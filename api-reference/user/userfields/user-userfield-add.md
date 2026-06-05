# Add custom field user.userfield.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `user.userfield.add` method adds a custom field.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md)| Field values for adding a new custom field ||
|#

### Parameter fields

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELD_NAME***
[`string`](../../data-types.md)| Field name (code). Supplemented with prefix `UF_USR_`
 ||
|| **USER_TYPE_ID***
[`string`](../../data-types.md)| Custom field type. Possible values:
- `string` — string
- `integer` — integer
- `double` — number
- `date` — date
- `datetime` — date with time
- `boolean` — Yes / No
- `file` — file
- `enumeration` — list
- `url` — link
- `address` — Google Maps address
- `money` — money
- `iblock_section` — Link to infoblock section
- `iblock_element` — Link to infoblock item
- `employee` — Link to user
- `crm` — Link to CRM item
- `crm_status` — Link to CRM directory ||
|| **XML_ID**
[`string`](../../data-types.md)| External code ||
|| **SORT**
[`integer`](../../data-types.md)| Sort order ||
|| **MULTIPLE**
[`boolean`](../../data-types.md)| Whether the field is multiple. Possible values:
- `Y` — yes
- `N` — no
 ||
|| **MANDATORY**
[`boolean`](../../data-types.md)| Whether the custom field is required. Possible values:
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
[`boolean`](../../data-types.md)| Whether to edit the field in the list. Possible values:
- `Y` — yes
- `N` — no ||
- || **IS_SEARCHABLE**
[`boolean`](../../data-types.md)| Whether the field is included in search. Possible values:
- `Y` — yes
- `N` — no ||
|| **SETTINGS**
[`object`](../../data-types.md)| An object in `{"field_1": "value_1", ... "field_N": "value_N"}` format to pass additional custom field settings. Settings are described [below](#settings) ||
|| **EDIT_FORM_LABEL**
[`string`](../../data-types.md)| Label in the editing form. You can pass a string or an object with labels by language in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be set for all languages ||
|| **LIST_COLUMN_LABEL**
[`string`](../../data-types.md)| Column header in the list. You can pass a string or an object with labels by language in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be set for all languages ||
|| **LIST_FILTER_LABEL**
[`string`](../../data-types.md)| Filter header in the list. You can pass a string or an object with labels by language in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be set for all languages ||
|| **ERROR_MESSAGE**
[`string`](../../data-types.md)| Error message for invalid input. You can pass a string or an object with texts by language in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be set for all languages ||
|| **HELP_MESSAGE**
[`string`](../../data-types.md)| Tooltip text for the field. You can pass a string or an object with texts by language in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be set for all languages ||
|| **LABEL**
[`string`](../../data-types.md)| Default custom field name. 

The value will be set in fields `LIST_FILTER_LABEL`, `LIST_COLUMN_LABEL`, `EDIT_FORM_LABEL`, `ERROR_MESSAGE`, `HELP_MESSAGE` if no value is provided in them ||
|#

### Parameter SETTINGS {#settings}

Each custom field type has its own set of additional configurations.

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

- date|datetime

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
    - `TYPE` — type of the default value:
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
    [`string`](../../data-types.md) | Infoblock type identifier.

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

    fields = {
        "FIELD_NAME": "UF_USR_SKILLS_PROFILE",
        "USER_TYPE_ID": "string",
        "XML_ID": "UF_USR_SKILLS_PROFILE",
        "SORT": 150,
        "MULTIPLE": "N",
        "MANDATORY": "N",
        "SHOW_FILTER": "Y",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "Y",
        "SETTINGS": {
            "DEFAULT_VALUE": "Python integration engineer",
            "ROWS": 3,
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
            "en": "Store a short integration skills summary.",
        },
        "LABEL": "Skills profile",
    }

    try:
        bitrix_response = client.user.userfield.add(
            fields=fields,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
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

{% note info "" %}

If it is necessary to create a custom field with an added custom type via the API, you must specify `rest_<app_number>_<added_type_USER_TYPE_ID>` in the `USER_TYPE_ID` field. For example, `rest_436278_test_type`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields": {
            "FIELD_NAME": "UF_USER_DEALS",
            "USER_TYPE_ID": "crm",
            "XML_ID": "UF_CRM_DEALS",
            "SORT": 100,
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "SETTINGS": {
                "DEAL": "Y"
            },
            "LABEL": "CRM Deal Linking",
            "EDIT_FORM_LABEL": {
                "de": "CRM Deal Linking"
            }
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.userfield.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields": {
            "FIELD_NAME": "UF_USER_DEALS",
            "USER_TYPE_ID": "crm",
            "XML_ID": "UF_CRM_DEALS",
            "SORT": 100,
            "MULTIPLE": "Y",
            "MANDATORY": "N",
            "SHOW_FILTER": "N",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "SETTINGS": {
                "DEAL": "Y"
            },
            "LABEL": "CRM Deal Linking",
            "EDIT_FORM_LABEL": {
                "de": "CRM Deal Linking"
            }
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.userfield.add
    ```
    
- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AddUserFieldResult = number

    try {
      const response = await $b24.actions.v2.call.make<AddUserFieldResult>({
        method: 'user.userfield.add',
        params: {
          fields: {
            FIELD_NAME: 'UF_USER_DEALS',
            USER_TYPE_ID: 'crm',
            XML_ID: 'UF_CRM_DEALS',
            SORT: 100,
            MULTIPLE: 'Y',
            MANDATORY: 'N',
            SHOW_FILTER: 'N',
            SHOW_IN_LIST: 'Y',
            EDIT_IN_LIST: 'Y',
            SETTINGS: {
              DEAL: 'Y',
            },
            LABEL: 'CRM deals binding',
            EDIT_FORM_LABEL: {
              ru: 'CRM deals binding',
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
        console.info('Created user field with ID:', result)
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
      async function addUserField() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'user.userfield.add',
            params: {
              fields: {
                FIELD_NAME: 'UF_USER_DEALS',
                USER_TYPE_ID: 'crm',
                XML_ID: 'UF_CRM_DEALS',
                SORT: 100,
                MULTIPLE: 'Y',
                MANDATORY: 'N',
                SHOW_FILTER: 'N',
                SHOW_IN_LIST: 'Y',
                EDIT_IN_LIST: 'Y',
                SETTINGS: {
                  DEAL: 'Y',
                },
                LABEL: 'CRM deals binding',
                EDIT_FORM_LABEL: {
                  ru: 'CRM deals binding',
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
          console.info('Created user field with ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addUserField)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'user.userfield.add',
                [
                    'fields' => [
                        'FIELD_NAME' => 'UF_USER_DEALS',
                        'USER_TYPE_ID' => 'crm',
                        'XML_ID' => 'UF_CRM_DEALS',
                        'SORT' => 100,
                        'MULTIPLE' => 'Y',
                        'MANDATORY' => 'N',
                        'SHOW_FILTER' => 'N',
                        'SHOW_IN_LIST' => 'Y',
                        'EDIT_IN_LIST' => 'Y',
                        'SETTINGS' => [
                            'DEAL' => 'Y',
                        ],
                        'LABEL' => 'CRM Deal Linking',
                        'EDIT_FORM_LABEL' => [
                            'de' => 'CRM Deal Linking'
                        ],
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
        echo 'Error adding user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'user.userfield.add', 
        {
            fields: {
                FIELD_NAME: "UF_USER_DEALS",
                USER_TYPE_ID: "crm",
                XML_ID: "UF_CRM_DEALS",
                SORT: 100,
                MULTIPLE: "Y",
                MANDATORY: "N",
                SHOW_FILTER: "N",
                SHOW_IN_LIST: "Y",
                EDIT_IN_LIST: "Y",
                SETTINGS: {
                    DEAL: "Y",
                },
                LABEL: "CRM Deal Linking",
                EDIT_FORM_LABEL: {
                    ru: "CRM Deal Linking"
                },
            },
        },
        function(result)
        {
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
        'user.userfield.add',
        [
            'fields' => [
                'FIELD_NAME' => 'UF_USER_DEALS',
                'USER_TYPE_ID' => 'crm',
                'XML_ID' => 'UF_CRM_DEALS',
                'SORT' => 100,
                'MULTIPLE' => 'Y',
                'MANDATORY' => 'N',
                'SHOW_FILTER' => 'N',
                'SHOW_IN_LIST' => 'Y',
                'EDIT_IN_LIST' => 'Y',
                'SETTINGS' => [
                    'DEAL' => 'Y',
                ],
                'LABEL' => 'CRM Deal Linking',
                'EDIT_FORM_LABEL' => [
                    'de' => 'CRM Deal Linking'
                ],
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
    "result":177,
    "time":{
        "start":1747301035.550121,
        "finish":1747301037.514112,
        "duration":1.9639909267425537,
        "processing":0.5865437984466553,
        "date_start":"2025-05-15T11:23:55+02:00",
        "date_finish":"2025-05-15T11:23:57+02:00",
        "operating":0
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
[`time`](../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":"",
   "error_description":"The \u0027FIELD_NAME\u0027 field is not found."
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| ERROR_ARGUMENT | Argument 'USER_TYPE_ID' is null or empty | Not specified `USER_TYPE_ID` ||
|| ERROR_ARGUMENT | Argument 'HANDLER' is null or empty | Not specified `HANDLER` ||
|| ERROR_CORE | Field \*** for USER object already exists | Field \*** for `USER` object already exists ||
|| ERROR_CORE | Fail to create new user field | Error while creating field ||
|| Empty string | The \u0027FIELD_NAME\u0027 field is not found. | Mandatory field `FIELD_NAME` is not specified ||
|| Empty string | The \u0027USER_TYPE_ID\u0027 field is not found. | Mandatory field `USER_TYPE_ID` is not specified ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-userfield-update.md)
- [{#T}](./user-userfield-list.md)
- [{#T}](./user-userfield-delete.md)