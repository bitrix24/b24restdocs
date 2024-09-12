# Get a List of Correspondence Elements for Individuals and Legal Entities sale.businessValuePersonDomain.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.businessValuePersonDomain.list` retrieves a list of correspondence elements for individuals and legal entities.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | The array contains a list of fields to be selected.

If not provided or an empty array is passed, all available fields of the correspondence elements for individuals and legal entities will be selected.

Possible values for the array elements:
- `personTypeId`
- `domain` ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected correspondence elements for individuals and legal entities in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field`:
- `personTypeId`
- `domain` 

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `=` — equals, exact match (used by default)
- `!=` - not equal ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected correspondence elements for individuals and legal entities in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field`:
- `personTypeId`
- `domain` 

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.

The page size of the results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["personTypeId"],"filter":{"=domain":"I"},"order":{"personTypeId":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.businessValuePersonDomain.list
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["personTypeId"],"filter":{"=domain":"I"},"order":{"personTypeId":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.businessValuePersonDomain.list
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.businessValuePersonDomain.list',
        {
            select: ["personTypeId"],
            filter: {"=domain": "I"},
            order: {"personTypeId": "DESC"}
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
        'sale.businessValuePersonDomain.list',
        [
            'select' => ['personTypeId'],
            'filter' => ['=domain' => 'I'],
            'order' => ['personTypeId' => 'DESC']
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
    "result": {
        "businessValuePersonDomains": [
            {
                "personTypeId": 3
            },
            {
                "personTypeId": 2
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1712326352.63409,
        "finish": 1712326352.8319,
        "duration": 0.197818040847778,
        "processing": 0.00833678245544434,
        "date_start": "2024-04-05T16:12:32+02:00",
        "date_finish": "2024-04-05T16:12:32+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **businessValuePersonDomains**
[`sale_business_value_person_domain[]`](../data-types.md) | An array of objects containing information about the selected correspondence elements for individuals and legal entities ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | No access to read ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-business-value-person-domain-add.md)
- [{#T}](./sale-business-value-person-domain-delete-by-filter.md)
- [{#T}](./sale-business-value-person-domain-get-fields.md)