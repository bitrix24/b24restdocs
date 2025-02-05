# Get a List of Registered Application Automation Rules bizproc.robot.list

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns a list of automation rules registered by the application.

It only works in the context of the [application](../../app-installation/index.md).

No parameters required.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.robot.list
    ```

- JS

    ```js
    BX24.callMethod(
        'bizproc.robot.list',
        {},
        function(result)
        {
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
        'bizproc.robot.list',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)

    ```php
    try {
        $result = $serviceBuilder
            ->getBizProcScope()
            ->robot()
            ->list();

        foreach ($result->getRobots() as $robot) {
            print($robot->code);
            print($robot->name);
            print($robot->handlerUrl);
            print($robot->authUserId);
            print($robot->isUseSubscription ? 'Yes' : 'No');
            print($robot->isUsePlacement ? 'Yes' : 'No');
            if ($robot->createdDate instanceof DateTime) {
                print($robot->createdDate->format(DateTime::ATOM));
            }
        }
    } catch (Throwable $e) {
        // Handle the exception
        print('Error: ' . $e->getMessage());
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        "test_robot",
        "sms_robot"
    ],
    "time": {
        "start": 1738151724.710429,
        "finish": 1738151724.7319269,
        "duration": 0.021497964859008789,
        "processing": 0.0011229515075683594,
        "date_start": "2025-01-29T14:55:24+01:00",
        "date_finish": "2025-01-29T14:55:24+01:00",
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
[`array`](../../data-types.md) | List of application automation rule identifiers ||
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
- [{#T}](./bizproc-robot-add.md)
- [{#T}](./bizproc-robot-update.md)
- [{#T}](./bizproc-robot-delete.md)
- [{#T}](./bizproc-event-send.md)