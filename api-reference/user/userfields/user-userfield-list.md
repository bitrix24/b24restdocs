# Get a list of custom fields user.userfield.list

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of custom fields based on a filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`string`](../../data-types.md)\|[`array`](../../data-types.md) | Sorting of the selected custom fields in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field_N`:

- `ID` — identifier of the custom field
- `FIELD_NAME` — name of the field
- `USER_TYPE_ID` — data type of the field
- `XML_ID` — external code
- `SORT` — sorting order of the field
- `MULTIPLE` — ability to input multiple values
- `MANDATORY` — requirement for filling
- `SHOW_FILTER` — display of the field in the user list filter
- `SHOW_IN_LIST` — display of the field in the user list
- `EDIT_IN_LIST` — ability to edit the field in the user list
- `IS_SEARCHABLE` — ability to search by the field

Possible values for `order_N`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **filter** 
[`array`](../../data-types.md)| Filter for the selected custom fields in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field_N` are similar to those in sorting.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@` — NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
  - `"mol%"` — searching for values starting with "mol"
  - `"%mol"` — searching for values ending with "mol"
  - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
  - `"mol%"` — searching for values not starting with "mol"
  - `"%mol"` — searching for values not ending with "mol"
  - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal
 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"id":"desc"},"filter":{"id":13}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.userfield.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"id":"desc"},"filter":{"id":13},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/user.userfield.list
    ```

- JS

    ```js
    BX24.callMethod(
        "user.userfield.list",
        {
            order: {
                id: 'desc',
            },
            filter: {
                id: 13
            },
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.userfield.list',
        [
            'order' => [
                'id' => 'desc',
            ],
            'filter' => [
                'id' => 13,
            ],
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
    "result":[
        {
            "ID":"176",
            "ENTITY_ID":"USER",
            "FIELD_NAME":"UF_USR_1724228142162",
            "USER_TYPE_ID":"enumeration",
            "XML_ID":null,
            "SORT":"100",
            "MULTIPLE":"Y",
            "MANDATORY":"N",
            "SHOW_FILTER":"E",
            "SHOW_IN_LIST":"Y",
            "EDIT_IN_LIST":"Y",
            "IS_SEARCHABLE":"N",
            "SETTINGS": {
                "DISPLAY":"UI",
                "LIST_HEIGHT":1,
                "CAPTION_NO_VALUE":"",
                "SHOW_NO_VALUE":"Y"
            },
            "LIST":[
                {
                    "ID":"26",
                    "SORT":"0",
                    "VALUE":"1",
                    "DEF":"N",
                    "XML_ID":"2a53ce08b86a6aba152b1b19204fdef2"
                },
                {
                    "ID":"27",
                    "SORT":"100",
                    "VALUE":"2",
                    "DEF":"N",
                    "XML_ID":"292df2be1171ed6ab038996deac29ac8"
                },
                {
                    "ID":"28",
                    "SORT":"200",
                    "VALUE":"3",
                    "DEF":"N",
                    "XML_ID":"3c5af70eafbba79a6cf52e299ed75123"
                }
            ]
        },
        {
            "ID":"177",
            "ENTITY_ID":"USER",
            "FIELD_NAME":"UF_USR_MONEY",
            "USER_TYPE_ID":"money",
            "XML_ID":null,
            "SORT":"100",
            "MULTIPLE":"N",
            "MANDATORY":"N",
            "SHOW_FILTER":"N",
            "SHOW_IN_LIST":"Y",
            "EDIT_IN_LIST":"Y",
            "IS_SEARCHABLE":"N",
            "SETTINGS": {
                "DEFAULT_VALUE":""
            }
        }
    ],
    "total":2,
    "time": {
        "start":1747313326.788124,
        "finish":1747313328.641663,
        "duration":1.853538990020752,
        "processing":0.011211156845092773,
        "date_start":"2025-05-15T14:48:46+02:00",
        "date_finish":"2025-05-15T14:48:48+02:00",
        "operating":0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
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
|| Empty string | Access denied. | A field with such `id` does not exist or access is denied ||
|| Empty string | ID is not defined or invalid | The `id` is not specified or is incorrect ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-userfield-add.md)
- [{#T}](./user-userfield-update.md)
- [{#T}](./user-userfield-delete.md)