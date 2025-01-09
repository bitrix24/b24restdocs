#|
|| **Parameter**
`type` | **Description** ||
|| **DOMAIN***
[`string`](../../data-types.md) | The Bitrix24 address where the widget handler was invoked ||
|| **PROTOCOL***
[`string`](../../data-types.md) | Secure or non-secure HTTP protocol:

- `0` - HTTP
- `1` - HTTPS
 ||
|| **LANG***
[`string`](../../data-types.md) | The user interface language of Bitrix24 that invoked the widget. You can localize the interface language in your widget based on this value ||
|| **APP_SID**
[`string`](../../data-types.md) | String identifier of the application that registered the widget handler ||
|| **AUTH_ID**
[`string`](../../data-types.md) | Authorization token [OAuth 2](../../oauth/simple-way.md) issued for the user who invoked the widget. Can be used for REST API calls on behalf of this user ||
|| **AUTH_EXPIRES**
[`integer`](../../data-types.md) | Time in seconds after which the authorization token will become invalid ||
|| **REFRESH_ID**
[`string`](../../data-types.md) | Refresh token [OAuth 2](../../oauth/simple-way.md) issued for the user who invoked the widget. Can be used to refresh the authorization token on behalf of this user ||
|| **member_id***
[`string`](../../data-types.md) | Unique string identifier of Bitrix24 where the widget handler was invoked.  ||
|| **status**
[`string`](../../data-types.md) | Type of application that registered the handler for this widget. Accepts values:

- `L` - [local](../../../local-integrations/local-apps.md) application
- `F` - [free mass-market](../../../market/index.md) application
||
|| **PLACEMENT***
[`string`](../../data-types.md) | Code for the widget embedding location. You can use the same handler URL for all your widgets. The value that Bitrix24 will report in the `PLACEMENT` parameter will help determine from which specific widget embedding location your handler was invoked in each case ||
|| **PLACEMENT_OPTIONS**
[`string`](../../data-types.md) | Additional data in the form of a JSON string that defines the context of the widget execution. For example, this could be an array containing the numeric identifier of the CRM entity in the detail form where the widget handler was invoked, etc. The `PLACEMENT_OPTIONS` parameter, along with the `PLACEMENT` parameter, allows you to accurately determine for which specific widget embedding location and object the widget handler was invoked. ||
|#