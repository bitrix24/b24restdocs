# Installing and Using B24JsSDK

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

[B24JsSDK](https://github.com/bitrix24/b24jssdk) is the official JavaScript and TypeScript library for working with the Bitrix24 REST API. It handles authorization, rate limiting, and response parsing. Method calls are structured identically in the browser and in Node.js, while the connection class depends on the environment.

The examples on this page are designed for the second major version of the library.

Use B24JsSDK if:

- the application opens within the Bitrix24 interface
- the integration runs on a Node.js server
- authorization via [incoming webhooks](../../local-integrations/local-webhooks.md) or the [OAuth protocol](../../settings/oauth/index.md) is required
- [batch requests](../../settings/how-to-call-rest-api/batch.md) and reading large lists in chunks are required

[BX24.js](../bx24-js-sdk/index.md) solves a more specific task — it works only within the Bitrix24 interface and authorizes via the OAuth protocol. Other Bitrix24 libraries are described in the [SDK overview](../index.md).

## Choosing a Connection Class

The SDK provides three connection classes. The choice depends on where the code is executed and how the application obtains authorization.

#|
|| **Scenario** | **Class** | **Authorization** ||
|| The application opens inside the Bitrix24 interface | `B24Frame` | Current user token, the SDK receives it from Bitrix24 ||
|| Server-side application with persistent access | `B24Hook` | Incoming webhook key ||
|| Server-side application with OAuth authorization | `B24OAuth` | OAuth tokens, the SDK updates them automatically ||
|#

`B24Hook` and `B24OAuth` are ready to work immediately after creation. `B24Frame` is created by the `initializeB24Frame()` function: it retrieves data from the Bitrix24 parent window and waits for initialization. Until then, REST API calls are unavailable.

{% note warning "" %}

An incoming webhook URL contains a secret access key. Use `B24Hook` only on the server and store the URL in an environment variable. In a browser, any user can see the key — use `B24Frame` on the client side.

{% endnote %}

## Installation

### Node.js and Nuxt

The SDK supports Node.js 18, 20, 22, and newer versions. For new projects, use 22 or newer: community support for Node.js 18 and 20 has ended, and security updates are no longer released for them. Install the package:

```bash
npm install @bitrix24/b24jssdk
```

There is a separate module for Nuxt projects. It requires Nuxt 4.2.2 or newer:

```bash
npx nuxi module add @bitrix24/b24jssdk-nuxt
```

The command will install the package and register the module in `nuxt.config`. The module plugin works only on the client and adds `$initializeB24Frame` — it does not exist on the Nuxt server.

The recommended connection format is ESM:

```js
import { B24Hook } from '@bitrix24/b24jssdk'
```

CommonJS is also supported — instead of `import`, use `require`:

```js
const { B24Hook } = require('@bitrix24/b24jssdk')
```

Installation details are described in the B24JsSDK documentation — with separate pages for [Node.js](https://bitrix-tools.github.io/b24jssdk/docs/getting-started/installation/nodejs), [Nuxt](https://bitrix-tools.github.io/b24jssdk/docs/getting-started/installation/nuxt), [Vue](https://bitrix-tools.github.io/b24jssdk/docs/getting-started/installation/vue), and [React](https://bitrix-tools.github.io/b24jssdk/docs/getting-started/installation/react).

### Browser via CDN

Connect the UMD build using the `script` tag. The major version number in the URL ensures compatibility: updates within the second version are applied automatically, while moving to the next major version remains your decision.

```html
<script src="https://unpkg.com/@bitrix24/b24jssdk@2/dist/umd/index.min.js"></script>
```

The build can be downloaded from [unpkg.com](https://unpkg.com/@bitrix24/b24jssdk@2/dist/umd/index.min.js) and connected from your project:

```html
<script src="/path/to/umd/index.min.js"></script>
```

After connecting, the library is available via the global variable `B24Js`.

The UMD build includes its dependencies within the file, so `npm audit` does not check them. If the project is built via npm, connect the SDK as ESM or CommonJS — in those cases, dependencies remain external.

## First Call Inside the Bitrix24 Interface

The code below runs in an application opened inside Bitrix24. It initializes `B24Frame` and requests a list of companies using the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method. The application requires the `crm` scope.

The example is provided for a UMD build, where everything is available via the global variable `B24Js`. In the project, the bundler imports the same names from the package: `import { initializeB24Frame, LoggerFactory, EnumCrmEntityTypeId, Text } from '@bitrix24/b24jssdk'`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bitrix24 Frame Demo</title>
</head>
<body>
<p>The result is output to the developer console</p>
<script src="https://unpkg.com/@bitrix24/b24jssdk@2/dist/umd/index.min.js"></script>
<script>
    const logger = B24Js.LoggerFactory.createForBrowser('local-app', true)

    async function main() {
        const $b24 = await B24Js.initializeB24Frame()

        const response = await $b24.actions.v2.call.make({
            method: 'crm.item.list',
            params: {
                entityTypeId: B24Js.EnumCrmEntityTypeId.company,
                select: ['id', 'title'],
                order: { id: 'desc' }
            },
            requestId: B24Js.Text.getUuidRfc4122()
        })

        // Data is only available upon a successful response
        if (!response.isSuccess) {
            logger.error('REST API error', { messages: response.getErrorMessages() })
            return
        }

        logger.info('Companies', { items: response.getData().result.items })
    }

    document.addEventListener('DOMContentLoaded', () => {
        main().catch((error) => logger.error('Failed to launch the application', { error }))
    })
</script>
</body>
</html>
```

What each part does:

- `initializeB24Frame()` returns a ready-to-use `$b24` object
- `actions.v2.call.make()` calls a single REST API method
- `requestId` is optional, but is sent with the request and helps locate it in the logs
- `isSuccess` shows whether Bitrix24 returned a result
- `getErrorMessages()` provides REST API error messages

The page must open as an [application](../../settings/app-installation/index.md) inside the Bitrix24 interface. Outside of this context, `initializeB24Frame()` does not hang, but immediately rejects the promise with error `SdkError` and code `JSSDK_CLIENT_SIDE_WARNING`. Therefore, always wrap the call in `try/catch` or handle it via `catch()`, as shown in the example above.

## First Call on the Server via Webhook

Create an [incoming webhook](../../local-integrations/local-webhooks.md) in the **Developer resources** section and copy a URL such as `https://example.bitrix24.com/rest/1/webhook_key/`. Save the URL in an environment variable — the secret key should not be in the code.

The example uses `import` and `await` at the top level, so the file must be an ESM module: specify `"type": "module"` in `package.json` or use the `.mjs` file extension.

```js
import { B24Hook, EnumCrmEntityTypeId, LoggerFactory } from '@bitrix24/b24jssdk'

const logger = LoggerFactory.createForBrowser('node-hook', process.env.NODE_ENV === 'development')

if (!process.env.B24_HOOK) {
    throw new Error('Environment variable B24_HOOK is not set')
}

const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

const response = await $b24.actions.v2.call.make({
    method: 'crm.item.list',
    params: {
        entityTypeId: EnumCrmEntityTypeId.company,
        select: ['id', 'title']
    },
    requestId: 'companies-list'
})

if (!response.isSuccess) {
    logger.error('REST API error', { messages: response.getErrorMessages() })
    process.exit(1)
}

logger.info('Companies', { items: response.getData().result.items })
```

`fromWebhookUrl()` validates the URL when creating the object: the protocol must be HTTPS, and the user identifier must be a number. If the URL is invalid, the method throws an exception immediately rather than on the first request.

Webhook permissions are set during creation: you select the required [scopes](../../api-reference/scopes/permissions.md), which determine the available set of methods. For the example above, the webhook requires the `crm` scope. If the method returns an access error, check the webhook permissions.

`LoggerFactory.createForBrowser()` in the example above is not a typo: this same factory is used in the server-side examples of the SDK documentation; there is no separate factory for Node.js in the library.

The second argument of the factory is the development mode flag. When `true`, the logger prints all messages; when `false` or without an argument, it prints only errors, and `logger.info()` is silently discarded. Therefore, in the example above, the "Companies" string will only appear if the `NODE_ENV` environment variable is set to `development`.

## Connection via OAuth Authorization

Marketplace applications and Local applications operate via OAuth tokens. Bitrix24 transmits a pair of tokens during application installation and in the [ONAPPINSTALL](../../api-reference/common/events/on-app-install.md) and `ONAPPUPDATE` events. Save them and pass them to `B24OAuth` along with the application's `clientId` and `clientSecret`.

`B24OAuth`, just like `B24Hook`, works only on the server: `clientSecret` and tokens must not be included in code executed in the browser.

```js
import { B24OAuth, EnumAppStatus } from '@bitrix24/b24jssdk'

const $b24 = new B24OAuth(
    {
        applicationToken: '<application_token>',
        userId: 1,
        memberId: '<member_id>',
        accessToken: '<access_token>',
        refreshToken: '<refresh_token>',
        expires: 1745997853,
        expiresIn: 3600,
        scope: 'crm,user_brief',
        domain: 'example.bitrix24.com',
        clientEndpoint: 'https://example.bitrix24.com/rest/',
        serverEndpoint: 'https://oauth.bitrix.info/rest/',
        status: EnumAppStatus.Free
    },
    {
        clientId: '<client_id>',
        clientSecret: '<client_secret>'
    }
)
```

The table below contains fields that must be renamed or found in the correct source. Bitrix24 sends them in lowercase using underscores, while the constructor expects camelCase. Fields `status` and `domain` are not included in the table — they are analyzed separately afterward.

#|
|| **Field from Bitrix24** | **Parameter `B24OAuth`** | **Source** ||
|| `access_token` | `accessToken` | Installation event and token request response ||
|| `refresh_token` | `refreshToken` | Installation event and token request response ||
|| `member_id` | `memberId` | Installation event and token request response ||
|| `client_endpoint` | `clientEndpoint` | Installation event and token request response ||
|| `server_endpoint` | `serverEndpoint` | Installation event and token request response ||
|| `expires_in` | `expiresIn` | Installation event and token request response ||
|| `application_token` | `applicationToken` | Installation event only ||
|| `scope` | `scope` | Installation event and token request response ||
|| `expires` | `expires` | Only [token request response](../../settings/oauth/auto-renewal.md) ||
|| `user_id` | `userId` | Only [token request response](../../settings/oauth/auto-renewal.md) ||
|#

The `domain` field was intentionally omitted from the table. It is named identically in both sources but means different things: in the installation event, it is the Bitrix24 address, whereas in the token request response, it is the authorization server address. The constructor requires the Bitrix24 address.

Please note: a single installation event is not sufficient for the constructor. Its payload does not contain `expires` and `user_id`, and there is no `application_token` in the token request response — mandatory parameters are collected from both sources.

The `expires` parameter is the token expiration moment as a Unix timestamp, and `expiresIn` is the token lifetime. Both values are specified in seconds, not milliseconds.

The `status` parameter is a single-character application plan code sent by Bitrix24. It corresponds to the following `EnumAppStatus` values: `F` — `Free`, `D` — `Demo`, `T` — `Trial`, `P` — `Paid`, `L` — `Local`, `S` — `Subscription`. For a local application, it receives `L`.

The SDK updates tokens automatically: when a method returns an `expired_token` error or when the token expiration date has passed. You must retain the new pair on your side; otherwise, after a restart, the application will retrieve outdated tokens from the data store.

```js
// tokenStore is your token storage: a database, file, or secret manager
$b24.setCallbackRefreshAuth(async ({ b24OAuthParams }) => {
    await tokenStore.save(b24OAuthParams)
})
```

Retain `b24OAuthParams` in its entirety. When updating, the SDK overwrites not only the tokens but also `expires`, `expiresIn`, `scope`, `status`, `clientEndpoint`, and `serverEndpoint`. If you only retain a subset of the fields, the application will retrieve outdated values from the data store after a restart.

If the token update fails, the SDK throws an exception. Its class depends on how exactly the update failed: in the event of an HTTP error from the authorization server, it is `RefreshTokenError`, in all other cases — a regular `Error`, and during automatic updates inside the method call, the error will arrive as `AjaxError`. Catch the exception without binding to a specific class — in any of these cases, the application must be re-authorized. A full description of the parameters and token update scenarios can be found in the [B24OAuth Class Documentation](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/oauth).

## Method Invocation Methods

All calls are made via `$b24.actions.v2`. For methods that have a version in REST API v3, a parallel set of works `$b24.actions.v3` — there is no need to include it; it is sufficient to call the method through this namespace instead of `v2`. The SDK itself does not switch to v3: filter formats differ between versions, so calls are not interchangeable. How [method invocation in v3](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/call-rest-api-ver3) is structured and how [filters differ](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/filtering) between the two versions is described in the SDK documentation.

In the examples below, `$b24` is a connection object from any of the sections above, and `logger` is a logger created alongside it.

#|
|| **Task** | **Action** | **Returns** | **Restrictions** ||
|| One method, one record, or one operation | `call.make()` | `AjaxResult` | One request to Bitrix24 ||
|| Entire list in memory | `callList.make()` | `Result` with an array of records | Collects all pages itself, up to 50 records arrive in one request to Bitrix24 ||
|| Large list in parts | `fetchList.make()` | Asynchronous generator | Read via `for await` loop ||
|| Several different methods in one request | `batch.make()` | `CallBatchResult` with command results | Up to 50 commands ||
|| Bulk creation, update, or deletion | `batchByChunk.make()` | `Result` with an array of records | Splits commands into batches of 50, executed non-atomically ||
|#

Wrappers return data differently. In `call.make()`, the `getData()` method returns a `{ result, time }` object, where `result` is the method result and `time` is the request execution time data. It does not contain the `next` and `total` fields from the REST API response: for paginated traversal, the SDK provides list wrappers. In `callList.make()`, the `getData()` method immediately returns an array of records — there is no need to access `result`.

### List Calls

List wrappers have two parameters that are formally optional but are required almost always. `idKey` is the name of the identifier field as it arrives in the response: paginated traversal works based on this field. By default, `idKey` is equal to `ID` in uppercase, while `crm.item.list` returns `id` in lowercase — without explicit specification, traversal will stop after the first page. `customKeyForResult` is the key name under which the method returns the array of records.

If a method sorts by one field name but returns another, add a third parameter — `cursorIdKey`. This is how `tasks.task.list`: sorting is done by `ID`, and the response contains `id`, so both are needed `idKey: 'id'`, and `cursorIdKey: 'ID'`.

It is useless to pass your own `order` to list wrappers. They always sort by cursor in ascending order and discard the passed value with a warning in the logger. To narrow the selection, use `filter`.

In `filter`, do not use a key with a "greater than" operator on the cursor field — for example, `'>id'` with `idKey: 'id'`. The wrapper writes the next page position into this key and silently overwrites your condition: all records will be returned, and no warning will be issued. Other operators and fields work as usual.

```js
const response = await $b24.actions.v2.callList.make({
    method: 'crm.item.list',
    params: {
        entityTypeId: EnumCrmEntityTypeId.company,
        select: ['id', 'title']
    },
    idKey: 'id',
    customKeyForResult: 'items',
    requestId: 'companies-all'
})

if (response.isSuccess) {
    for (const company of response.getData()) {
        logger.info('Company', { id: company.id, title: company.title })
    }
}
```

If there are too many records to hold in memory, use `fetchList.make()` with the same parameters. It returns an asynchronous generator: each iteration of the `for await` loop returns not a single record, but the next batch. A detailed breakdown with an example can be found in the [SDK documentation](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/fetch-list-rest-api-ver2).

### Batch Calls

`batch.make()` combines up to 50 commands into a single request. Commands are passed as an array of "method and parameters" pairs, and results arrive in the same order:

```js
const response = await $b24.actions.v2.batch.make({
    calls: [
        ['crm.item.get', { entityTypeId: EnumCrmEntityTypeId.company, id: 1 }],
        ['crm.item.get', { entityTypeId: EnumCrmEntityTypeId.company, id: 2 }]
    ],
    options: { requestId: 'companies-batch' }
})

if (response.isSuccess) {
    // Each result contains the content of the result of the corresponding command
    for (const data of response.getData()) {
        logger.info('Company', { title: data.item.title })
    }
}
```

The 50-command limit in `batch.make()` cannot be bypassed: if there are more commands or the array is empty, it throws an exception before the request is sent.

If it is more convenient to access results by name rather than by order, pass an object with named commands — `calls: { company: ['crm.item.get', { ... }] }`. The data will arrive under those same names.

By default, `isHaltOnError` is equal to `true` — the batch stops at the first error. Pass `false` to `options` to receive the results of the remaining commands. Unsuccessful commands are not included in the data, so check for the existence of a result before accessing it.

Calling methods in a loop creates one request per iteration. For a set of similar operations, use `batchByChunk.make()`: it removes the 50-command limit and divides the set into batches itself. The call format is the same, but it does not accept named commands — only an array, because names do not survive the batch splitting. Only successful commands are included in the result, so compare the length of `getData()` with the number of sent commands.

`isHaltOnError` in `batchByChunk.make()` operates within a single batch. A batch with an error will stop, but the remaining batches will still be sent to the server — the entire set of commands is not interrupted by the first error. For a detailed breakdown with an example, see the [SDK Documentation](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/batch-by-chunk-rest-api-ver2).

### Rate Limits

The SDK handles rate limits itself. If different thresholds are required, they are set using the `setRestrictionManagerParams()` method with predefined sets. The method is asynchronous; call it with `await`, otherwise the new thresholds might not be applied in time to the nearest requests. [How to Choose a Set](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/limiters).

The SDK also handles network failures and exceeding SDK limits: it retries the request up to three times with a delay of at least one second. Therefore, such a call does not return an error immediately, and every attempt is visible in the logs. The number of attempts can be checked via the `maxRetries` field of the same method. The SDK does not retry access errors or other errors that a retry will not resolve.

### TypeScript Typing

The data type is specified by a type parameter, but it means different things for different wrappers: for `call.make()`, it refers to the entire content of the `result` field, while for `callList.make()` and `fetchList.make()`, it refers to a single record.

{% note warning "" %}

The `$b24.callMethod()`, `$b24.callListMethod()`, `$b24.fetchListMethod()`, `$b24.callBatch()`, `$b24.callBatchByChunk()` methods and the `LoggerBrowser` class are deprecated. They work in version 2 of the SDK and issue a warning on every call, but they will be removed in version 3.0.0. The mapping between old and new calls is described in the [Migration Guide](https://bitrix-tools.github.io/b24jssdk/docs/getting-started/migration/v1).

In version 3.0.0, the pagination methods for the `call.make()` result — `hasMore()`, `isMore()`, `getTotal()`, `fetchNext()`, and `getNext()` — will also be removed. They are related to the REST API v2 response format; they do not issue any warnings when calling, but `getNext()` via `actions.v3` already throws an exception. To iterate through lists, use `callList.make()` and `fetchList.make()`.

{% endnote %}

## Error Handling

Errors arrive at two levels, and the level depends on the error code, not on whether the request reached its destination:

- some Bitrix24 REST API SDK errors are returned softly — `response.isSuccess` equals `false`, and error texts are provided by `response.getErrorMessages()`. This includes "entity not found" errors and REST API v3 validation errors.
- all other REST API errors, network failures, and SDK errors are returned as a `SdkError` exception with `code` and `status` fields; for HTTP call failures, its descendant is `AjaxError`, which adds request data to `requestInfo`. This is how most frequent portal errors arrive: `ACCESS_DENIED`, `NOT_FOUND`, `INVALID_REQUEST`, `expired_token`, `OVERLOAD_LIMIT`

Handle both levels — checking `isSuccess` inside `try` and catching the exception in `catch`. The model is the same for all three connection classes; the following example continues server-side code using a webhook:

```js
try {
    const response = await $b24.actions.v2.call.make({
        method: 'crm.item.list',
        params: { entityTypeId: EnumCrmEntityTypeId.company },
        requestId: 'companies-list'
    })

    if (response.isSuccess) {
        logger.info('Companies', { items: response.getData().result.items })
    } else {
        logger.error('REST API error', { messages: response.getErrorMessages() })
    }
} catch (error) {
    // Access error, network error, timeout, or an SDK error itself
    logger.error('Request not completed', { error })
}
```

An exception to this model is `fetchList.make()`. The generator cannot terminate partially, so upon failure it does not return a soft error, but throws an exception `SdkError`. Also wrap Loop `for await` in `try/catch`.

Error codes and the exception hierarchy are described in the [B24JsSDK documentation](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/errors).

## Examples

Ready-to-use examples are collected in the [b24sdk-examples](https://github.com/bitrix24/b24sdk-examples/tree/main/js) repository:

- creating a Bitrix24-style interface
- working via incoming webhooks
- authorization via the OAuth protocol
- connecting the UMD version in a browser
- using the SDK on a Node.js server

Walkthroughs of typical tasks with code can be found in the [B24JsSDK documentation examples section](https://bitrix-tools.github.io/b24jssdk/docs/examples).

## Additional Materials

- [Interaction of the Embed With the Messenger Input Field](./iframe-messenger-textarea.md)
- [B24Frame Class](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/frame)
- [B24Hook Class](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/hook)
- [B24OAuth Class](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/oauth)
- [Choosing a Method to Call Methods](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/choosing-the-right-method)
