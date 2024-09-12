# Get a List of Triggers crm.automation.trigger.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator with access to CRM in the application context

This method retrieves a list of applications and triggers.

The method can only be executed in the context of an application.

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automation.trigger.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automation.trigger.list
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.automation.trigger.list',
        {},
        function(result) 
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.automation.trigger.list',
        []
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
    "result": [
        {
            "NAME": "Trigger 1",
            "CODE": "trigger1"
        },
        {
            "NAME": "Trigger 2",
            "CODE": "trigger2"
        }
    ],
    "time":{
        "start":1718952595.479501,
        "finish":1718952595.594397,
        "duration":0.11489605903625488,
        "processing":0.007472038269042969,
        "date_start":"2024-06-21T06:49:55+00:00",
        "date_finish":"2024-06-21T06:49:55+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Returns an array of triggers added by the application with fields `NAME` and `CODE` ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ACCESS_DENIED",
    "error_description":"Access denied! Admin permissions required"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Access denied. | User did not pass the preliminary access rights check for CRM ||
|| ACCESS_DENIED | Access denied! Admin permissions required | Admin rights check failed ||
|| ACCESS_DENIED | Access denied! Application context required | Method called outside of application context ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-automation-trigger-add.md)
- [{#T}](./crm-automation-trigger-execute.md)
- [{#T}](./crm-automation-trigger-delete.md)