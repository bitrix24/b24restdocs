# Send Events to the RT Channel of the Application pull.application.event.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

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

{% include [Parameter Note](../../../_includes/required.md) %}

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

{% include [Example Note](../../../_includes/examples.md) %}

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
|| MODULE_ID_ERROR     | The format of the MODULE_ID field is incorrect. Allowed are lowercase English letters, digits, dot, and underscore. ||
|| USER_ID_ACCESS_ERROR | Only a user with administrator rights can specify arbitrary users. ||
|| PARAMS_ERROR         | An incorrect JSON object was provided. ||
|| WRONG_AUTH_TYPE     | The method can only be used within the framework of [OAuth 2.0](https://training.bitrix24.com/support/training/course/index.php?COURSE_ID=169&LESSON_ID=20110&LESSON_PATH=13643.20052.20096.20110). ||
|#

## See Also

- [Interactivity in Applications](https://training.bitrix24.com/support/training/course/index.php?COURSE_ID=169&CHAPTER_ID=020088&LESSON_PATH=13643.20052.20088)