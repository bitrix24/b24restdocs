# Read Notification List (excluding CONFIRM) im.notify.read.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.read.list` "reads" the list of notifications, excluding notifications of type CONFIRM.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **IDS^*^**
[`unknown`](../../data-types.md) | `[1,2,3]` | Array of notification identifiers | 30 ||
|| **ACTION**
[`unknown`](../../data-types.md) | `'Y'` | Mark as read|unread (`Y`\|`N`) | 30 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'im.notify.read.list',
        {
          IDS: [1, 2, 3],
          ACTION: 'Y'
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('im.notify.read.list', { IDS: [1, 2, 3], ACTION: 'Y' }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('im.notify.read.list', { IDS: [1, 2, 3], ACTION: 'Y' }, 0)
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
                'im.notify.read.list',
                [
                    'IDS'    => [1, 2, 3],
                    'ACTION' => 'Y',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error reading notification list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.notify.read.list',
        {
            IDS: [1,2,3],
            ACTION: 'Y'
        },
        res => {
            if (res.error())
            {
            console.error(result.error().ex);
            }
            else
            {
            console.log(res.data())
            }
        }
    );
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": true
}        
```

## Error Response

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "No IDS param or it is not an array"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **PARAMS_ERROR** | The `IDS` parameter is not provided or it is not an array ||
|#

