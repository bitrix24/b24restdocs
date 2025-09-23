# Get a list of VAT rates by filter crm.vat.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method development has been halted" %}

The method `crm.vat.list` continues to function, but there is a more current equivalent [catalog.vat.list](../../../catalog/vat/catalog-vat-list.md).

{% endnote %}

The method `crm.vat.list` returns a list of VAT rates based on a filter. 
It is an implementation of the [list method](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md) for VAT rates.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **order** 
[`object`](../../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — the name of the field by which the selection of rates will be sorted
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

The list of available fields for sorting can be found using the method [crm.vat.fields](./crm-vat-fields.md) ||
|| **filter** 
[`object`](../../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — the name of the field by which the selection of elements will be filtered
- `value_n` — the filter value

The list of available fields for filtering can be found using the method [crm.vat.fields](./crm-vat-fields.md)
||
|| **select** 
[`array`](../../../data-types.md) | Array of returned fields. If not specified, all fields are returned ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"order":{"ID":"ASC"},"filter":{"ACTIVE":"Y"},"select":["ID","NAME","RATE"]}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.vat.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"ACTIVE":"Y"},"select":["ID","NAME","RATE"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.vat.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.vat.list',
        {
          order: { ID: "ASC" },
          filter: { ACTIVE: "Y" },
          select: ["ID", "NAME", "RATE"]
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.vat.list', { order: { ID: "ASC" }, filter: { ACTIVE: "Y" }, select: ["ID", "NAME", "RATE"] }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod with large volumes of data.
    
    try {
      const response = await $b24.callMethod('crm.vat.list', { order: { ID: "ASC" }, filter: { ACTIVE: "Y" }, select: ["ID", "NAME", "RATE"] }, 0)
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
                'crm.vat.list',
                [
                    'order'  => ['ID' => 'ASC'],
                    'filter' => ['ACTIVE' => 'Y'],
                    'select' => ['ID', 'NAME', 'RATE'],
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
        echo 'Error fetching VAT list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.vat.list",
        {
            order: { ID: "ASC" },
            filter: { ACTIVE: "Y" },
            select: ["ID", "NAME", "RATE"]
        },
        function(result) {
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
        'crm.vat.list',
        [
            'order' => [ 'ID' => 'ASC' ],
            'filter' => [ 'ACTIVE' => 'Y' ],
            'select' => [ 'ID', 'NAME', 'RATE' ]
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
            "NAME": "No VAT",
            "RATE": null
        },
        {
            "ID": "3",
            "NAME": "VAT 20%",
            "RATE": "20.00"
        },
        {
            "ID": "7",
            "NAME": "12",
            "RATE": "12.00"
        }
    ],
    "total": 3,
    "time": {
        "start": 1752044697.589623,
        "finish": 1752044697.66439,
        "duration": 0.0747671127319336,
        "processing": 0.00588679313659668,
        "date_start": "2025-07-09T10:04:57+03:00",
        "date_finish": "2025-07-09T10:04:57+03:00",
        "operating_reset_at": 1752045297,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The root element of the response. Contains an array of objects with information about VAT rate fields. 

The structure of the fields may change due to the `select` parameter ||
|| **total**
[`integer`](../../../data-types.md) | The total number of found items ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Inadmissible fields for selection",
    "error_description": "Invalid fields for selection were provided."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `The Commercial Catalog module is not installed.` | The catalog module is not installed ||
|| `400`     | `Access denied.` | No permission to perform the operation ||
|| `400`     | `"Inadmissible fields for selection.` | Invalid fields for selection were provided ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-vat-fields.md)
- [{#T}](./crm-vat-get.md)
- [{#T}](./crm-vat-add.md)
- [{#T}](./crm-vat-update.md)
- [{#T}](./crm-vat-delete.md) 