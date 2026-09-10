### Statuses and System Error Codes

HTTP Status: **4xx**, **5xx**

The errors described below are returned by the REST API itself, not by the logic of a specific method. They can arrive in response to any method.

#| 
|| **Status** | **Code**
**Error Message** | **Description** ||
|| `500` | `INTERNAL_SERVER_ERROR`
Internal server error | An internal server error has occurred. Retry the call, and if the error persists, contact the server administrator or [Bitrix24 technical support](/bitrix-support.html) ||
|| `500` | `ERROR_UNEXPECTED_ANSWER`
Server returned an unexpected response | The server returned an unexpected response. Retry the call, and if the error persists, contact the server administrator or [Bitrix24 technical support](/bitrix-support.html) ||
|| `503` | `QUERY_LIMIT_EXCEEDED`
Too many requests | The [request intensity limit](/limits.html) has been exceeded ||
|| `429` | `OPERATION_TIME_LIMIT`
Method is blocked due to operation time limit | The method is blocked because the [request resource intensity limit](/limits.html) has been exceeded. The block is lifted automatically once the accumulated execution time of the method no longer exceeds the limit ||
|| `401` | `NO_AUTH_FOUND`
Wrong authorization data | The request contains no authorization data: neither an [access token](/settings/oauth/index.html) nor a [webhook code](/local-integrations/local-webhooks.html) was passed ||
|| `401` | `INVALID_REQUEST`
Https required | Methods are called over the HTTPS protocol only ||
|| `401` | `OVERLOAD_LIMIT`
REST API is blocked due to overload | The REST API is blocked due to overload. This is a manual individual block. To have it lifted, contact [Bitrix24 technical support](/bitrix-support.html) ||
|| `401` | `ACCESS_DENIED`
REST is available only on commercial plans | The REST API is available only on commercial plans. A [webhook](/local-integrations/local-webhooks.html) receives a different error message — `REST is available only by subscription` ||
|| `401` | `INVALID_CREDENTIALS`
Invalid request credentials | No active [webhook](/local-integrations/local-webhooks.html) with the specified user identifier and secret code was found ||
|| `404` | `ERROR_METHOD_NOT_FOUND`
Method not found! | No method with this name was found. The name is misspelled, the method does not exist in the REST API, or it is unavailable without the required [scope](/api-reference/scopes/permissions.html) ||
|| `401` | `insufficient_scope`
The request requires higher privileges than provided by the webhook token | The request requires broader permissions than the token has: for a [webhook](/local-integrations/local-webhooks.html) these are the permissions granted to it, for an application it is the [scope](/api-reference/scopes/permissions.html). For an application, the error message ends with `provided by the access token` ||
|| `401` | `expired_token`
The access token provided has expired | The [access token](/settings/oauth/index.html) has expired ||
|| `401` | `user_access_error`
The user does not have access to the application | The application is installed, but the Bitrix24 administrator has granted access to it only to specific users ||
|| `403` | `PORTAL_DELETED`
Portal was deleted | The public part of the site is closed. To open it on an on-premise installation, disable the "Temporary closure of the public part of the site" option. Path to the setting: *Desktop > Settings > Product Settings > Module Settings > Main Module > Temporary closure of the public part of the site* ||
|#