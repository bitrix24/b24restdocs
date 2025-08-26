# Get a list of matches for a natural or legal person sale.businessValuePersonDomain.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.businessValuePersonDomain.list` retrieves a list of matches for a natural or legal person.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | The array contains a list of fields to select.

If not provided or an empty array is passed, all available fields for matching a natural or legal person will be selected.

Possible values for array elements:
- `personTypeId`
- `domain` ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected matches for a natural or legal person in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

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
[`object`](../../data-types.md) | An object for sorting the selected matches for a natural or legal person in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field`:
- `personTypeId`
- `domain` 

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|| **start**
[`integer`](../../data-types.md) | The parameter is used to manage pagination.

The page size of results is always static: 50 records.

To select the second page of results, the value `50` must be passed. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

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
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'sale.businessValuePersonDomain.list',
        {
          select: ["personTypeId"],
          filter: {"=domain": "I"},
          order: {"personTypeId": "DESC"}
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative fetching using a generator, allowing data to be processed in chunks and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('sale.businessValuePersonDomain.list', {
        select: ["personTypeId"],
        filter: {"=domain": "I"},
        order: {"personTypeId": "DESC"}
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('sale.businessValuePersonDomain.list', {
        select: ["personTypeId"],
        filter: {"=domain": "I"},
        order: {"personTypeId": "DESC"}
      }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.businessValuePersonDomain.list',
                [
                    'select' => ["personTypeId"],
                    'filter' => ["=domain" => "I"],
                    'order' => ["personTypeId" => "DESC"]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

HTTP status: **200**

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
[`sale_business_value_person_domain[]`](../data-types.md) | An array of objects with information about the selected matches ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | No access to read ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-business-value-person-domain-add.md)
- [{#T}](./sale-business-value-person-domain-delete-by-filter.md)
- [{#T}](./sale-business-value-person-domain-get-fields.md)