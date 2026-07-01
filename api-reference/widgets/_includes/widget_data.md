#|
|| **Parameter**
`type` | **Description** ||
|| **DOMAIN***
[`string`](/api-reference/data-types.html) | The Bitrix24 address where the widget handler was invoked ||
|| **PROTOCOL***
[`string`](/api-reference/data-types.html) | Secure or non-secure HTTP protocol:

- `0` - HTTP
- `1` - HTTPS
 ||
|| **LANG***
[`string`](/api-reference/data-types.html) | The user interface language of Bitrix24 that invoked the widget. You can localize the interface language in your widget based on this value ||
|| **APP_SID**
[`string`](/api-reference/data-types.html) | String identifier of the application that registered the widget handler ||
|| **AUTH_ID**
[`string`](/api-reference/data-types.html) | Authorization token [OAuth 2](/settings/oauth/simple-way.html) issued for the user who invoked the widget. Can be used for REST API calls on behalf of this user ||
|| **AUTH_EXPIRES**
[`integer`](/api-reference/data-types.html) | Time in seconds after which the authorization token will become invalid ||
|| **REFRESH_ID**
[`string`](/api-reference/data-types.html) | Refresh token [OAuth 2](/settings/oauth/simple-way.html) issued for the user who invoked the widget. Can be used to refresh the authorization token on behalf of this user ||
|| **member_id***
[`string`](/api-reference/data-types.html) | Unique string identifier of Bitrix24 where the widget handler was invoked.  ||
|| **status**
[`string`](/api-reference/data-types.html) | Type of application that registered the handler for this widget. Accepts values:

- `L` - [local](/local-integrations/local-apps.html) application
- `F` - [free mass-market](/market/index.html) application
- `S` - [subscription mass-market](/market/monetization/index.html) application
||
|| **PLACEMENT***
[`string`](/api-reference/data-types.html) | Code for the widget embedding location. You can use the same handler URL for all your widgets. The value that Bitrix24 will report in the `PLACEMENT` parameter will help determine from which specific widget embedding location your handler was invoked in each case ||
|| **PLACEMENT_OPTIONS**
[`string`](/api-reference/data-types.html) | Additional data in the form of a JSON string that defines the context of the widget execution. For example, this could be an array containing the numeric identifier of the CRM object in the detail form where the widget handler was invoked, etc. The `PLACEMENT_OPTIONS` parameter, along with the `PLACEMENT` parameter, allows you to accurately determine for which specific widget embedding location and object the widget handler was invoked. ||
|#
