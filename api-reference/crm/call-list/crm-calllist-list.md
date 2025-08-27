# Get the list of call lists crm.calllist.list

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with read access permission for CRM elements

The method `crm.calllist.list` returns a list of call list activities.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../../data-types.md) | Array of fields to retrieve. By default, all fields are returned.
List of fields to retrieve:
- `ID`,
- `DATE_CREATE`,
- `CREATED_BY_ID`,
- `WEBFORM_ID`,
- `ENTITY_TYPE_ID` ||
|| **FILTER**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — name of the field by which the call list will be filtered
- `value_n` — filter value

List of fields for filtering:
- `ID`,
- `CREATED_BY_ID`,
- `WEBFORM_ID`,
- `ENTITY_TYPE_ID`.
  
An additional prefix can be assigned to the field to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to,
- `>` — greater than,
- `<=` — less than or equal to,
- `<` — less than,
- `@` — IN, an array is passed as the value,
- `!@`— NOT IN, an array is passed as the value,
- `=` — equal, exact match, used by default,
- `!=` - not equal,
- `!` — not equal ||
|| **ORDER**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — name of the field by which the call list will be sorted
- `value_n` — value of type `string`, equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"SELECT":["ID","CREATED_BY_ID"],"FILTER":{"ENTITY_TYPE_ID":3},"ORDER":{"ID":"DESC"}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.calllist.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"SELECT":["ID","CREATED_BY_ID"],"FILTER":{"ENTITY_TYPE_ID":3},"ORDER":{"ID":"DESC"},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.calllist.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.calllist.list',
        {
          SELECT: ["ID", "CREATED_BY_ID"],
          FILTER: { "ENTITY_TYPE_ID": 3 },
          ORDER: { "ID": "DESC" }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative retrieval using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.calllist.list', { SELECT: ["ID", "CREATED_BY_ID"], FILTER: { "ENTITY_TYPE_ID": 3 }, ORDER: { "ID": "DESC" } }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. Suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('crm.calllist.list', { SELECT: ["ID", "CREATED_BY_ID"], FILTER: { "ENTITY_TYPE_ID": 3 }, ORDER: { "ID": "DESC" } }, 0)
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
                'crm.calllist.list',
                [
                    'SELECT' => ['ID', 'CREATED_BY_ID'],
                    'FILTER' => ['ENTITY_TYPE_ID' => 3],
                    'ORDER'  => ['ID' => 'DESC'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching call list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.calllist.list",
        {
            SELECT: ["ID", "CREATED_BY_ID"],
            FILTER: { "ENTITY_TYPE_ID": 3 },
            ORDER: { "ID": "DESC" }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.calllist.list',
        [
            'SELECT' => ['ID', 'CREATED_BY_ID'],
            'FILTER' => ['ENTITY_TYPE_ID' => 3],
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
            "ID": "13",
            "CREATED_BY_ID": "29"
        },
        {
            "ID": "9",
            "CREATED_BY_ID": "1"
        },
        {
            "ID": "3",
            "CREATED_BY_ID": "131"
        },
        {
            "ID": "1",
            "CREATED_BY_ID": "131"
        }
    ],
    "time": {
        "start": 1752475786.965766,
        "finish": 1752475787.035008,
        "duration": 0.06924200057983398,
        "processing": 0.016666889190673828,
        "date_start": "2025-07-14T09:49:46+02:00",
        "date_finish": "2025-07-14T09:49:47+02:00",
        "operating_reset_at": 1752476387,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Root element of the response. Contains an array of objects with information about call lists. 

The structure of fields depends on the `select` parameter ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Invalid parameters.",
    "error_description": "Invalid parameters were provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `Invalid parameters` | Invalid parameters were provided ||
|| `100` | `Unknown field definition "TITLE"` | Unknown parameter "Field name" ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-calllist-add.md)
- [{#T}](./crm-calllist-get.md)
- [{#T}](./crm-calllist-items-get.md)
- [{#T}](./crm-calllist-statuslist.md)
- [{#T}](./crm-calllist-update.md)