# Get a List of SMS Providers or Message Providers messageservice.sender.list

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `messageservice.sender.list` returns a list of provider codes registered with the current application.

This method works only in the context of the [application](../../settings/app-installation/index.md).

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/messageservice.sender.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small datasets (< 1000 items) due to high memory load.
    
    try {
      const response = await $b24.callListMethod(
        'messageservice.sender.list',
        {},
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in chunks using an iterator. Use for large datasets for efficient memory consumption.
    
    try {
      const generator = $b24.fetchListMethod('messageservice.sender.list', {}, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('messageservice.sender.list', {}, 0)
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
                'messageservice.sender.list',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing senders: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'messageservice.sender.list',
        {},
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'messageservice.sender.list',
        []
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
        "provider1",
        "provider2"
    ],
    "time": {
        "start": 1742895600,
        "finish": 1742895600.845505,
        "duration": 0.845505952835083,
        "processing": 0,
        "date_start": "2025-03-25T10:00:00+02:00",
        "date_finish": "2025-03-25T10:00:00+02:00",
        "operating_reset_at": 1742896200,
        "operating": 0
    }
}
```
### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | An array of provider codes registered with the current application.

An empty array indicates that the current application has no registered providers ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | `Application context required` | Method called outside the application context ||
|| `ACCESS_DENIED` | `Access denied!` | Method was not executed by an administrator ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./messageservice-sender-add.md)
- [{#T}](./messageservice-sender-update.md)
- [{#T}](./messageservice-sender-list.md)
- [{#T}](./messageservice-sender-delete.md)
- [{#T}](./messageservice-message-status-update.md)