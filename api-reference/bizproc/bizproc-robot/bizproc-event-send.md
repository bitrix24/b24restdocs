# Return Parameters to the Action or Automation Rule bizproc.event.send

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns the output parameters of the action specified in the action description.

## Method Parameters

#|
|| **Name**
`type` | **Description**||
|| **EVENT_TOKEN** | A special token that is sent to the application handler when the action or automation rule is executed. The value of this token is received by the handler of the automation rule or business process action in the array of input data passed.

An event can be sent if the application action is subscribed with `'USE_SUBSCRIPTION': 'Y'` to the execution of the business process or automation rules. ||
|| **RETURN_VALUES** | An array of returned values from the action or automation rule. It specifies the values of properties that were registered as additional results `RETURN_PROPERTIES` by the methods:
- [bizproc.robot.add](./bizproc-robot-add.md), [bizproc.robot.update](./bizproc-robot-update.md)
- [bizproc.activity.add](../bizproc-activity/bizproc-activity-add.md), [bizproc.activity.update](../bizproc-activity/bizproc-activity-update.md) ||
|| **LOG_MESSAGE** | Text for the business process log.

By default, it has the value "Received response from the application."

Logging of events must be [enabled in the template](https://helpdesk.bitrix24.com/open/22095380/) of the business process
||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

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

- PHP

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

## Error Handling

### Possible Error Codes

The method may return an error code and text from the business process or automation rule.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)