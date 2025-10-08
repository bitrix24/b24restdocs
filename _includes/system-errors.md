### Statuses and System Error Codes

HTTP status: **20x**, **40x**, **50x**

The errors described below may occur when calling any method.

#|
|| **Status** | **Code**
**Error Message** | **Description** ||
|| `500` | `INTERNAL_SERVER_ERROR`
Internal server error | An internal server error has occurred, please contact the server administrator or [Bitrix24 technical support](/bitrix-support.html) ||
|| `500` | `ERROR_UNEXPECTED_ANSWER`
Server returned an unexpected response | An internal server error has occurred, please contact the server administrator or [Bitrix24 technical support](/bitrix-support.html) ||
|| `503` | `QUERY_LIMIT_EXCEEDED`
Too many requests | The [request intensity limit](/limits.html) has been exceeded ||
|| `405` | `ERROR_BATCH_METHOD_NOT_ALLOWED`
Method is not allowed for batch usage | The current method is not allowed to be called using [batch](/settings/how-to-call-rest-api/batch.html) ||
|| `400` | `ERROR_BATCH_LENGTH_EXCEEDED`
Max batch length exceeded | The maximum length of parameters passed to the [batch](/settings/how-to-call-rest-api/batch.html) method has been exceeded ||
|| `401` | `NO_AUTH_FOUND`
Wrong authorization data | Invalid [access token](/settings/oauth/index.html) or [webhook code](/local-integrations/local-webhooks.html) ||
|| `400` | `INVALID_REQUEST`
Https required. | The HTTPS protocol is required for method calls ||
|| `503` | `OVERLOAD_LIMIT`
REST API is blocked due to overload | The REST API is blocked due to overload. This is a manual individual block, to remove it you need to contact [Bitrix24 technical support](/bitrix-support.html) ||
|| `403` | `ACCESS_DENIED`
REST API is available only on commercial plans | The REST API is available only on commercial plans. ||
|| `403` | `INVALID_CREDENTIALS`
Invalid request credentials | The user whose [access token](/settings/oauth/index.html) or [webhook](/local-integrations/local-webhooks.html) was used to call the method lacks permissions ||
|| `404` | `ERROR_MANIFEST_IS_NOT_AVAILABLE`
Manifest is not available. | The manifest is not available. ||
|| `403` | `insufficient_scope`
The request requires higher privileges than provided by the webhook token | The request requires higher privileges than those provided by the [webhook token](/local-integrations/local-webhooks.html) ||
|| `401` | `expired_token`
The access token provided has expired | The provided [access token](/settings/oauth/index.html) has expired ||
|| `403` | `user_access_error`
The user does not have access to the application | The user does not have access to the application. This means that the application is installed, but the account administrator has granted access to this application only to specific users ||
|#