# Get the List of Application Actions bizproc.activity.list

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of actions defined by the application.

It only works in the context of the [application](../../app-installation/index.md).

No parameters required.

## Code Examples

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.activity.list
    ```

- JS

    ```js
    BX24.callMethod(
        'bizproc.activity.list',
        {},
        function(result) {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data().join(', '));
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.activity.list',
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
        "md5_action",
        "doc_action"
    ],
    "time": {
        "start": 1738151724.710429,
        "finish": 1738151724.7319269,
        "duration": 0.021497964859008789,
        "processing": 0.0011229515075683594,
        "date_start": "2025-01-29T14:55:24+02:00",
        "date_finish": "2025-01-29T14:55:24+02:00",
        "operating_reset_at": 1738152324,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of application action identifiers ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ACCESS_DENIED` | Application context required | Application context is required ||
|| `ACCESS_DENIED` | Access denied! | Method was not executed by an administrator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-activity-add.md)
- [{#T}](./bizproc-activity-update.md)
- [{#T}](./bizproc-activity-delete.md)
- [{#T}](./bizproc-activity-log.md)