# Get a list of digital workspaces crm.automatedsolution.list

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

The method will return an array of digital workspace settings. Each element of the array is a structure similar to the response from the request [crm.automatedsolution.get](./crm-automated-solution-get.md).

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`](../../data-types.md) | List for sorting in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where the key is the field and the value is `ASC` or `DESC`. Available fields for sorting:
- `id`
- `title` 
||
|| **filter**
[`object`](../../data-types.md) | Object for filtering selected digital workspaces in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. Available fields for filtering:
- `id`
- `title`
 
The filter can have unlimited nesting and number of conditions. By default, all conditions are combined using `AND`. If you need to use `OR`, you can pass a special key `logic` with the value `OR`.

An additional prefix can be assigned to the key to specify the filter's behavior. Possible prefix values:

- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position.

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.

- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position.

- `!%=` — NOT LIKE (see description above)

- `=` — equals, exact match (used by default)
- `!=` - not equal
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for pagination control.
 
The page size of results is always static: 50 records.
 
To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` is the desired page number
 ||
|#

## Code Examples

1. Get all digital workspaces sorted by descending `id`

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{"id":"DESC"}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.list
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{"id":"DESC"},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.list
        ```

    - JS

        ```js
        BX24.callMethod(
            "crm.automatedsolution.list",
            {
                "order": {
                    "id": "DESC",
                }
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
            'crm.automatedsolution.list',
            [
                'order' => [
                    'id' => 'DESC'
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Get all digital workspaces whose titles start with "HR"

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"filter":{"%=title":"HR%"}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.list
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"filter":{"%=title":"HR%"},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.list
        ```

    - JS

        ```js
        BX24.callMethod(
            "crm.automatedsolution.list",
            {
                "filter": {
                    "%=title": "HR%"
                }
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
            'crm.automatedsolution.list',
            [
                'filter' => [
                    '%=title' => 'HR%'
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

3. Get all digital workspaces whose titles start with "HR" or "Customer" and `id` greater than `100`, sorted by title

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{"title":"ASC"},"filter":{">id":100,"0":{"logic":"OR","0":{"%=title":"HR%"},"1":{"%=title":"Customer%"}}}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.list
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{"title":"ASC"},"filter":{">id":100,"0":{"logic":"OR","0":{"%=title":"HR%"},"1":{"%=title":"Customer%"}}},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.list
        ```

    - JS

        ```js
        BX24.callMethod(
            "crm.automatedsolution.list",
            {
                "order": {
                    "title": "ASC"
                },
                "filter": {
                    ">id": 100,
                    "0": {
                        "logic": "OR",
                        "0": {
                            "%=title": "HR%"
                        },
                        "1": {
                            "%=title": "Customer%"
                        }
                    }
                }
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
            'crm.automatedsolution.list',
            [
                'order' => [
                    'title' => 'ASC'
                ],
                'filter' => [
                    '>id' => 100,
                    '0' => [
                        'logic' => 'OR',
                        '0' => [
                            '%=title' => 'HR%'
                        ],
                        '1' => [
                            '%=title' => 'Customer%'
                        ]
                    ]
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
	"result": {
		"automatedSolutions": [
			{
				"id": 238,
				"title": "HR",
				"typeIds": [
					129
				]
			},
			{
				"id": 240,
				"title": "Customer Success",
				"typeIds": []
			},
			{
				"id": 267,
				"title": "R&D",
				"typeIds": [
					14,
					158
				]
			}
		]
	},
	"total": 3,
	"time": {
		"start": 1715849396.642359,
		"finish": 1715849396.954623,
		"duration": 0.31226396560668945,
		"processing": 0.0068209171295166016,
		"date_start": "2024-05-16T11:49:56+03:00",
		"date_finish": "2024-05-16T11:49:56+03:00",
		"operating_reset_at": 1715849996,
		"operating": 0
	}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **automatedSolutions**
[`object`](../../data-types.md) | Array of objects with information about selected payments ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ACCESS_DENIED",
    "error_description":"Insufficient permissions"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `INVALID_ARG_VALUE` | Invalid input argument value. Details can be found in the error message ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-automated-solution-add.md)
- [{#T}](./crm-automated-solution-update.md)
- [{#T}](./crm-automated-solution-get.md)
- [{#T}](./crm-automated-solution-delete.md)
- [{#T}](./crm-automated-solution-fields.md)