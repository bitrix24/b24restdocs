# Send Events to the RT Channel of the pull.application.event.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`pull`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `pull.application.event.add` is used to send events to the RT channel of the application.

#|
|| **Parameter** | **Description** ||
|| **COMMAND**^*^ | Type of event, string. ||
|| **PARAMS** | Arbitrary JSON array with data. ||
|| **MODULE_ID** | If commands are sent from different subsystems of the application, this can be specified through the module. ||
|| **USER_ID** | If USER_ID is not specified, the data will be sent to the general channel. If a user ID is specified, the data will be sent to a private channel. An administrator can send to multiple users and the general channel simultaneously, while a user without permissions can only send to themselves or the general channel. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JavaScript
  
    ```js
    BX24.callMethod('pull.application.event.add', {
        'COMMAND': 'test',
        'PARAMS': '{"param1":"value1"}',
    }, function(result){
        if(result.error())
        {
            console.error(result.error().ex);
        }
        else
        {
            console.log(result.data());
        }
    });
    ```

- PHP

    ```php
    $result = restCommand('pull.application.event.add', [
        'COMMAND' => 'test',
        'PARAMS' => ['param1' => 'value1'],
    ], $_REQUEST["auth"]);
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Response on Success

> 200 OK

```json
{
    "result": true
}
```

## Response on Error

> 200 Error, 50x Error

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Get access to application config available only for application authorization."
}
```
Keys:

- **error** - code of the occurred error
- **error_description** - brief description of the occurred error
  
### Possible Error Codes

#|
|| **Code** | **Description** ||
|| COMMAND_ERROR        | The format of the MODULE_ID field is incorrect. Allowed are English letters in mixed case, digits, underscore, dot, and hyphen. ||
|| MODULE_ID_ERROR     | The format of the MODULE_ID field is incorrect. Allowed are English letters in lowercase, digits, dot, and underscore. ||
|| USER_ID_ACCESS_ERROR | Only users with administrator rights can specify arbitrary users. ||
|| PARAMS_ERROR         | An incorrect JSON object was passed. ||
|| WRONG_AUTH_TYPE     | The method can only be used within [OAuth 2.0](../../oauth/index.md). ||
|#

## See Also

- [Interactivity in Applications](../../interactivity/index.md)