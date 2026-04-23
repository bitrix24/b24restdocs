# Security Recommendations for Applications Using REST API

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The security of an application relies on proper handling of tokens, data validation, and securing communication channels. Errors at these stages can lead to information leaks or unauthorized access.

Follow basic security rules at all stages: during authentication, event processing, and data storage. This will help establish a secure workflow for the application.

## Step-by-Step Guide for Secure Requests

1. Accept the request from the client and validate the incoming parameters.
2. Verify the token on the server side.
3. Ensure that the access permissions `scope` are sufficient for the operation.
4. Make the REST API call over HTTPS.
5. Handle API limits and errors.
6. Log the result without personal data.

## Validate Incoming Data

Do not assume parameters from users or external services are safe by default. Validate values before executing business logic and making API calls.

Basic rules:

- Validate types, ranges, and required fields
- Reject requests with incorrect data formats
- Do not use incoming values in SQL queries, file paths, and system commands without sanitization
- Log validation errors without sensitive data

Example of validating an incoming parameter:

```php
$taskId = filter_input(INPUT_POST, 'TASK_ID', FILTER_VALIDATE_INT);

if ($taskId === false || $taskId <= 0)
{
    throw new \RuntimeException('Invalid TASK_ID');
}
```

## Do Not Trust Tokens from Client-Side Code

Always verify the token on the server side of the application before performing actions. Do not trust the token obtained from client-side code.

Verification helps:

- Filter out fake or expired tokens
- Reduce the risk of unauthorized access to data
- Centralize security control in one place

Minimum checklist:

- Perform verification only on the server side of the application
- Monitor the token's lifespan and handle its expiration
- Ensure the token was issued for your application and Bitrix24
- Do not execute business logic if the token verification fails
- Log unsuccessful verifications in the security log

For cloud and on-premise, use a unified approach to token renewal through the Bitrix24 authorization server. For details, refer to the [overview of cloud and on-premise differences](./index.md).

## Act on Token Compromise

If you suspect a token leak, act immediately according to a pre-prepared plan.

1. Stop processing requests with the suspicious token.
2. Revoke the token and issue a new pair.
3. Check the API logs for any unusual activity.
4. Update application secrets if they may have been compromised.
5. Document the incident and notify responsible parties.

Removing the application invalidates all saved tokens. Read more in the article [Application Removal](../app-uninstallation.md).

To properly terminate the application, register a handler for [`OnAppUninstall`](../../api-reference/common/events/on-app-uninstall.md). In this event, the authorization data is no longer valid, so be sure to check the `application_token`. This check confirms that the call indeed came from Bitrix24.

## Validate application_token in Events

For each incoming event, check the `application_token` field. Compare it with the value stored on the server for the corresponding `member_id`, and process the event only if they match.

If the `application_token` verification fails, the event should be rejected and logged.

Recommended order:

- Save the `application_token` during application installation via [`OnAppInstall`](../../api-reference/common/events/on-app-install.md) linked to `member_id`
- Update the saved value during the `OnAppUpdate` event if Bitrix24 has provided a new `application_token`
- Compare the token value for each incoming event before any data processing
- Do not trigger the event handler if the `application_token` does not match or `member_id` is not found
- Log the rejection without recording sensitive data

Algorithm for validating an incoming event:

1. Accept the HTTP request and check the structure of the `auth` field.
2. Verify the presence of `application_token`.
3. Retrieve `member_id` from `auth` and find the stored `application_token` for this `member_id`.
4. Compare the `application_token` from the request with the stored value.
5. If they do not match, return a rejection and do not trigger the business logic handler.
6. If they match, trigger the handler and log the processing result.

Result of a successful verification:

```json
{
  "status": "ok",
  "message": "event accepted"
}
```

Event rejection:

```json
{
  "status": "error",
  "error": "invalid_application_token"
}
```

The format of events and the structure of the `auth` field are described in the [Events](../../api-reference/events/index.md) section.

Keep in mind that the `auth` field does not always contain valid authorization tokens. In automated scenarios and certain events, authorization data may be absent or outdated.

Technical requirements for receiving events and network access can be found in the [Required Network Access](./network-access.md) section.

Example of an event with `application_token`:

```json
{
  "event": "ONCRMLEADUPDATE",
  "data": {
    "FIELDS": {
      "ID": "123"
    }
  },
  "auth": {
    "access_token": "xxxx",
    "domain": "example.bitrix24.com",
    "application_token": "51856fefc120afa4b628cc82d3935cce"
  }
}
```

## Do Not Expose Secrets in the Frontend

Do not place secret webhook keys, OAuth tokens, and other sensitive data in client-side code.

