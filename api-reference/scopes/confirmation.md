# Method Calls with Confirmation

Some methods require permission from the account administrator to be called. When such a method is invoked by an application, the account administrator will receive a notification asking to allow or deny the call, while the application will receive an error.

The permission or denial is granted to a specific authorization token used to call the method. This means that the permission is valid for the lifetime of the token, and a new permission must be obtained when receiving the next token.

{% include [Footnote on Examples](../../_includes/examples.md) %}

```js
GET https://portal.bitrix24.com/rest/voximplant.user.get?auth=fkp963yuv1ggkfbs5z3f5hy8lilm0iw6&USER_ID=1
HTTP/1.1 401 Unauthorized
{
    "error": "METHOD_CONFIRM_WAITING", 
    "error_description": "Waiting for confirmation"
}
```

Calling the method before receiving confirmation or a response will yield the same reply, but without a request for re-confirmation.

When the administrator confirms or denies the permission, the event handler [`OnAppMethodConfirm`](../common/events/on-app-method-confirm.md) will be triggered, passing the confirmation result along with the token that was granted this permission:

```js
array (
    'event' => 'ONAPPMETHODCONFIRM',
    'data' => 
    array (
        'TOKEN' => 'fkp963yuv1ggkfbs5z3f5hy8lilm0iw6',
        'METHOD' => 'voximplant.user.get',
        'CONFIRMED' => '1',
        'LANGUAGE_ID' => 'en',
        ),
    'ts' => '1478790852',
    'auth' => 
    array (
        'domain' => 'portal.bitrix24.com',
        'client_endpoint' => 'https://portal.bitrix24.com/rest/',
        'server_andpoint' => 'https://oauth.bitrix.info/rest/',
        'member_id' => '74ef8a46a75104de55d5d4a61b98ab6d',
        'application_token' => 'c289487163b58658eae5e8b42eaf11b8',
	),
```

If the administrator allows the action, the application can use the same authorization token to work with the requested method:

```js
GET https://portal.bitrix24.com/rest/voximplant.user.get?auth=fkp963yuv1ggkfbs5z3f5hy8lilm0iw6&USER_ID=1
HTTP/1.1 200 OK
{
    "result": [
        {
            "DEFAULT_LINE": null, 
            "ID": "1", 
            "INNER_NUMBER": null, 
            "PHONE_ENABLED": "Y", 
            "SIP_LOGIN": "****", 
            "SIP_PASSWORD": "*****", 
            "SIP_SERVER": "*****"
        }
    ]
}
```

In case of denial, the corresponding error will be returned:

```js
GET https://portal.bitrix24.com/rest/voximplant.user.get?auth=fkp963yuv1ggkfbs5z3f5hy8lilm0iw6&USER_ID=1
HTTP/1.1 403 Forbidden
{
    "error": "METHOD_CONFIRM_DENIED", 
    "error_description": "Method call denied"
}
```

## List of Methods Requiring Confirmation

- [{#T}](../../api-reference/telephony/voximplant/users/voximplant-user-get.md)