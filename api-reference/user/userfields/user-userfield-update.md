# Update User Field user.userfield.update

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates a user field.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md)| Identifier of the user field.

To obtain identifiers of user fields, use the [user.userfield.list](./user-userfield-list.md) method.
 ||
|| **fields**
[`object`](../../data-types.md)| Values of the fields to update the user field ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **XML_ID**
[`string`](../../data-types.md)| External code ||
|| **SORT**
[`integer`](../../data-types.md)| Sorting order ||
|| **MANDATORY**
[`boolean`](../../data-types.md)| Whether the user field is mandatory. Possible values:
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
[`boolean`](../../data-types.md)| Whether the field is searchable. Possible values:
- `Y` — yes
- `N` — no ||
|| **SETTINGS**
[`object`](../../data-types.md)| Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}` for passing additional settings for user fields. Settings are described [below](#settings) ||
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
|#

### Parameter SETTINGS {#settings}

Each type of user field has its own set of additional settings.

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
    [`boolean`](../../data-types.md) | Whether to show elements with the active flag. Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    |#

- crm_status

    #|
    || **Name**
    `type` | **Description** ||
    || **ENTITY_TYPE**
    [`string`](../../data-types.md) | Identifier of the reference type.

    Use [`crm.status.entity.types`](../../crm/status/crm-status-entity-types.md) to find possible values.

    Default is `''` ||
    |#

- crm

    If none of the following options are provided, the binding to leads will be enabled by default (`LEAD = Y`).

    #|
    || **Name**
    `type` | **Description** ||
    || **LEAD**
    [`boolean`](../../data-types.md) | Is the binding to [Leads](../../crm/leads/index.md) enabled? Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **CONTACT**
    [`boolean`](../../data-types.md) | Is the binding to [Contacts](../../crm/contacts/index.md) enabled? Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **COMPANY**
    [`boolean`](../../data-types.md) | Is the binding to [Companies](../../crm/companies/index.md) enabled? Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    || **DEAL**
    [`boolean`](../../data-types.md) | Is the binding to [Deals](../../crm/deals/index.md) enabled? Possible values:
    - `Y` — yes
    - `N` — no

    Default is `N` ||
    |#

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

- JS

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

- PHP

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
[`boolean`](../../data-types.md) | Contains `true` in case of successful update of the user field ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"",
    "error_description":"Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | Field with such `id` does not exist or access is denied ||
|| Empty string | ID is not defined or invalid | `id` is not set or is invalid ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-userfield-add.md)
- [{#T}](./user-userfield-list.md)
- [{#T}](./user-userfield-delete.md)