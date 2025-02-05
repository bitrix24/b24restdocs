# Delete Registered Automation Rule bizproc.robot.delete

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes an Automation rule registered by the application.

It only works in the context of the [application](../../app-installation/index.md).

When an application is deleted or updated, the associated Automation rules are removed from the list of Automation rules. If the Automation rule is in use, it is blocked and can only be deleted from the workflow. Upon reinstallation of the application, the Automation rule becomes available again.

## Method Parameters

#|
|| **Name**
`type` | **Description**||
|| **CODE***
[`string`](../../data-types.md) | Symbolic identifier of the application Automation rule ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"test_robot","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.robot.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'bizproc.robot.delete',
        {
            'CODE': 'test_robot'
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
        'bizproc.robot.delete',
        [
            'CODE' => 'test_robot'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)

    ```php
    try {
        $robotCode = 'your_robot_code_here'; // Replace with the actual Automation rule code
        $result = $serviceBuilder
            ->getBizProcScope()
            ->robot()
            ->delete($robotCode);

        if ($result->isSuccess()) {
            print("Automation rule deleted successfully.");
        } else {
            print("Failed to delete Automation rule.");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
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
[`boolean`](../../data-types.md) | Returns `true` if the Automation rule was successfully deleted ||
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
|| `ACCESS_DENIED` | Access denied! | Method was not executed by an administrator ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty activity code! | Automation rule code not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity code! | Incorrect Automation rule code ||
|| `ERROR_ACTIVITY_NOT_FOUND` | Activity or Automation rule not found! | Automation rule not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bizproc-robot-add.md)
- [{#T}](./bizproc-robot-update.md)
- [{#T}](./bizproc-robot-list.md)
- [{#T}](./bizproc-event-send.md)