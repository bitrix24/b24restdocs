# Add Custom Field user.userfield.add

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `user.userfield.add` adds a custom field.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md)| Values of the fields to add a new custom field ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FIELD_NAME***
[`string`](../../data-types.md)| Name (code) of the field. Prefixed with `UF_USR_`
 ||
|| **USER_TYPE_ID***
[`string`](../../data-types.md)| Type of the custom field. Possible values:
- `string` — string
- `integer` — integer
- `double` — number
- `date` — date
- `datetime` — date with time
- `boolean` — Yes / No
- `file` — file
- `enumeration` — list
- `url` — link
- `address` — Google map address
- `money` — money
- `iblock_section` — Binding to the information block section
- `iblock_element` — Binding to the information block element
- `employee` — Binding to the user
- `crm` — Binding to the CRM entity
- `crm_status` — Binding to the CRM directory ||
|| **XML_ID**
[`string`](../../data-types.md)| External code ||
|| **SORT**
[`integer`](../../data-types.md)| Sorting order ||
|| **MULTIPLE**
[`boolean`](../../data-types.md)| Is the field multiple. Possible values:
- `Y` — yes
- `N` — no
 ||
|| **MANDATORY**
[`boolean`](../../data-types.md)| Is the custom field mandatory. Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_FILTER**
[`boolean`](../../data-types.md)| Should the field be shown in the list filter. Possible values:
- `Y` — yes
- `N` — no ||
|| **SHOW_IN_LIST**
[`boolean`](../../data-types.md)| Should the field be shown in the list. Possible values:
- `Y` — yes
- `N` — no ||
- || **EDIT_IN_LIST**
[`boolean`](../../data-types.md)| Can the field be edited in the list. Possible values:
- `Y` — yes
- `N` — no ||
- || **IS_SEARCHABLE**
[`boolean`](../../data-types.md)| Is the field searchable. Possible values:
- `Y` — yes
- `N` — no ||
|| **SETTINGS**
[`object`](../../data-types.md)| Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}` for passing additional settings for custom fields. Settings are described [below](#settings) ||
|| **EDIT_FORM_LABEL**
[`string`](../../data-types.md)| Label in the edit form ||
|| **LIST_COLUMN_LABEL**
[`string`](../../data-types.md)| Column header in the list ||
|| **LIST_FILTER_LABEL**
[`string`](../../data-types.md)| Filter header in the list ||
|| **ERROR_MESSAGE**
[`string`](../../data-types.md)| Error message for invalid input ||
|| **HELP_MESSAGE**
[`string`](../../data-types.md)| Help text for the field ||
|| **LABEL**
[`string`](../../data-types.md)| Default name of the custom field.

The provided value will be set in the following fields: `LIST_FILTER_LABEL`, `LIST_COLUMN_LABEL`, `EDIT_FORM_LABEL`, `ERROR_MESSAGE`, `HELP_MESSAGE`, if no value is provided in them ||
|#

### Parameter SETTINGS {#settings}

Each type of custom fields has its own set of additional settings.

{% list tabs %}

- string

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`string`](../../data-types.md) | Default value.

    Default is `''` ||
    || **ROWS**
    [`integer`](../../data-types.md) | Number of rows in the input field. Must be greater than 0.

    Default is `1` ||
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
    [`integer`](../../data-types.md) | Precision of the number. Must be greater than or equal to 0.

    Default is `2` ||
    |#

- boolean

    #|
    || **Name**
    `type` | **Description** ||
    || **DEFAULT_VALUE**
    [`integer`](../../data-types.md) | Default value, where `1` — yes, `0` — no.

    Possible values:
    - `>= 1` -> 1
    - `<= 0` -> 0

    Default is `0` ||
    || **DISPLAY**
    [`string`](../../data-types.md) | Appearance. Possible values:
    - `CHECKBOX` — checkbox
    - `RADIO` — radio buttons
    - `DROPDOWN` — dropdown list

    Default is `CHECKBOX` ||
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
      - `NOW` — use the current time/date
      - `FIXED` — use the time/date from `VALUE`

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
    - `UI` — input list
    - `CHECKBOX` — checkboxes
    - `DIALOG` — entity selection dialog

    Default is `LIST` ||
    || **LIST_HEIGHT** | Height of the list. Must be greater than 0.

    Available only when `DISPLAY = LIST` or `DISPLAY = UI`.

    Default is `1` ||
    |#

- iblock_section|iblock_element

    #|
    || **Name**
    `type` | **Description** ||
    || **IBLOCK_TYPE_ID**
    [`string`](../../data-types.md) | Identifier of the information block type.

    Default is `''` ||
    || **IBLOCK_ID**
    [`string`](../../data-types.md) | Identifier of the information block.

    Default is `0` ||
    || **DEFAULT_VALUE**
    [`string`](../../data-types.md) | Default value.

    Default is `''` ||
    || **DISPLAY**
    [`string`](../../data-types.md) | Appearance. Possible values:
    - `DIALOG` — dialog
    - `UI` — input list
    - `LIST` — list
    - `CHECKBOX` — checkboxes

    Default is `LIST` ||
    || **LIST_HEIGHT**
    [`integer`](../../data-types.md) | Height of the list. Must be greater than 0.

    Default is `1` ||
    || **ACTIVE_FILTER**
    [`boolean`](../../data-types.md) | Show elements with the active flag. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
    || **ENTITY_TYPE**
    [`string`](../../data-types.md) | Identifier of the directory type.

    Use [`crm.status.entity.types`](../../crm/status/crm-status-entity-types.md) to find possible values.

    Default is `''` ||
    |#

- crm

    If none of the following options are provided, the binding to leads will be enabled by default (`LEAD = Y`).

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`boolean`](../../data-types.md) | Is the binding to [Leads](../../crm/leads/index.md) enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **CONTACT**
    [`boolean`](../../data-types.md) | Is the binding to [Contacts](../../crm/contacts/index.md) enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **COMPANY**
    [`boolean`](../../data-types.md) | Is the binding to [Companies](../../crm/companies/index.md) enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **DEAL**
    [`boolean`](../../data-types.md) | Is the binding to [Deals](../../crm/deals/index.md) enabled. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    |#

{% endlist %}

{% note info "" %}

If you need to create a custom field with an added custom type via the API, you must specify `rest_<app_number>_<USER_TYPE_ID of added type>` in the `USER_TYPE_ID` field. For example, `rest_436278_test_type`.

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
            "EDIT_FORM_LABEL": {
                "en": "Binding to CRM Deals"
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
            "EDIT_FORM_LABEL": {
                "en": "Binding to CRM Deals"
            }
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.userfield.add
    ```

- JS

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
                EDIT_FORM_LABEL: {
                    en: "Binding to CRM Deals"
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

- PHP

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
                'EDIT_FORM_LABEL' => [
                    'en' => 'Binding to CRM Deals'
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
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":"",
   "error_description":"The \u0027FIELD_NAME\u0027 field is not found."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| ERROR_ARGUMENT | Argument 'USER_TYPE_ID' is null or empty | 'USER_TYPE_ID' is not set ||
|| ERROR_ARGUMENT | Argument 'HANDLER' is null or empty | 'HANDLER' is not set ||
|| ERROR_CORE | Field \*** for USER object already exists | Field \*** for `USER` object already exists ||
|| ERROR_CORE | Fail to create new user field | Error creating field ||
|| Empty string | The \u0027FIELD_NAME\u0027 field is not found. | Mandatory field `FIELD_NAME` is not set ||
|| Empty string | The \u0027USER_TYPE_ID\u0027 field is not found. | Mandatory field `USER_TYPE_ID` is not set ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-userfield-update.md)
- [{#T}](./user-userfield-list.md)
- [{#T}](./user-userfield-delete.md)