Safe approach: perform all operations with tokens and keys through the server side of the application. The server controls access and hides secret data.

## Request Minimum Necessary Scope

When installing and authorizing the application, request only the permissions needed for current scenarios. Excessive permissions increase the damage in case of a breach.

Recommendations:

- Document the minimum set of scopes during the design phase
- Do not add permissions "just in case"
- Regularly review the list and remove unused permissions

Check the permissions for each method in its description through the `scope` block.

## Consider REST API Limits

When exceeding the request limit, cloud Bitrix24 returns status `503` and error code `QUERY_LIMIT_EXCEEDED`.

To reduce the risk of blocks:

- Reduce the number of requests and use caching
- Use batch calls via `batch`
- Add retries with delays for `QUERY_LIMIT_EXCEEDED`
- Monitor the load on event handlers

If the event handler is unavailable, the event may be lost. For reliability, use an incoming queue.

Example of handling `QUERY_LIMIT_EXCEEDED`:

```php
$maxRetries = 3;
$attempt = 0;

while ($attempt < $maxRetries)
{
    $response = $client->call($method, $payload);
    $status = $response['status'] ?? 200;
    $error = $response['error'] ?? null;

    if ($status === 503 && $error === 'QUERY_LIMIT_EXCEEDED')
    {
        usleep((2 ** $attempt) * 1000000);
        $attempt++;
        continue;
    }

    break;
}
```

{% note tip "More Details" %}

- [REST API Limits](../../limits.md)
- [General Performance Recommendations](../performance/index.md)
- [Incoming Queue for Events](../performance/queue.md)

{% endnote %}

## Use HTTPS

Transmit tokens and working data only over a secure HTTPS channel. Do not disable SSL certificate verification in the production environment.

Recommendations:

- Use only `https://` in endpoint and callback URL
- Verify the certificate chain and hostname in the HTTP client of the server side of the application
- Disable certificate verification only during isolated local debugging

For network access requirements, refer to the [Required Network Access](./network-access.md) section.

## Minimize Storage of Personal Data

Store only the data necessary for the application's operation. Do not collect information "for the future."

Practical steps:

- Define retention periods and delete outdated records
- Limit data access by roles
- Do not store tokens and personal data in open logs

## Isolate Applications

If multiple applications are running on the server, separate their resources:

- Use separate databases
- Separate file access permissions
- Run applications under different user accounts
- Store secrets for each application separately

This approach limits the risk area and prevents a single vulnerability from affecting all applications at once.

## Maintain a Security Event Log

Keep a log of key security events to quickly identify problems and investigate incidents.

What to log:

- Unsuccessful token and `application_token` verifications
- Validation errors for incoming data
- API limit triggers and retries
- Access denials and authorization errors

What not to log:

- `access_token` and `refresh_token` in plain text
- Personal data without necessity
- Application secrets and service keys

Example of secure logging on the server side of the application:

```php
$maskedToken = substr($token, 0, 4) . '***';
$b24Domain = $domain ?? '-';

error_log(sprintf(
    'AUTH_FAIL app=%s b24=%s token=%s ip=%s',
    $appId,
    $b24Domain,
    $maskedToken,
    $_SERVER['REMOTE_ADDR'] ?? '-'
));
```

## Security Measures Priority

### Mandatory {#must}

- Validation of incoming data
- Validation of tokens and `application_token`
- Use of HTTPS
- Prohibition of storing secrets in the frontend

### Recommended {#should}

- Control of API limits and resilience of event handlers
- Isolation of applications by access, databases, and secrets
- Regular review of `scope` access permissions

### Additional

- Automated checks for security requirements in CI/CD
- Enhanced incident analytics and reporting for the team

## Anti-Patterns

Avoid the following solutions:

- Passing webhook keys to the frontend
- Disabling SSL certificate verification in production
- Heavy synchronous logic in event handlers without a queue
- Using a shared database and secrets for different applications
- Storing `application_token` on the client side
- Modifying data in an event handler that triggers the same event again

## Release Checklist

Before publishing the application, check the following:

- Requirements from the [Mandatory](#must) and [Recommended](#should) sections are met for all test scenarios
- The token compromise scenario has been executed at least once in the test environment
- There are no `access_token`, `refresh_token`, and personal data in the logs of the test run
- The event processing queue is enabled, and the handler works correctly under peak incoming events

Adhering to these rules reduces the risk of leaks and increases the stability of the integration.

## Continue Learning

- [Differences in Using REST API in Cloud and On-Premise Bitrix24](./index.md)
- [Required Network Access](./network-access.md)