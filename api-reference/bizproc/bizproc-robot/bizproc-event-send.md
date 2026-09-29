# Return Parameters to Action or Automation Rule bizproc.event.send

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `bizproc.event.send` method returns the output parameters to the Automation rule or action that were specified during the registration or update of the Automation rule or action.

The call completes a step that is waiting for a response, even if `RETURN_VALUES` is not passed. To write an intermediate message to the log without completing the step, use the [bizproc.activity.log](../bizproc-activity/bizproc-activity-log.md) method.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description**||
|| **EVENT_TOKEN***
[`string`](../../data-types.md) | The run token of the Automation rule or action. Bitrix24 passes it to the application handler in the `event_token` field.

The process accepts the result only if the step with this token is still waiting for a response. Waiting is enabled by the `USE_SUBSCRIPTION: 'Y'` parameter when the Automation rule or action is registered. If the parameter is not set, the step does not wait for a response by default, and waiting can be enabled in the step settings ||
|| **RETURN_VALUES**
[`object`](../../data-types.md) | Return values of the Automation rule or action. The keys are the parameter codes from `RETURN_PROPERTIES` that were set with the methods:
- [bizproc.robot.add](./bizproc-robot-add.md), [bizproc.robot.update](./bizproc-robot-update.md)
- [bizproc.activity.add](../bizproc-activity/bizproc-activity-add.md), [bizproc.activity.update](../bizproc-activity/bizproc-activity-update.md)

Keys are case-insensitive. Bitrix24 converts the value to the `Type` of this parameter. Bitrix24 does not retain keys that are not in `RETURN_PROPERTIES` ||
|| **LOG_MESSAGE**
[`string`](../../data-types.md) | Text for the business process log.

If this parameter is not passed, the log receives the standard entry "Received application response".

Event logging must be enabled in the business process template
||
|#

{% note warning "" %}

The method checks only the signature of `EVENT_TOKEN`: with an invalid token, it returns the `ACCESS_DENIED` error. The method responds before Bitrix24 passes the values to the process. If the step has already completed, timed out, or is not waiting for a response, the method still returns `true`, and the process does not change.

{% endnote %}

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"event_token":"55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90","return_values":{"outputString":"846c55d14f552180874a628d2615e285"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.event.send
    ```

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
    	const response = await $b24.actions.v2.call.make({
    		method: 'bizproc.event.send',
    		params: {
    			event_token: '55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90',
    			return_values: {
    				outputString: '846c55d14f552180874a628d2615e285'
    			}
    		}
    	});

    	if (!response.isSuccess)
    		console.error(response.getErrorMessages().join('; '));
    	else
    		console.log('Success:', response.getData().result);
    }
    catch (error)
    {
    	console.error('Error:', error);
    }
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.bizproc.event.send(
            event_token="55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90",
            return_values={
                "outputString": "846c55d14f552180874a628d2615e285",
            },
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

        echo 'Success: ' . var_export($result[0], true);

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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "bizproc.event.send", b24.Params{
    	"EVENT_TOKEN": "55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90",
    	"RETURN_VALUES": b24.Params{
    		"outputString": "846c55d14f552180874a628d2615e285",
    	},
    })
    if err != nil {
    	return fmt.Errorf("bizproc.event.send: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
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
[`boolean`](../../data-types.md) | `true` if Bitrix24 accepted the request. This does not confirm that the process applied the values ||
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
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! | `EVENT_TOKEN` is not passed or its signature is invalid ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bizproc-robot-add.md)
- [{#T}](../bizproc-activity/index.md)
- [{#T}](../bizproc-activity/bizproc-activity-log.md)
