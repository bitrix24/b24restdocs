# Update Custom Field of crm.requisite.userfield.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates an existing custom field of the requisites.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field. Can be obtained using the method [crm.requisite.userfield.list](./crm-requisite-userfield-list.md) ||
|| **fields***
[`object`](../../../data-types.md) | Set of fields — an object of the form `{"field": "value"[, ...]}`, the values of which need to be changed ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for data exchange operations. Identifier of the object in the external information base. 

The purpose of the field may change by the final developer ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Multiplicity indicator. Possible values:
- `Y` — yes
- `N` — no 
||
|| **MANDATORY**
[`char`](../../../data-types.md) | Mandatory indicator. Possible values:
- `Y` — yes
- `N` — no 
||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Whether to show in the list filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring 
||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Whether to show in the list. Possible values:
- `Y` — yes
- `N` — no 
||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Allow user editing. Possible values:
- `Y` — yes
- `N` — no 
||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Whether the field values participate in search. Possible values:
- `Y` — yes
- `N` — no 
||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md) | Label in the edit form ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md) | Header in the list ||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md) | Filter label in the list ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md) | Error message ||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md) | Help ||
|| **LIST**
[`uf_enum_element`](../../../data-types.md) | List elements. For detailed information, see the section [{#T}](../../universal/user-defined-fields/crm-userfield-enumeration-fields.md) ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional settings (dependent on type). For detailed information, see the section [{#T}](../../universal/user-defined-fields/crm-userfield-settings-fields.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":235,"fields":{"EDIT_FORM_LABEL":"Category","LIST_COLUMN_LABEL":"Category","LIST_FILTER_LABEL":"Category"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.userfield.update
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":235,"fields":{"EDIT_FORM_LABEL":"Category","LIST_COLUMN_LABEL":"Category","LIST_FILTER_LABEL":"Category"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.userfield.update
    ```

- JS

    ```js
    const title = "Category";
    BX24.callMethod(
        "crm.requisite.userfield.update",
        {
            id: 235,
            fields:
            {
                "EDIT_FORM_LABEL": title,
                "LIST_COLUMN_LABEL": title,
                "LIST_FILTER_LABEL": title
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $title = "Category";

    $result = CRest::call(
        'crm.requisite.userfield.update',
        [
            'id' => 235,
            'fields' =>
            [
                'EDIT_FORM_LABEL' => $title,
                'LIST_COLUMN_LABEL' => $title,
                'LIST_FILTER_LABEL' => $title
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1717769551.504986,
        "finish": 1717769551.817433,
        "duration": 0.31244707107543945,
        "processing": 0.04784202575683594,
        "date_start": "2024-06-07T16:12:31+02:00",
        "date_finish": "2024-06-07T16:12:31+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of updating the custom field of the requisites:
- true — updated
- false — not updated 
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The entity with ID '235' is not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Error Text** | **Description** ||
|| Empty string | `Operation is not allowed. Entity ID is not defined` | Custom field with the specified identifier not found ||
|| Empty string | `The entity with ID '235' is not found` | Custom field with the specified identifier not found ||
|| Empty string | `ID is not defined or invalid` | Identifier of the custom field is not specified or has an invalid value ||
|| Empty string | `Access denied` | Insufficient access permissions to modify the custom field ||
|| `ERROR_CORE` | `Fail to update user field` |  Failed to update the custom field ||
|| `ERROR_CORE` | `Fail to save enumeration field values` | Failed to save values of the enumeration type custom field (e.g., when there is a duplication of the external key of one of the values) ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-userfield-add.md)
- [{#T}](./crm-requisite-userfield-get.md)
- [{#T}](./crm-requisite-userfield-list.md)
- [{#T}](./crm-requisite-userfield-delete.md)