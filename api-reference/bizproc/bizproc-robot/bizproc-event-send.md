# Return Parameters to Action or Automation Rule bizproc.event.send

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns the output parameters to the Automation rule or action that were specified during the registration or update of the Automation rule or action.

## Method Parameters

#|
|| **Name**
`type` | **Description**||
|| **EVENT_TOKEN**
[`string`](../../data-types.md) | A special token that is sent to the application handler when the action or Automation rule is executed. The value of this token is received by the handler in the input data array.

An event can be sent if the Automation rule or action is registered with `'USE_SUBSCRIPTION': 'Y'` ||
|| **RETURN_VALUES**
[`object`](../../data-types.md) | An array of returned values from the action or Automation rule. It specifies the values of properties that were registered as additional results `RETURN_PROPERTIES` by the methods:
- [bizproc.robot.add](./bizproc-robot-add.md), [bizproc.robot.update](./bizproc-robot-update.md)
- [bizproc.activity.add](../bizproc-activity/bizproc-activity-add.md), [bizproc.activity.update](../bizproc-activity/bizproc-activity-update.md) ||
|| **LOG_MESSAGE**
[`string`](../../data-types.md) | Text for the business process log.

By default, it has the value "Received response from the application."

Event logging must be enabled in the business process template
||
|#

{% note info "" %}

`EVENT_TOKEN` must be valid and current. If the token is invalid or expired, the method will return an access error `ACCESS_DENIED`

{% endnote %}

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"event_token":"55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90","return_values":{"outputString":"846c55d14f552180874a628d2615e285"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.event.send
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'bizproc.event.send',
    		{
    			event_token: '55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90',
    			return_values: {
    				outputString: '846c55d14f552180874a628d2615e285'
    			}
    		}
    	);
    	
    	if(response.error())
    		alert("Error: " + response.error());
    	else
    		alert("Success: " + response.getData().result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'bizproc.event.send',
                [
                    'event_token' => '55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90',
                    'return_values' => [
                        'outputString' => '846c55d14f552180874a628d2615e285'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sending bizproc event: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'bizproc.event.send',
        {
            event_token: '55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90',
            return_values: {
                outputString: '846c55d14f552180874a628d2615e285'
            }
        },
        function(result) {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.event.send',
        [
            'event_token' => '55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90',
            'return_values' => [
                'outputString' => '846c55d14f552180874a628d2615e285'
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
    "result": true,
    "time": {
        "start": 1738152544.203554,
        "finish": 1738152544.248411,
        "duration": 0.044857025146484375,
        "processing": 0.0039920806884765625,
        "date_start": "2025-01-29T15:09:04+01:00",
        "date_finish": "2025-01-29T15:09:04+01:00",
        "operating_reset_at": 1738153144,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the values were successfully sent to the process ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Message** | **Description** ||
|| `ACCESS_DENIED` | Access denied! | Invalid or expired `EVENT_TOKEN` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)