# Get a List of Custom Fields of the Requisite by Filter crm.requisite.userfield.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of custom fields of the requisite based on the filter.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected custom fields in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field`:
- ID
- ENTITY_ID
- FIELD_NAME
- USER_TYPE_ID
- XML_ID
- SORT

Possible values for `order`:
- asc — in ascending order
- desc — in descending order
||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected requisites in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. This method's filter only supports simple value comparisons.

Possible values for `field`:
- ID
- ENTITY_ID
- FIELD_NAME
- USER_TYPE_ID
- XML_ID
- SORT
- MULTIPLE
- MANDATORY
- SHOW_FILTER
- SHOW_IN_LIST
- EDIT_IN_LIST
- IS_SEARCHABLE
- LANG

Additional prefixes in keys that clarify filter behavior are not provided. The `equals` operation is always used.

Possible values for `value` correspond to [field descriptions](#fields-description) 
||
|#

### Description of Custom Field of the Requisite {#fields-description}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`int`](../../../data-types.md) | Identifier of the custom field ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Identifier of the entity to which the custom field belongs. For requisites, this is always `CRM_REQUISITE` ||
|| **FIELD_NAME^*^**
[`string`](../../../data-types.md) | Symbolic code. For requisites, it always starts with the prefix `UF_CRM_` ||
|| **USER_TYPE_ID^*^**
[`string`](../../../data-types.md) | Data type ([`string`](../../universal/user-defined-fields/crm-userfield-types.md), [`boolean`](../../universal/user-defined-fields/crm-userfield-types.md), [`double`](../../universal/user-defined-fields/crm-userfield-types.md) or [`datetime`](../../universal/user-defined-fields/crm-userfield-types.md)) ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information base. 

The purpose of the field may change by the final developer ||
|| **SORT**
[`int`](../../../data-types.md) | Sorting ||
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
[`char`](../../../data-types.md) | Whether to allow editing by the user. Possible values:
- `Y` — yes
- `N` — no 
||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Whether the field values participate in the search. Possible values:
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

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"MANDATORY":"N","LANG":"en"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.userfield.list
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"SORT":"ASC"},"filter":{"MANDATORY":"N","LANG":"en"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.userfield.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.userfield.list",
        {
            order: { "SORT": "ASC" },
            filter: { "MANDATORY": "N", "LANG": "en" }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.userfield.list',
        [
            'order' => ['SORT' => 'ASC'],
            'filter' => ['MANDATORY' => 'N', 'LANG' => 'en']
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
    "result": [
        {
        "ID": "232",
        "ENTITY_ID": "CRM_REQUISITE",
        "FIELD_NAME": "UF_CRM_NEWTECH_V1_BOOLEAN",
        "USER_TYPE_ID": "boolean",
        "XML_ID": null,
        "SORT": "100",
        "MULTIPLE": "N",
        "MANDATORY": "N",
        "SHOW_FILTER": "E",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "DEFAULT_VALUE": 0,
            "DISPLAY": "CHECKBOX",
            "LABEL": [
            "",
            ""
            ],
            "LABEL_CHECKBOX": "UF - Yes/No"
        },
        "EDIT_FORM_LABEL": "UF - Yes/No",
        "LIST_COLUMN_LABEL": "UF - Yes/No",
        "LIST_FILTER_LABEL": "UF - Yes/No",
        "ERROR_MESSAGE": null,
        "HELP_MESSAGE": null
        },
        {
        "ID": "233",
        "ENTITY_ID": "CRM_REQUISITE",
        "FIELD_NAME": "UF_CRM_NEWTECH_V1_DATETIME",
        "USER_TYPE_ID": "datetime",
        "XML_ID": null,
        "SORT": "100",
        "MULTIPLE": "N",
        "MANDATORY": "N",
        "SHOW_FILTER": "E",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "DEFAULT_VALUE": {
            "TYPE": "NONE",
            "VALUE": ""
            },
            "USE_SECOND": "Y",
            "USE_TIMEZONE": "N"
        },
        "EDIT_FORM_LABEL": "UF - Date",
        "LIST_COLUMN_LABEL": "UF - Date",
        "LIST_FILTER_LABEL": "UF - Date",
        "ERROR_MESSAGE": null,
        "HELP_MESSAGE": null
        },
        {
        "ID": "234",
        "ENTITY_ID": "CRM_REQUISITE",
        "FIELD_NAME": "UF_CRM_NEWTECH_V1_DOUBLE",
        "USER_TYPE_ID": "double",
        "XML_ID": null,
        "SORT": "100",
        "MULTIPLE": "N",
        "MANDATORY": "N",
        "SHOW_FILTER": "E",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "PRECISION": 2,
            "SIZE": 20,
            "MIN_VALUE": 0,
            "MAX_VALUE": 0,
            "DEFAULT_VALUE": null
        },
        "EDIT_FORM_LABEL": "UF - Number",
        "LIST_COLUMN_LABEL": "UF - Number",
        "LIST_FILTER_LABEL": "UF - Number",
        "ERROR_MESSAGE": null,
        "HELP_MESSAGE": null
        },
        {
        "ID": "235",
        "ENTITY_ID": "CRM_REQUISITE",
        "FIELD_NAME": "UF_CRM_NEWTECH_V1_STRING",
        "USER_TYPE_ID": "string",
        "XML_ID": null,
        "SORT": "100",
        "MULTIPLE": "N",
        "MANDATORY": "N",
        "SHOW_FILTER": "N",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "SIZE": 20,
            "ROWS": 1,
            "REGEXP": "",
            "MIN_LENGTH": 0,
            "MAX_LENGTH": 0,
            "DEFAULT_VALUE": ""
        },
        "EDIT_FORM_LABEL": "UF - String",
        "LIST_COLUMN_LABEL": "UF - String",
        "LIST_FILTER_LABEL": "UF - String",
        "ERROR_MESSAGE": "UF_CRM_NEWTECH_V1_STRING",
        "HELP_MESSAGE": "UF_CRM_NEWTECH_V1_STRING"
        }
    ],
    "total": 4,
    "time": {
        "start": 1717754823.56747,
        "finish": 1717754823.938955,
        "duration": 0.37148499488830566,
        "processing": 0.007915973663330078,
        "date_start": "2024-06-07T12:07:03+02:00",
        "date_finish": "2024-06-07T12:07:03+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md)| An array of objects containing information from the selected custom fields. Each element contains the selected [fields describing the custom field of the requisite](#fields-description) ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Error Text** | **Description** ||
|| `Access denied` | Insufficient access permissions to retrieve the list of custom fields of the requisite ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-userfield-add.md)
- [{#T}](./crm-requisite-userfield-update.md)
- [{#T}](./crm-requisite-userfield-get.md)
- [{#T}](./crm-requisite-userfield-delete.md)