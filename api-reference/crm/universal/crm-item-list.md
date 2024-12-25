# Get a list of crm.item.list elements

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "read" access permission for CRM entity elements

The method retrieves a list of elements of a specific type of CRM entity.

CRM entity elements will not be included in the final selection if the user does not have "read" access permission for these elements.  

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](./index.md) or [user-defined type](./user-defined-object-types/index.md) whose elements need to be retrieved ||
|| **select**
[`array`][1] | List of fields that should be populated in the selected elements.

Can contain only field names or `'*'`.

The list of available fields for selection can be obtained using the [`crm.item.fields`](./crm-item-fields.md) method.
||
|| **filter**
[`object`][1] |
Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where
- `field_n` — the name of the field by which the selection of elements will be filtered
- `value_n` — the filter value

The filter can have unlimited nesting and number of conditions.
By default, all conditions are combined with `AND`. If you need to use `OR`, you can pass a special key `logic` with the value `OR`.

You can add a prefix to the `field_n` keys to specify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character does not need to be passed in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` character needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character does not need to be passed in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` character needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The list of available fields for filtering can be obtained using the [`crm.item.fields`](./crm-item-fields.md) method. 
The filter for deals `entityTypeId: 2` does not support the fields `contactIds` and `contacts` 
||
|| **order**
[`object`][1] |
Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where
- `field_n` — the name of the field by which the selection of elements will be sorted
- `value_n` — a `string` value equal to:
  - `ASC` — ascending sort
  - `DESC` — descending sort

The list of available fields for sorting can be obtained using the [`crm.item.fields`](./crm-item-fields.md) method
||
|| **start**
[`integer`][1] | This parameter is used for pagination control.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number
||
|#

## Code Examples

**Get a list of leads where:**
1. First name or last name is not empty
2. They are in the status "In Progress" or "Unprocessed".
3. They came from sources "Advertising" or "Website".
4. They are assigned to managers with IDs 1 or 6.
5. They have a deal amount between 5000 and 20000.
6. The calculation mode for the amount is manual.

**Set the following sort order for this selection:**
* First name and last name in ascending order

