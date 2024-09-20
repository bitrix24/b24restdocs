# Get a List of Epics tasks.api.scrum.epic.list

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of epics.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`array`](../../../data-types.md) | An array for sorting the result in the format `{'sorting_field': 'sorting_direction' [, ...]}`. 

Sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending

Possible values for the array elements correspond to the fields in the response of [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md#fields)
||
|| **filter**
[`array`](../../../data-types.md) | An array in the format `{'filter_field': 'filter_value' [, ...]}`.

An additional prefix can be specified for the key to clarify the filter behavior.

Possible prefix values:
- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol does not need to be included in the filter value. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol does not need to be included in the filter value. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol needs to be included in the value. Examples:
  - `"mol%"` — searching for values starting with "mol"
  - `"%mol"` — searching for values ending with "mol"
  - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol needs to be included in the value. Examples:
  - `"mol%"` — searching for values not starting with "mol"
  - `"%mol"` — searching for values not ending with "mol"
  - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)

Possible values for the array elements correspond to the fields in the response of [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md#fields)

||
|| **select**
[`array`](../../../data-types.md) | An array of record fields that will be returned by the method.

Possible values for the array elements correspond to the fields in the response of [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md#fields). You can specify only the fields that are necessary.

If the array contains the value `"*"`, all available fields will be returned.

The default value is an empty array `array()`. This means that all fields from the main query table will be returned.
||
|| **start**
[`integer`](../../../data-types.md) | The page number of the output. Works for HTTPS requests.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:
`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Examples

{% include [Footnote on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "filter": {
            "GROUP_ID": 143,
            ">=ID": 1,
            "<=ID": 50,
            "NAME": "%epic%",
            "!=DESCRIPTION": "old epic"
        },
        "order": {
            "ID": "asc",
            "NAME": "desc"
        },
        "select": ["ID", "NAME", "DESCRIPTION", "CREATED_BY", "MODIFIED_BY", "COLOR"],
        "start": 0
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "filter": {
            "GROUP_ID": 143,
            ">=ID": 1,
            "<=ID": 50,
            "NAME": "%epic%",
            "!=DESCRIPTION": "old epic",
            "CREATED_BY": 1,
            "MODIFIED_BY": 3,
            "COLOR": "#69dafc"
        },
        "order": {
            "ID": "asc",
            "NAME": "desc"
        },
        "select": ["ID", "NAME", "DESCRIPTION", "CREATED_BY", "MODIFIED_BY", "COLOR"],
        "start": 0,
        "auth": "YOUR_ACCESS_TOKEN"
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.epic.list
    ```

- JS

    ```js
    const groupId = 143;
    BX24.callMethod(
        'tasks.api.scrum.epic.list',
        {
            filter: {
                GROUP_ID: groupId,
                '>=ID': 1,
                '<=ID': 50,
                'NAME': '%epic%',
                '!=DESCRIPTION': 'old epic',
                'CREATED_BY': 1,
                'MODIFIED_BY': 3,
                'COLOR': '#69dafc'
            },
            order: {
                'ID': 'asc',
                'NAME': 'desc'
            },
            select: ['ID', 'NAME', 'DESCRIPTION', 'CREATED_BY', 'MODIFIED_BY', 'COLOR'],
            start: 0
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.epic.list',
        [
            'filter' => [
                'GROUP_ID' => 143,
                '>=ID' => 1,
                '<=ID' => 50,
                'NAME' => '%epic%',
                '!=DESCRIPTION' => 'old epic',
                'CREATED_BY' => 1,
                'MODIFIED_BY' => 3,
                'COLOR' => '#69dafc'
            ],
            'order' => [
                'ID' => 'asc',
                'NAME' => 'desc'
            ],
            'select' => ['ID', 'NAME', 'DESCRIPTION', 'CREATED_BY', 'MODIFIED_BY', 'COLOR'],
            'start' => 0
        ]
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    }
    else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
[
    {
        "id": 1,
        "groupId": 143,
        "name": "epic",
        "description": "",
        "createdBy": 1,
        "modifiedBy": 0,
        "color": "#69dafc"
    },
    {
        "id": 3,
        "groupId": 143,
        "name": "epic2",
        "description": "new epic",
        "createdBy": 3,
        "modifiedBy": 5,
        "color": "#69dagc"
    }
]
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Epic identifier ||
|| **groupId**
[`integer`](../../../data-types.md) | Group identifier (scrum) to which the epic is attached ||
|| **name**
[`string`](../../../data-types.md) | Epic name ||
|| **description**
[`string`](../../../data-types.md) | Epic description ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the epic ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the epic ||
|| **color**
[`string`](../../../data-types.md) | Epic color in HEX format ||

|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Could not load list"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**  | **Value** ||
|| `0` | Could not load list| No epics found with the specified filters ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-add.md)
- [{#T}](./tasks-api-scrum-epic-update.md)
- [{#T}](./tasks-api-scrum-epic-get.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)
- [{#T}](./tasks-api-scrum-epic-get-fields.md)