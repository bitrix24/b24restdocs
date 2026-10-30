# Add custom field user.userfield.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `user.userfield.add` method creates a custom field in the employee profile and returns its ID. The `UF_USR_` prefix is automatically added to the `FIELD_NAME` code. The type and multiplicity of the field cannot be changed after it is created.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md)| Field values for adding a new custom field ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELD_NAME***
[`string`](../../data-types.md)| Field name (code). Bitrix24 converts it to uppercase and supplements it with the `UF_USR_` prefix:

- `DEALS` and `UF_DEALS` become `UF_USR_DEALS`
- `UF_USR_DEALS` remains unchanged

The resulting field code is returned by the [user.userfield.list](./user-userfield-list.md) method ||
|| **USER_TYPE_ID***
[`string`](../../data-types.md)| Custom field type. The type cannot be changed after the field is created. Possible values:
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
[`string`](../../data-types.md)| Whether the field is multiple. Cannot be changed after the field is created. Possible values:
- `Y` — yes
- `N` — no

Default is `N` ||
|| **SHOW_FILTER**
[`string`](../../data-types.md)| Whether to show the field in the list filter. Possible values:
- `Y` — yes. The method saves the value `E`, a mask filter, which is returned by [user.userfield.list](./user-userfield-list.md)
- `N` — no

Default is `N` ||
|| **SETTINGS**
[`object`](../../data-types.md)| Additional field settings. The set depends on the `USER_TYPE_ID` type [(detailed description)](#settings) ||
|| **LIST**
[`object[]`](../../data-types.md)| Value options for an `enumeration` field [(detailed description)](#list) ||
|| **EDIT_FORM_LABEL**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) | Label in the editing form. You can pass a string or an object with labels by language in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be set for all languages ||
|| **LIST_COLUMN_LABEL**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) | Column header in the list. You can pass a string or an object with labels by language in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be set for all languages ||
|| **LIST_FILTER_LABEL**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) | Filter header in the list. You can pass a string or an object with labels by language in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be set for all languages ||
|| **ERROR_MESSAGE**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) | Error message for invalid input. You can pass a string or an object with texts by language in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be set for all languages ||
|| **HELP_MESSAGE**
[`string`](../../data-types.md) \| [`object`](../../data-types.md) | Tooltip text for the field. You can pass a string or an object with texts by language in `{"de": "...", "en": "..."}` format. If a string is passed, the value will be set for all languages ||
|| **LABEL**
[`string`](../../data-types.md)| Default custom field name. 

The value will be set in fields `LIST_FILTER_LABEL`, `LIST_COLUMN_LABEL`, `EDIT_FORM_LABEL`, `ERROR_MESSAGE`, `HELP_MESSAGE` if no value is provided in them ||
|#

