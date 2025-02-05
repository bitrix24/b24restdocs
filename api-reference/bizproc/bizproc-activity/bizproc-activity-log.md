# Write Information to the Business Process Log bizproc.activity.log

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- It needs to be clarified what the unique identifier is and why it is needed.
- The example is unclear. A description should be provided, explaining how it works, or a link to a tutorial where this method is used in a real task should be added.

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method logs information into the business process log. Event logging must be enabled in the business process template.

{% note tip "User Documentation" %}

- [Workflow test log](https://helpdesk.bitrix24.com/open/22095380/)

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description**||
|| **EVENT_TOKEN***
[`string`](../../data-types.md) | A unique key required to send an event to the business process.

The token is sent to the application action handler when the business process reaches this action.

Logging is possible if the application action is subscribed with `'USE_SUBSCRIPTION': 'Y'` during the execution of the business process. ||
|| **LOG_MESSAGE***
[`string`](../../data-types.md) | Message to be logged ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"event_token":"55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90","log_message":"Please wait for answer!"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.activity.log
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"event_token":"55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90","log_message":"Please wait for answer!","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.activity.log
    ```

- JS

    ```js
    BX24.callMethod(
        'bizproc.activity.log',
        {
            event_token: '55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90',
            log_message: 'Please wait for answer!'
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
        'bizproc.activity.log',
        [
            'event_token' => '55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90',
            'log_message' => 'Please wait for answer!'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)

    ```php
    try {
        $eventToken = 'your_event_token'; // Replace with actual event token
        $message = 'Your log message'; // Replace with actual message

        $result = $serviceBuilder
            ->getBizProcScope()
            ->activity()
            ->log($eventToken, $message);

        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print('Log entry failed.');
        }
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

{% endlist %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_EMPTY_LOG_MESSAGE",
    "error_description": "Empty log message!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ERROR_EMPTY_LOG_MESSAGE` | Empty log message! | No text provided for the log entry ||
|#

The method may also return errors from the business process.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-activity-add.md)
- [{#T}](./bizproc-activity-update.md)
- [{#T}](./bizproc-activity-list.md)
- [{#T}](./bizproc-activity-delete.md)