**Parameters in the Handler URL Query String**

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
|| **APP_SID***
[`string`](/api-reference/data-types.html) | Application session identifier. Bitrix24 generates a new one each time the widget is rendered and uses it to link the js library with the application environment ||
|#

**Parameters in the POST Request Body**

#|
|| **Parameter**
`type` | **Description** ||
|| **AUTH_ID**
[`string`](/api-reference/data-types.html) | Authorization token [OAuth 2](/settings/oauth/simple-way.html) issued for the user who invoked the widget. Can be used for REST API calls on behalf of this user ||
|| **AUTH_EXPIRES**
[`integer`](/api-reference/data-types.html) | Time in seconds after which the authorization token will become invalid ||
|| **REFRESH_ID**
[`string`](/api-reference/data-types.html) | Refresh token [OAuth 2](/settings/oauth/simple-way.html) issued for the user who invoked the widget. Can be used to refresh the authorization token on behalf of this user ||
|| **SERVER_ENDPOINT***
[`string`](/api-reference/data-types.html) | Address of the Bitrix24 authorization server needed to refresh [OAuth 2](/settings/oauth/simple-way.html) tokens ||
|| **APPLICATION_TOKEN***
[`string`](/api-reference/data-types.html) | Application token. The same value is passed in the `application_token` parameter when [event handlers](/api-reference/events/safe-event-handlers.html) are invoked. The widget handler can use it to verify that the request came from Bitrix24 ||
|| **APPLICATION_SCOPE***
[`string`](/api-reference/data-types.html) | List of [scopes](/api-reference/scopes/permissions.html) granted to the application, separated by commas. Shows which REST API methods are available with the authorization token received ||
|| **member_id***
[`string`](/api-reference/data-types.html) | Unique string identifier of Bitrix24 where the widget handler was invoked.  ||
|| **status**
[`string`](/api-reference/data-types.html) | Type of application that registered the handler for this widget. Accepts values:

- `L` - [local](/local-integrations/local-apps.html) application
- `F` - [free mass-market](/market/index.html) application
- `D` - demo version of a mass-market application
- `T` - trial version of a mass-market application, time-limited
- `P` - paid mass-market application
||
|| **PLACEMENT***
[`string`](/api-reference/data-types.html) | The placement code. You can use the same handler URL for all your widgets. The value that Bitrix24 will report in the `PLACEMENT` parameter will help determine from which specific placement your handler was invoked in each case ||
|| **PLACEMENT_OPTIONS**
[`string`](/api-reference/data-types.html) | Additional data in the form of a JSON string that defines the context of the widget execution. For example, this could be an array containing the numeric identifier of the CRM object in the detail form where the widget handler was invoked, etc. The `PLACEMENT_OPTIONS` parameter, along with the `PLACEMENT` parameter, allows you to accurately determine for which specific placement and object the widget handler was invoked ||
|#

Bitrix24 adds a `URI` key to `PLACEMENT_OPTIONS` — the path with the query string of the page from which the widget was opened. It arrives for any placement, along with the keys of that placement itself. The key is absent if the browser did not send the `Referer` header or if the widget was opened from a page on a different domain.
