# Get a list of available logos crm.timeline.logo.list

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method retrieves a list of available logos for timeline log entries.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.logo.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.logo.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.timeline.logo.list',
        {},
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.timeline.logo.list', {}, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('crm.timeline.logo.list', {}, 0)
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
                'crm.timeline.logo.list',
                []
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
        echo 'Error fetching timeline logos: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.logo.list", 
        {},
        result => {
            if (result.error())
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
        'crm.timeline.logo.list',
        []
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
        "logos": [
            {
                "code": "call",
                "isSystem": true,
                "fileUri": ""
            },
            {
                "code": "arrow-down",
                "isSystem": true,
                "fileUri": ""
            },
            {
                "code": "info",
                "isSystem": false,
                "fileUri": "/upload/crm/286/a1k092hygdrzpiz6a01enuymqoo5qzym/ou0akdwnbxalzk9hgfme39nbvtozblew"
            }
        ]
    },
    "total": 3,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
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
[`object`](../../../../data-types.md) | Root element of the response.

The `result` field contains an array of `logos`, each entry of which includes an associative array of logo fields [logo](./crm-timeline-logo-add.md#logo) ||
|| **total**
[`integer`](../../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Could not find value"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Other errors (e.g., fatal) ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Exploring 

- [{#T}](./crm-timeline-logo-add.md)
- [{#T}](./crm-timeline-logo-get.md)
- [{#T}](./crm-timeline-logo-delete.md)