**For clarity, we will select only the fields we need:**
* Identifier `id`
* Title `title`
* First name `name`
* Last name `lastName`
* Stage ID `stageId`
* Source ID `sourceId`
* Responsible ID `assignedById`
* Amount `opportunity`
* Amount calculation mode `isManualOpportunity`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"select":["id","title","lastName","name","stageId","sourceId","assignedById","opportunity","isManualOpportunity"],"filter":{"0":{"logic":"OR","0":{"!=name":""},"1":{"!=lastName":""}},"@stageId":["NEW","IN_PROCESS"],"@sourceId":["WEB","ADVERTISING"],"@assignedById":[1,6],">=opportunity":5000,"<=opportunity":20000,"isManualOpportunity":"Y"},"order":{"lastName":"ASC","name":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"select":["id","title","lastName","name","stageId","sourceId","assignedById","opportunity","isManualOpportunity"],"filter":{"0":{"logic":"OR","0":{"!=name":""},"1":{"!=lastName":""}},"@stageId":["NEW","IN_PROCESS"],"@sourceId":["WEB","ADVERTISING"],"@assignedById":[1,6],">=opportunity":5000,"<=opportunity":20000,"isManualOpportunity":"Y"},"order":{"lastName":"ASC","name":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.list
    ```

- JS

    ```js
        BX24.callMethod(
            'crm.item.list',
            {
                entityTypeId: 1,
                select: [
                    "id", 
                    "title",
                    "lastName",
                    "name",
                    "stageId", 
                    "sourceId", 
                    "assignedById", 
                    "opportunity", 
                    "isManualOpportunity",
                ],
                filter: {
                    "0": {
                        logic: "OR",
                        "0": {
                            "!=name": "",
                        },
                        "1": {
                            "!=lastName": "",
                        },
                    },
                    "@stageId": ["NEW", "IN_PROCESS"],
                    "@sourceId": ['WEB', "ADVERTISING"],
                    "@assignedById": [1, 6],
                    ">=opportunity": 5000,
                    "<=opportunity": 20000,
                    "isManualOpportunity": "Y",
                },
                order: {
                    lastName: 'ASC',
                    name: 'ASC',
                },
            },
            (result) => {
                if (result.error())
                {
                    console.error(result.error());

                    return;
                }

                console.info(result.data());
            },
        );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.list',
        [
            'entityTypeId' => 1,
            'select' => [
                "id",
                "title",
                "lastName",
                "name",
                "stageId",
                "sourceId",
                "assignedById",
                "opportunity",
                "isManualOpportunity",
            ],
            'filter' => [
                "0" => [
                    "logic" => "OR",
                    "0" => [
                        "!=name" => "",
                    ],
                    "1" => [
                        "!=lastName" => "",
                    ],
                ],
                "@stageId" => ["NEW", "IN_PROCESS"],
                "@sourceId" => ['WEB', "ADVERTISING"],
                "@assignedById" => [1, 6],
                ">=opportunity" => 5000,
                "<=opportunity" => 20000,
                "isManualOpportunity" => "Y",
            ],
            'order' => [
                'lastName' => 'ASC',
                'name' => 'ASC',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)
  
    ```php        
    try {
        $entityTypeId = 1; // Replace with actual entity type ID
        $order = []; // Replace with actual order array
        $filter = []; // Replace with actual filter array
        $select = []; // Replace with actual select array
        $startItem = 0; // Optional, can be adjusted as needed
        $itemsResult = $serviceBuilder
            ->getCRMScope()
            ->item()
            ->list($entityTypeId, $order, $filter, $select, $startItem);
        foreach ($itemsResult->getItems() as $item) {
            print("ID: " . $item->id . PHP_EOL);
            print("XML ID: " . $item->xmlId . PHP_EOL);
            print("Title: " . $item->title . PHP_EOL);
            print("Created By: " . $item->createdBy . PHP_EOL);
            print("Updated By: " . $item->updatedBy . PHP_EOL);
            print("Created Time: " . $item->createdTime->format(DATE_ATOM) . PHP_EOL);
            print("Updated Time: " . $item->updatedTime->format(DATE_ATOM) . PHP_EOL);
            // Add more fields as necessary
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage() . PHP_EOL);
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 253,
                "assignedById": 6,
                "stageId": "NEW",
                "opportunity": 19000,
                "sourceId": "WEB",
                "title": "Lead #253",
                "name": "Admin",
                "lastName": null,
                "isManualOpportunity": "Y"
            },
            {
                "id": 255,
                "assignedById": 1,
                "stageId": "NEW",
                "opportunity": 19600,
                "sourceId": "WEB",
                "title": "Lead #255",
                "name": "John",
                "lastName": "Doe",
                "isManualOpportunity": "Y"
            },
            {
                "id": 252,
                "assignedById": 1,
                "stageId": "NEW",
                "opportunity": 12000,
                "sourceId": "ADVERTISING",
                "title": "Lead #252",
                "name": "John",
                "lastName": "Smith",
                "isManualOpportunity": "Y"
            },
            {
                "id": 254,
                "assignedById": 6,
                "stageId": "IN_PROCESS",
                "opportunity": 19000,
                "sourceId": "ADVERTISING",
                "title": "Lead #254",
                "name": "Cat",
                "lastName": "Smith",
                "isManualOpportunity": "Y"
            }
        ]
    },
    "total": 4,
    "time": {
        "start": 1721724354.214286,
        "finish": 1721724354.805263,
        "duration": 0.5909769535064697,
        "processing": 0.24513697624206543,
        "date_start": "2024-07-23T10:45:54+02:00",
        "date_finish": "2024-07-23T10:45:54+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response. Contains a single key `items` ||
|| **items**
[`item[]`](./crm-item-add.md#item) | An array containing information about the found elements.

Fields of a single [`item`](./crm-item-add.md#item) are configured by the `select` parameter ||
|| **total**
[`integer`][1] | Total number of found elements ||
|| **next**
[`integer`][1] | Contains the value to be passed in the next request in the `start` parameter to get the next batch of data.

The `next` parameter appears in the response if the number of elements matching your request exceeds `50`. ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "INVALID_ARG_VALUE",
    "error_description": "Invalid filter: field 'FIELD' is not allowed in filter"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code**                          | **Description**                                             | **Value**                                          ||
|| `403`      | `allowed_only_intranet_user`     | Action is allowed only for intranet users                 | User is not an intranet user                      ||
|| `400`      | `NOT_FOUND`                      | Smart process not found                                    | Occurs when an invalid `entityTypeId` is passed   ||
|| `400`      | `INVALID_ARG_VALUE`              | Invalid filter: field '`field`' is not allowed in filter | The field `field` passed in `filter` is not available for filtering ||
|| `400`      | `INVALID_ARG_VALUE`              | Invalid filter: field '`field`' has invalid value        | The value passed for the field `field` in `filter` is incorrect ||
|| `400`      | `INVALID_ARG_VALUE`              | Invalid order: field '`field`' is not allowed in order   | The field `field` passed in `order` is not available for sorting ||
|| `400`      | `INVALID_ARG_VALUE`              | Invalid order: allowed sort directions are `ASC, DESC`. But got '`orderValue`' for field '`field`' | The `orderValue` passed for the field `field` in the `order` parameter is incorrect ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](crm-item-add.md)
- [{#T}](crm-item-update.md)
- [{#T}](crm-item-get.md)
- [{#T}](crm-item-delete.md)
- [{#T}](crm-item-fields.md)

[1]: ../data-types.md