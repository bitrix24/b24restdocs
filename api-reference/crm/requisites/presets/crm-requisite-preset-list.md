# Get a List of Requisite Templates by Filter crm.requisite.preset.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of requisite templates based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array of fields to be selected (see [template fields](#fields)).

If the array is not provided or an empty array is passed, all available template fields will be selected. ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected templates in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to [template fields](#fields).

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character should not be included in the filter value. The search is conducted from both sides.
- `!=%` — NOT LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal 
||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected templates in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to [template fields](#fields).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static: 50 records.

To select the second page of results, the value `50` must be passed. To select the third page of results, the value should be `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

### Description of Requisite Template Fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the requisite. Automatically created and unique within the account. ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object's type.

The identifiers of CRM object types are provided by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md). 
||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of requisite template fields (to get available values, see the method [crm.requisite.preset.countries](./crm-requisite-preset-countries.md)). ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Creation date. ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date. Contains an empty string if the template has not been modified since creation. ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite. ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the requisite. ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the requisite. ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the external information base object. 

The purpose of the field may change by the final developer. 

Each application ensures the uniqueness of values in this field. It is recommended to use a unique prefix to avoid collisions with other applications. 

Values of the form `#CRM_REQUISITE_PRESET_DEF_...` are reserved in CRM for identifying templates that are used by default. These identifiers should not be used for your purposes, as this may disrupt logic. ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status. Values `Y` or `N` are used. Determines the availability of the template in the selection list when adding requisites. ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting. ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Searching for templates by country binding:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"COUNTRY_ID":"1"},"select":["ID","NAME"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.list
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"COUNTRY_ID":"1"},"select":["ID","NAME"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.preset.list",
        {
            order: { "ID": "ASC" },
            filter: { "COUNTRY_ID": "1"},
            select: [ "ID", "NAME"]
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
        'crm.requisite.preset.list',
        [
            'order' => ['ID' => 'ASC'],
            'filter' => ['COUNTRY_ID' => '1'],
            'select' => ['ID', 'NAME']
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
    "result": [
        {
            "ID": "1",
            "NAME": "Organization"
        },
        {
            "ID": "2",
            "NAME": "Individual Entrepreneur"
        },
        {
            "ID": "3",
            "NAME": "Individual"
        },
        {
            "ID": "4",
            "NAME": "Organization (additional)"
        }
    ],
    "total": 4,
    "time": {
        "start": 1716564019.674636,
        "finish": 1716564020.067474,
        "duration": 0.3928380012512207,
        "processing": 0.02863287925720215,
        "date_start": "2024-05-24T17:20:19+02:00",
        "date_finish": "2024-05-24T17:20:20+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md)| An array of objects containing information about the selected templates. Each element contains the selected [template fields](#fields). ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found. ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error":0,
    "error_description":"Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `Access denied` | Insufficient access permissions to retrieve the list of templates. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-requisite-preset-add.md)
- [{#T}](./crm-requisite-preset-update.md)
- [{#T}](./crm-requisite-preset-countries.md)
- [{#T}](./crm-requisite-preset-get.md)
- [{#T}](./crm-requisite-preset-delete.md)
- [{#T}](./crm-requisite-preset-fields.md)