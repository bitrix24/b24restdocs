# Send push notifications to mobile devices within the Bitrix24 application pull.application.push.add

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

The method `pull.application.push.add` is used to send push notifications to mobile devices within the Bitrix24 application.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_ID**^*^ | Identifiers of users receiving the push notifications. ||
|| **TEXT** | Arbitrary text. ||
|| **AVATAR** | Link to an image. ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JavaScript
  
    ```js
    BX24.callMethod('pull.application.push.add', {
        'USER_ID': [1,2,3],
        'TEXT': 'Hello, world!',
        'AVATAR': 'https://files.shelenkov.com/images/avatar-smith.jpg',
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
    $result = restCommand('pull.application.push.add', [
        'USER_ID': [1,2,3],
        'TEXT': 'Hello, world!',
        'AVATAR': 'https://files.shelenkov.com/images/avatar-smith.jpg',
    ], $_REQUEST["auth"]);
    ```

{% endlist %}

{% include [Example notes](../../../_includes/examples.md) %}

## Successful response

> 200 OK

```json
{
    "result": true
}
```

## Error response

> 200 Error, 50x Error

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Send push notifications available only for application authorization."
}
```

Keys:

- **error** - code of the occurred error
- **error_description** - brief description of the occurred error
  

### Possible error codes

#|
|| **Code** | **Description** ||
|| TEXT_ERROR     | Message text not provided. ||
|| EMPTY_APP_NAME | Error occurs if your application does not have a name. ||
|| ACCESS_ERROR    | The method can only be used by a user with administrator rights. ||
|| WRONG_AUTH_TYPE | The method can only be used within [OAuth 2.0](https://training.bitrix24.com/support/training/course/index.php?COURSE_ID=169&LESSON_ID=20110&LESSON_PATH=13643.20052.20096.20110). ||
|#

## See also

- [Interactivity in applications](https://training.bitrix24.com/support/training/course/index.php?COURSE_ID=169&CHAPTER_ID=020088&LESSON_PATH=13643.20052.20088)