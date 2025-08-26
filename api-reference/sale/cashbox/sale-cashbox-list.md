# Get a list of configured cash registers sale.cashbox.list

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (access permission "Allow to modify settings")

The method returns a list of configured cash registers.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../../data-types.md) | An array with the list of fields to select (see fields of the [sale_cashbox](../data-types.md#sale_cashbox) object) ||
|| **FILTER**
[`object`](../../data-types.md) | An object for filtering selected cash registers in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_cashbox](../data-types.md#sale_cashbox) object.

When specifying multiple fields, the `AND` logic is used.

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **ORDER**
[`object`](../../data-types.md) | An object for sorting selected records in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_cashbox](../data-types.md#sale_cashbox) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","NAME"],"FILTER":{"=NAME":"My Rest-Cash Register",">ID":9},"ORDER":{"ID":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.cashbox.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","NAME"],"FILTER":{"=NAME":"My Rest-Cash Register",">ID":9},"ORDER":{"ID":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.cashbox.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'sale.cashbox.list',
        {
          "SELECT": ["ID", "NAME"],
          "FILTER": {"=NAME": "My Rest-Cash Register", ">ID": 9},
          "ORDER": {"ID": "DESC"}
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('sale.cashbox.list', { "SELECT": ["ID", "NAME"], "FILTER": {"=NAME": "My Rest-Cash Register", ">ID": 9}, "ORDER": {"ID": "DESC"} }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('sale.cashbox.list', { "SELECT": ["ID", "NAME"], "FILTER": {"=NAME": "My Rest-Cash Register", ">ID": 9}, "ORDER": {"ID": "DESC"} }, 0)
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
                'sale.cashbox.list',
                [
                    'SELECT' => ['ID', 'NAME'],
                    'FILTER' => ['=NAME' => 'My Rest-Cash Register', '>ID' => 9],
                    'ORDER'  => ['ID' => 'DESC'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling sale.cashbox.list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod( 
        "sale.cashbox.list", 
        { 
            "SELECT": ["ID", "NAME"], 
            "FILTER": {"=NAME": "My Rest-Cash Register", ">ID": 9},
            "ORDER": {"ID": "DESC"},
        }, 
        function(result) 
        { 
            if(result.error()) 
                console.error(result.error()); 
            else 
                console.dir(result.data()); 
        } 
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.cashbox.list',
        [
            'SELECT' => ['ID', 'NAME'],
            'FILTER' => ['=NAME' => 'My Rest-Cash Register', '>ID' => 9],
            'ORDER' => ['ID' => 'DESC']
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
            "NAME": "REST Cash Register",
            "OFD": "bx_firstofd",
            "EMAIL": "admin@example.com",
            "DATE_CREATE": {},
            "DATE_LAST_CHECK": null,
            "NUMBER_KKM": "",
            "KKM_ID": null,
            "ACTIVE": "Y",
            "SORT": "100",
            "USE_OFFLINE": "N",
            "ENABLED": "Y"
        }
    ],
    "time": {
        "start": 1712135957.057659,
        "finish": 1712135957.407821,
        "duration": 0.3501620292663574,
        "processing": 0.011919021606445312,
        "date_start": "2024-04-03T11:19:17+02:00",
        "date_finish": "2024-04-03T11:19:17+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_cashbox[]`](../data-types.md#sale_cashbox) | An array of cash registers registered in the system  ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "BAD_FIELD not allowed for select"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient rights to obtain the list of cash registers | 403 ||
|| ‘’ (empty error code) | An incorrect field has been specified for selection, filtering, or sorting. More detailed information about the error can be found in `error_description` | 400 ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-cashbox-handler-add.md)
- [{#T}](./sale-cashbox-handler-update.md)
- [{#T}](./sale-cashbox-handler-list.md)
- [{#T}](./sale-cashbox-handler-delete.md)
- [{#T}](./sale-cashbox-add.md)
- [{#T}](./sale-cashbox-update.md)
- [{#T}](./sale-cashbox-delete.md)
- [{#T}](./sale-cashbox-check-apply.md)