# Delete Registered Robot bizproc.robot.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `bizproc.robot.delete` method removes a robot registered by the application.

It only works in the context of the [application](../../../settings/app-installation/index.md).

When an application is deleted or updated, Bitrix24 removes only the robots registered by this application from the list of available robots. Robots of other applications and standard robots are not affected. If a robot is already used in an automation rule, it remains in the rule as unavailable: it can be removed but cannot be configured or executed as an available robot. Here, update means updating the application, not calling the [bizproc.robot.update](./bizproc-robot-update.md) method. Upon reinstalling the application, the robot with the same code becomes available again.

## Method Parameters

#|
|| **Name**
`type` | **Description**||
|| **CODE***
[`string`](../../data-types.md) | Symbolic identifier of the application robot ||
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
    try
    {
    	const response = await $b24.callMethod(
    		'bizproc.robot.delete',
    		{
    			'CODE': 'test_robot'
    		}
    	);
    	
    	const result = response.getData().result;
    	alert("Success: " + result);
    }
    catch( error )
    {
    	alert('Error: ' + error);
    }
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.bizproc.robot.delete(
            code="test_robot",
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $robotCode = 'your_robot_code_here'; // Replace with the actual robot code
        $result = $serviceBuilder
            ->getBizProcScope()
            ->robot()
            ->delete($robotCode);

        if ($result->isSuccess()) {
            print("Robot deleted successfully.");
        } else {
            print("Failed to delete robot.");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

- BX24.js

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

- PHP CRest

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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "bizproc.robot.delete", b24.Params{
    	"CODE": "test_robot",
    })
    if err != nil {
    	return fmt.Errorf("bizproc.robot.delete: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
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
[`boolean`](../../data-types.md) | Returns `true` if the robot was successfully deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_ACTIVITY_NOT_FOUND",
    "error_description": "Activity or Robot not found!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ACCESS_DENIED` | Application context required | Application context is required ||
|| `ACCESS_DENIED` | Access denied! | Method executed by non-administrator ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty activity code! | Robot code not specified ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity code! | Invalid robot code ||
|| `ERROR_ACTIVITY_NOT_FOUND` | Activity or Robot not found! | Robot not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-robot-add.md)
- [{#T}](./bizproc-robot-update.md)
- [{#T}](./bizproc-robot-list.md)
- [{#T}](./bizproc-event-send.md)