The method does not accept the `MANDATORY`, `SHOW_IN_LIST`, `EDIT_IN_LIST`, and `IS_SEARCHABLE` parameters and ignores them without an error. The field is always created with `MANDATORY` = `N`, `SHOW_IN_LIST` = `Y`, `EDIT_IN_LIST` = `Y`, `IS_SEARCHABLE` = `N`.

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

    ```json
    {
        "VALUE": "2026-01-15",
        "TYPE": "FIXED"
    }
    ```

    where:
    - `VALUE` — default value of type `datetime` or `date`
    - `TYPE` — type of the default value:
      - `NONE` — do not set a default value
      - `NOW` — use current time/date
      - `FIXED` — use time/date from `VALUE`. For a `datetime` field, the `FIXED` value is currently saved incorrectly: the method returns the date `1970-01-01`. Use `NOW` or `NONE`

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

    If none of the following options are passed, linking to leads (`LEAD = Y`) will be enabled by default.

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`string`](../../data-types.md) | Whether binding to [Leads](../../crm/leads/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N`. If no binding is enabled, `LEAD` is enabled ||
    || **CONTACT**
    [`string`](../../data-types.md) | Whether binding to [Contacts](../../crm/contacts/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **COMPANY**
    [`string`](../../data-types.md) | Whether binding to [Companies](../../crm/companies/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **DEAL**
    [`string`](../../data-types.md) | Whether binding to [Deals](../../crm/deals/index.md) is enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    |#

{% endlist %}

For `file`, `url`, `address`, `money`, and `employee` fields, the method does not accept `SETTINGS`: the passed settings are ignored, and the field gets the default system settings. For an `employee` field, the `SHOW_FILTER` = `Y` value is saved as `I`, an exact match filter.

### Parameter LIST {#list}

Value options for an `enumeration` field. Passed in `fields` as an array of objects.

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **VALUE***
[`string`](../../data-types.md) | Option text. Options with an empty `VALUE` are not saved ||
|| **SORT**
[`integer`](../../data-types.md) | Option order in the list ||
|| **DEF**
[`string`](../../data-types.md) | Default option:
- `Y` — yes
- `N` — no

For a field with `MULTIPLE` = `N`, only one option can be the default: the first one with `DEF` = `Y` ||
|| **XML_ID**
[`string`](../../data-types.md) | External code of the option. If not passed, it is generated automatically ||
|#

{% note info "" %}

If it is necessary to create a custom field with an added custom type via the API, you must specify `rest_<app_number>_<added_type_USER_TYPE_ID>` in the `USER_TYPE_ID` field. For example, `rest_436278_test_type`.

{% endnote %}

{% note warning "" %}

The field is added to the profiles of all employees, so in a Bitrix24 account with many users the request takes a long time and may be interrupted by a timeout. For more details, see [Common Causes of Server Errors](./index.md#server-errors).

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
            "FIELD_NAME": "UF_USR_DEALS",
            "USER_TYPE_ID": "crm",
            "XML_ID": "UF_CRM_DEALS",
            "SORT": 100,
            "MULTIPLE": "Y",
            "SHOW_FILTER": "N",
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
            "FIELD_NAME": "UF_USR_DEALS",
            "USER_TYPE_ID": "crm",
            "XML_ID": "UF_CRM_DEALS",
            "SORT": 100,
            "MULTIPLE": "Y",
            "SHOW_FILTER": "N",
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
            FIELD_NAME: 'UF_USR_DEALS',
            USER_TYPE_ID: 'crm',
            XML_ID: 'UF_CRM_DEALS',
            SORT: 100,
            MULTIPLE: 'Y',
            SHOW_FILTER: 'N',
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
                FIELD_NAME: 'UF_USR_DEALS',
                USER_TYPE_ID: 'crm',
                XML_ID: 'UF_CRM_DEALS',
                SORT: 100,
                MULTIPLE: 'Y',
                SHOW_FILTER: 'N',
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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.userfield.add(
            fields={
                "FIELD_NAME": "UF_USR_DEALS",
                "USER_TYPE_ID": "crm",
                "XML_ID": "UF_CRM_DEALS",
                "SORT": 100,
                "MULTIPLE": "Y",
                "SHOW_FILTER": "N",
                "SETTINGS": {
                    "DEAL": "Y",
                },
                "LABEL": "Linking to CRM deals",
                "EDIT_FORM_LABEL": {
                    "ru": "Linking to CRM deals",
                },
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
                'user.userfield.add',
                [
                    'fields' => [
                        'FIELD_NAME' => 'UF_USR_DEALS',
                        'USER_TYPE_ID' => 'crm',
                        'XML_ID' => 'UF_CRM_DEALS',
                        'SORT' => 100,
                        'MULTIPLE' => 'Y',
                        'SHOW_FILTER' => 'N',
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
                FIELD_NAME: "UF_USR_DEALS",
                USER_TYPE_ID: "crm",
                XML_ID: "UF_CRM_DEALS",
                SORT: 100,
                MULTIPLE: "Y",
                SHOW_FILTER: "N",
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
                'FIELD_NAME' => 'UF_USR_DEALS',
                'USER_TYPE_ID' => 'crm',
                'XML_ID' => 'UF_CRM_DEALS',
                'SORT' => 100,
                'MULTIPLE' => 'Y',
                'SHOW_FILTER' => 'N',
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "user.userfield.add", b24.Params{
    	"fields": b24.Params{
    		"FIELD_NAME":   "UF_USR_DEALS",
    		"USER_TYPE_ID": "crm",
    		"XML_ID":       "UF_CRM_DEALS",
    		"SORT":         100,
    		"MULTIPLE":     "Y",
    		"SHOW_FILTER":  "N",
    		"SETTINGS": b24.Params{
    			"DEAL": "Y",
    		},
    		"LABEL": "CRM Deal Linking",
    		"EDIT_FORM_LABEL": b24.Params{
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("user.userfield.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 6008111,
    "time": {
        "start": 1793288724,
        "finish": 1793288724.469025,
        "duration": 0.4690248966217041,
        "processing": 0,
        "date_start": "2026-10-29T18:45:24+01:00",
        "date_finish": "2026-10-29T18:45:24+01:00",
        "operating_reset_at": 1793289240,
        "operating": 0.11251688003540039
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
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | The field UF_USR_\*\*\* for entity USER already exists. | A field with this `FIELD_NAME` already exists ||
|| `400` | `ERROR_CORE` | The custom type is invalid. | The `USER_TYPE_ID` contains a type that is not in the list ||
|| `400` | `ERROR_CORE` | Fail to create new user field | Error while creating field ||
|| `400` | Empty string | The \u0027FIELD_NAME\u0027 field is not found. | Mandatory field `FIELD_NAME` is not specified ||
|| `400` | Empty string | The \u0027USER_TYPE_ID\u0027 field is not found. | Mandatory field `USER_TYPE_ID` is not specified ||
|| `400` | Empty string | Access denied. | The user does not have administrator permissions ||
|| `401` | `insufficient_scope` | The request requires higher privileges than provided by the webhook token | The webhook or application does not have the `user.userfield` scope ||
|| `500` | `INTERNAL_SERVER_ERROR` | Internal server error | The limit of 1016 custom fields is exceeded, or the request did not complete within `max_execution_time`. For more details, see [Common Causes of Server Errors](./index.md#server-errors) ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-userfield-update.md)
- [{#T}](./user-userfield-list.md)
- [{#T}](./user-userfield-delete.md)
- [{#T}](../user-update.md)
- [{#T}](../user-get.md)