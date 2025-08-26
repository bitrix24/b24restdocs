# Get the list of bindings for a record in the timeline crm.timeline.bindings.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method retrieves a list of bindings for a record in the timeline.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../../data-types.md) | Object for filtering selected records.

The `OWNER_ID` field is required; other fields are not necessary. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"OWNER_ID":999}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.bindings.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"OWNER_ID":999},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.bindings.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.timeline.bindings.list',
        {
          filter: {
            "OWNER_ID": 999,
          },
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
      const generator = $b24.fetchListMethod('crm.timeline.bindings.list', {
        filter: {
          "OWNER_ID": 999,
        },
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('crm.timeline.bindings.list', {
        filter: {
          "OWNER_ID": 999,
        },
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
                'crm.timeline.bindings.list',
                [
                    'filter' => [
                        'OWNER_ID' => 999,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            print_r($result->data());
            if ($result->more()) {
                $result->next();
            }
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching timeline bindings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.bindings.list",
        {
            filter: {
                "OWNER_ID": 999,
            },
        }, result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
                if (result.more()) 
                {
                    result.next();
                }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.bindings.list',
        [
            'filter' => [
                'OWNER_ID' => 999,
            ]
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
    "result": [
        {
            "OWNER_ID": "999",
            "ENTITY_ID": "39",
            "ENTITY_TYPE": "deal"
        },
        {
            "OWNER_ID": "999",
            "ENTITY_ID": "92",
            "ENTITY_TYPE": "company"
        },
        {
            "OWNER_ID": "999",
            "ENTITY_ID": "205",
            "ENTITY_TYPE": "lead"
        }
    ],
    "total": 3,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+02:00",
        "date_finish": "2024-05-03T17:19:01+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response containing an array of objects with [information](crm-timeline-bindings-bind.md#parametr-fields) about the found bindings ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "OWNER_ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | OWNER_ID is not defined or invalid | The required parameter `OWNER_ID` was not provided or the provided `OWNER_ID` is invalid. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-timeline-bindings-bind.md)
- [{#T}](./crm-timeline-bindings-unbind.md)
- [{#T}](./crm-timeline-bindings-fields.md)