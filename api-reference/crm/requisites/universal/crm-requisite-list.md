# Get a List of Requisites by Filter crm.requisite.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves a list of requisites based on a filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array containing the list of fields to be selected (see [requisite fields](./index.md#fields)).

If not provided or an empty array is passed, all available requisite fields will be selected. ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected requisites in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to [requisite fields](./index.md#fields).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:

- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
   - "mol%" — searching for values starting with "mol"
   - "%mol" — searching for values ending with "mol"
   - "%mol%" — searching for values where "mol" can be in any position.

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search is performed from both sides.

- `!=%` — NOT LIKE, substring search. The `%` symbol must be included in the value. Examples:
   - "mol%" — searching for values not starting with "mol"
   - "%mol" — searching for values not ending with "mol"
   - "%mol%" — searching for values where the substring "mol" is not present in any position.

- `!%=` — NOT LIKE (see description above)

- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
  || **order**
  [`object`](../../../data-types.md) | An object for sorting the selected requisites in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to [requisite fields](./index.md#fields).

Possible values for `order`:

- `asc` — ascending order
- `desc` — descending order
  ||
  || **start**
  [`integer`](../../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static: 50 records.

To select the second page of results, the value `50` must be passed. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Example

{% include [Example Notes](../../../../_includes/examples.md) %}

1. Retrieving requisites by template ID

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{"DATE_CREATE":"ASC"},"filter":{"PRESET_ID":"1"},"select":["ENTITY_TYPE_ID","ENTITY_ID","ID","NAME"]}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.list
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{"DATE_CREATE":"ASC"},"filter":{"PRESET_ID":"1"},"select":["ENTITY_TYPE_ID","ENTITY_ID","ID","NAME"],"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.requisite.list
        ```

    - JS

        ```js
        BX24.callMethod(
            "crm.requisite.list",
            {
                order: { "DATE_CREATE": "ASC" },
                filter: { "PRESET_ID": "1"},
                select: [ "ENTITY_TYPE_ID", "ENTITY_ID", "ID", "NAME" ]
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
            'crm.requisite.list',
            [
                'order' => ["DATE_CREATE" => "ASC"],
                'filter' => ["PRESET_ID" => "1"],
                'select' => ["ENTITY_TYPE_ID", "ENTITY_ID", "ID", "NAME"]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Retrieving the value of a custom field in requisites

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{},"filter":{"ID":"51"},"select":["UF_CRM_1707997209"]}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.list
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{},"filter":{"ID":"51"},"select":["UF_CRM_1707997209"],"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.requisite.list
        ```

    - JS

        ```js
        BX24.callMethod(
            "crm.requisite.list",
            {
                order: {},
                filter: { "ID": "51"}, // Requisite ID
                select: [ "UF_CRM_1707997209"] // Custom field ID
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
            'crm.requisite.list',
            [
                'order' => [],
                'filter' => ['ID' => '51'],
                'select' => ['UF_CRM_1707997209']
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

## Successful Response

HTTP status: **200**

1. Response from example 1:

    ```json
    {
    "result": [
        {
        "ENTITY_TYPE_ID": "4",
        "ENTITY_ID": "3027",
        "ID": "40",
        "NAME": "Organization"
        },
        {
        "ENTITY_TYPE_ID": "4",
        "ENTITY_ID": "3028",
        "ID": "41",
        "NAME": "Head Office Requisites"
        },
        {
        "ENTITY_TYPE_ID": "4",
        "ENTITY_ID": "3028",
        "ID": "42",
        "NAME": "Branch in Chernyakhovsk"
        }
    ],
    "total": 3,
    "time": {
        "start": 1717150154.197056,
        "finish": 1717150154.505106,
        "duration": 0.30804991722106934,
        "processing": 0.030454158782958984,
        "date_start": "2024-05-31T12:09:14+02:00",
        "date_finish": "2024-05-31T12:09:14+02:00",
        "operating": 0
    }
    }
    ```

2. Response from example 2:

    ```json
    {
        "result": [
            {
                "UF_CRM_1707997209": "45"
            }
        ],
        "total": 1,
        "time": {
            "start": 1717151052.551011,
            "finish": 1717151052.94743,
            "duration": 0.39641880989074707,
            "processing": 0.028468847274780273,
            "date_start": "2024-05-31T12:24:12+02:00",
            "date_finish": "2024-05-31T12:24:12+02:00",
            "operating": 0
        }
    }
    ```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md)| An array of objects containing information from the selected requisites. Each element contains the selected [requisite fields](./index.md#fields) ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Response

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Error Text** | **Description** ||
|| `0` | Access denied. | Insufficient access permissions to retrieve the list of requisites. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-requisite-add.md)
- [{#T}](./crm-requisite-update.md)
- [{#T}](./crm-requisite-get.md)
- [{#T}](./crm-requisite-delete.md)
- [{#T}](./crm-requisite-fields.md)