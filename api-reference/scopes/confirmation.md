# Method Calls with Confirmation

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Some methods require the Bitrix24 administrator's permission to be called. Confirmation protects actions performed by an application that require elevated access.

The application must handle the `METHOD_CONFIRM_WAITING` error, receive the administrator's decision from the event, and retry the method call after approval.

{% include [Footnote on Examples](../../_includes/examples.md) %}

## When Confirmation Is Required

Confirmation is required for applications that call methods with elevated access. For example, [`voximplant.user.get`](../../api-reference/telephony/voximplant/users/voximplant-user-get.md) returns telephony user settings and requires an administrator's decision.

Other methods follow the standard authorization and permission rules.

## How Confirmation Works

1. The application calls a method that requires administrator confirmation
2. Bitrix24 sends the administrator a notification asking them to allow or deny the call
3. The method returns the `METHOD_CONFIRM_WAITING` error to the application
4. After the administrator makes a decision, Bitrix24 calls the [`OnAppMethodConfirm`](../common/events/on-app-method-confirm.md) event handler
5. If the call is allowed, the application can retry it with the same authorization token

The permission or denial is associated with the application and the authorization token used to call the method. The decision remains valid until the token expires. When the application receives a new token, the method call requires new confirmation.

Calling the method again before the administrator makes a decision returns `METHOD_CONFIRM_WAITING` but does not create another confirmation request.

## Method Responses

The result depends on the administrator's decision: the method reports that confirmation is pending, returns data after approval, or returns an error after denial.

### Pending Confirmation

Until the administrator makes a decision, the method returns `METHOD_CONFIRM_WAITING`.

```http
GET https://portal.bitrix24.com/rest/voximplant.user.get?auth=fkp963yuv1ggkfbs5z3f5hy8lilm0iw6&USER_ID=1
HTTP/1.1 401 Unauthorized
{
    "error": "METHOD_CONFIRM_WAITING",
    "error_description": "Waiting for confirmation"
}
```

### Call Approved

If the administrator allows the action, the application can use the same authorization token to call the requested method.

```http
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

### Call Denied

If the administrator denies the action, the method returns `METHOD_CONFIRM_DENIED`.

```http
GET https://portal.bitrix24.com/rest/voximplant.user.get?auth=fkp963yuv1ggkfbs5z3f5hy8lilm0iw6&USER_ID=1
HTTP/1.1 403 Forbidden
{
    "error": "METHOD_CONFIRM_DENIED",
    "error_description": "Method call denied"
}
```

## OnAppMethodConfirm Event

After the administrator makes a decision, Bitrix24 calls the [`OnAppMethodConfirm`](../common/events/on-app-method-confirm.md) event handler. The event contains the confirmation result and the token for which the decision was made.

If the application does not handle the event, it can determine the decision only by calling the method again: a successful response means approval, while `METHOD_CONFIRM_DENIED` means denial.

```json
{
    "event": "ONAPPMETHODCONFIRM",
    "data": {
        "TOKEN": "fkp963yuv1ggkfbs5z3f5hy8lilm0iw6",
        "METHOD": "voximplant.user.get",
        "CONFIRMED": "1",
        "LANGUAGE_ID": "en"
    },
    "ts": "1478790852",
    "auth": {
        "domain": "portal.bitrix24.com",
        "client_endpoint": "https://portal.bitrix24.com/rest/",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "member_id": "74ef8a46a75104de55d5d4a61b98ab6d",
        "application_token": "c289487163b58658eae5e8b42eaf11b8"
    }
}
```

The `CONFIRMED` value indicates the administrator's decision: `1` means the call is allowed, and `0` means it is denied. See [`OnAppMethodConfirm`](../common/events/on-app-method-confirm.md) for a complete field description.

## Method Requiring Confirmation

Confirmation is required for [`voximplant.user.get`](../../api-reference/telephony/voximplant/users/voximplant-user-get.md).
