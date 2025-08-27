# Write information to the business process log bizproc.activity.log

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- need to clarify what the unique identifier is and why it is needed
- the example is unclear. It needs a description and explanation of how it works or a link to a tutorial where this method is used in a real task

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method logs information into the business process log. Event logging must be enabled in the business process template.

{% note tip "User documentation" %}

- [Business process debugging log: how to enable log storage](https://helpdesk.bitrix24.com/open/22095380/)

{% endnote %}

## Method parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description**||
|| **EVENT_TOKEN***
[`string`](../../data-types.md) | A unique key required to send an event to the business process.

The token is sent to the application action handler when the business process reaches the execution of this action.

Logging is possible if the application action is subscribed with `'USE_SUBSCRIPTION': 'Y'` during the execution of the business process. ||
|| **LOG_MESSAGE***
[`string`](../../data-types.md) | Message to log ||
|#

## Code examples

{% include [Note on examples](../../../_includes/examples.md) %}

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
    try
    {
    	const response = await $b24.callMethod(
    		'bizproc.activity.log',
    		{
    			event_token: '55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP.47dd5d888e8e549d2c984713e12a4268e6e87d0208ca1f093ba1075e77f92e90',
    			log_message: 'Please wait for answer!'
    		}
    	);
    	
    	const result = response.getData().result;
    	alert("Success: " + result);
    }
    catch( error )
    {
    	alert("Error: " + error);
    }
    ```

- PHP

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

- BX24.js

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

- PHP CRest

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

{% endlist %}

## Error handling

HTTP status: **400**

```json
{
    "error": "ERROR_EMPTY_LOG_MESSAGE",
    "error_description": "Empty log message!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Error message** | **Description** ||
|| `ERROR_EMPTY_LOG_MESSAGE` | Empty log message! | log entry text is not specified ||
|#

The method may also return errors from the business process.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue exploring 

- [{#T}](./index.md)
- [{#T}](./bizproc-activity-add.md)
- [{#T}](./bizproc-activity-update.md)
- [{#T}](./bizproc-activity-list.md)
- [{#T}](./bizproc-activity-delete.md)