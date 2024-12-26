### Statuses and System Error Codes

HTTP Status: **20x**, **40x**, **50x**

The errors described below may occur when calling any method.

#| 
|| **Status** | **Code**
**Error Message** | **Description** ||
|| `500` | `INTERNAL_SERVER_ERROR`
Internal server error | An internal server error has occurred. ||
|| `500` | `ERROR_UNEXPECTED_ANSWER`
Server returned an unexpected response | An internal server error has occurred. ||
|| `503` | `QUERY_LIMIT_EXCEEDED`
Too many requests | The [request intensity limit](../limits.md) has been exceeded. ||
|| `200` | `ERROR_BATCH_METHOD_NOT_ALLOWED`
Method is not allowed for batch usage | The current method is not permitted for calls using [batch](../api-reference/how-to-call-rest-api/batch.md). ||
|| `200` | `ERROR_BATCH_LENGTH_EXCEEDED`
Max batch length exceeded | The maximum length of parameters passed to the [batch](../api-reference/how-to-call-rest-api/batch.md) method has been exceeded. ||
|| `200` | `NO_AUTH_FOUND`
Wrong authorization data | Invalid [access token](../api-reference/oauth/index.md) or [webhook code](../local-integrations/local-webhooks.md). ||
|| `200` | `INVALID_REQUEST`
Https required. | The REST methods require the use of the HTTPS protocol. ||
|| `200` | `OVERLOAD_LIMIT`
REST API is blocked due to overload | The REST API is blocked due to overload. This is a manual individual block; to remove it, you need to contact Bitrix24 technical support. ||
|| `200` | `ACCESS_DENIED`
REST API is available only on commercial plans | The REST API is available only on commercial plans. ||
|| `200` | `INVALID_CREDENTIALS`
Invalid request credentials | The user whose [access token](../api-reference/oauth/index.md) or [webhook](../local-integrations/local-webhooks.md) was used to call the method lacks permissions. ||
|| `200` | `ERROR_MANIFEST_IS_NOT_AVAILABLE`
Manifest is not available. | The manifest is not available. ||
|| `200` | `insufficient_scope`
The request requires higher privileges than provided by the webhook token | The request requires higher privileges than those provided by the [webhook token](../local-integrations/local-webhooks.md). ||
|| `200` | `expired_token`
The access token provided has expired | The provided [access token](../api-reference/oauth/index.md) has expired. ||
|| `200` | `user_access_error`
The user does not have access to the application | The user does not have access to the application. This means that the application is installed, but the account administrator has granted access to this application only to specific users. ||
|#