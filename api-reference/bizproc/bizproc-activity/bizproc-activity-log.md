# Write Information to the Business Process Log bizproc.activity.log

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- The description needs to include what this log is, where it is located, and what it looks like.
- Parameter types and a link to the types page are missing.
- It needs to clarify what the unique identifier is and why it is needed.
- There is no note about required parameters.
- Examples are lacking.
- The example is unclear; it would be better to provide a description and explain what is happening, or add a link to a tutorial where this method is used in a real task.
- Standard blocks are missing.

{% endnote %}

{% endif %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Edits are needed to meet writing standards.
- Parameter types are not specified.
- The requirement for parameters is not indicated.
- Examples are absent.
- There is no response in case of success.
- There is no response in case of error.

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method logs information into the business process log.

## Parameters

#|
|| **Parameter**    | **Description**  ||
|| **EVENT_TOKEN**^*^ | A unique key that must be used when sending an event to the business process.    ||
|| **LOG_MESSAGE**^*^ | Messages to be recorded in the log. ||
|#

## Example

{% list tabs %}

- JS

    ```javascript
    var params = {
        event_token: '55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90',
        log_message: 'Please wait for answer!'
    };

    BX24.callMethod(
        'bizproc.activity.log',
        params,
        function(result) {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

- B24-PHP-SDK

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

{% include [Note on examples](../../../_includes/examples.md) %}