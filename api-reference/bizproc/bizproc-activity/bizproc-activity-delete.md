# Delete workflow action bizproc.activity.delete

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method removes an action for workflows that was added by the application.

It only works in the context of the [application](../../app-installation/index.md).

When an application is deleted or updated, the associated actions are removed from the list of actions in the workflow designer. If the action is used in a workflow, it is blocked and can only be removed from the scheme. Upon reinstallation of the application, the action becomes available again.

## Method Parameters

#|
|| **Name**
`type` | **Description**||
|| **CODE***
[`string`](../../data-types.md) | Symbolic identifier of the application action ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"code":"md5_action","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.activity.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'bizproc.activity.delete',
        {
            code: 'md5_action'
        },
        function(result) {
            if(result.error())
                alert('Error: ' + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.activity.delete',
        [
            'code' => 'md5_action'
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
    "result": true,
    "time": {
        "start": 1738150149.8462,
        "finish": 1738150149.8894911,
        "duration": 0.043291091918945312,
        "processing": 0.0053689479827880859,
        "date_start": "2025-01-29T14:29:09+01:00",
        "date_finish": "2025-01-29T14:29:09+01:00",
        "operating_reset_at": 1738150749,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the action was successfully deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_ACTIVITY_NOT_FOUND",
    "error_description": "Activity or Automation rule not found!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ACCESS_DENIED` | Application context required | Application context is required ||
|| `ACCESS_DENIED` | Access denied! | The method was executed by a non-administrator ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty activity code! | No action code specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity code! | Invalid action code ||
|| `ERROR_ACTIVITY_NOT_FOUND` | Activity or Automation rule not found! | Action or Automation rule not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-activity-add.md)
- [{#T}](./bizproc-activity-update.md)
- [{#T}](./bizproc-activity-list.md)
- [{#T}](./bizproc-activity-log.md)