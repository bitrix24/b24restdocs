# Push&Pull in the Browser

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The built-in `BX.PullClient` keeps a connection to the real-time servers and delivers events sent by the application's server side with [pull.application.event.add](./push-and-pull/pull-application-event-add.md) to the browser. The application interface updates immediately, without polling the server or reloading the page.

The client is added to an existing application page. If the application runs outside the Bitrix24 interface and the built-in client is not enough, you will have to maintain the connection yourself — this is described in the [{#T}](./custom-push-and-pull-client.md) article.

{% note info "" %}

The client works only in the context of an [application](../app-installation/index.md): it requests the connection configuration with the [pull.application.config.get](./push-and-pull/pull-application-config-get.md) method, which requires an OAuth token and the `pull` scope. A webhook does not create such a context.

{% endnote %}

## Before You Start

- an installed [application](../app-installation/index.md) with an interface
- the [`pull`](../../api-reference/scopes/permissions.md) scope

The client needs two libraries from `api.bitrix24.com`: `api/v1/` provides the `BX24` object for REST calls, and `api/v1/pull/` provides the `BX.PullClient` constructor.

## How to Connect the Client

1. Include the `api/v1/` and `api/v1/pull/` libraries in the `<head>` of the page
2. Wait until `BX24` is ready: perform the remaining steps inside `BX24.init`
3. Retrieve the user ID with the [user.current](../../api-reference/user/user-current.md) method
4. Create the client with `new BX.PullClient()` and pass the [parameters](#params)
5. Subscribe to events with the `subscribe` method
6. Start the connection with the `start` method

```html
<!DOCTYPE html>
<html>
<head>
	<title>Bitrix24 application with Push & Pull</title>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<script src="//api.bitrix24.com/api/v1/"></script>
	<script src="//api.bitrix24.com/api/v1/pull/"></script>
</head>
<body>
	<script>
		BX24.init(function () {
			BX24.callMethod('user.current', {}, function (result) {
				if (result.error()) {
					console.error(result.error().ex);
					return;
				}

				window.appPullClient = new BX.PullClient({
					restApplication: 'my_app_pull',
					restClient: BX24,
					userId: Number(result.data().ID)
				});

				window.appPullClient.subscribe({
					moduleId: 'application',
					callback: function (data) {
						console.warn(data); // {command: '...', params: {...}, extra: {...}}
					}
				});

				window.appPullClient.start();
			});
		});
	</script>
</body>
</html>
```

To make sure the client receives events, send an event from the server side with the [pull.application.event.add](./push-and-pull/pull-application-event-add.md) method. The handler prints an object with the `command`, `params`, and `extra` fields to the browser console.

## BX.PullClient Parameters {#params}

#|
|| **Parameter** | **Description** ||
|| `restApplication` | A string identifier of the application. When it is set, the client requests the configuration with the [pull.application.config.get](./push-and-pull/pull-application-config-get.md) method and connects to the application channels. The client also uses this value to retain the connection state in the browser, so set a stable string — one per application ||
|| `restClient` | The object through which the client calls REST methods. In an application, pass `BX24` from the included library. Without this parameter, the client creates its own object, which authorizes with a Bitrix24 session ID — and there is no such session on an application page ||
|| `userId` | The ID of the current user. On an application page, the client has no way to retrieve it on its own, so the value is passed explicitly — in the example it is returned by [user.current](../../api-reference/user/user-current.md) ||
|#

## Subscribing to Events {#subscribe}

The `subscribe` method registers a handler and returns a function that removes it.

#|
|| **Field** | **Description** ||
|| `moduleId` | The module whose events the application needs. Events from the application channel have `application` as their module ID — this is the value of the `MODULE_ID` parameter of the [pull.application.event.add](./push-and-pull/pull-application-event-add.md) method ||
|| `callback` | The handler function. What it receives depends on whether the `command` field is set ||
|| `command` | Optional. The command the handler is subscribed to — the value of the `COMMAND` parameter of the [pull.application.event.add](./push-and-pull/pull-application-event-add.md) method. Without it, the handler receives all commands of the module ||
|| `type` | Optional. The event source, `server` by default — events sent by the server side. For application events, there is no need to change it ||
|#

The `command` field determines the form in which the handler receives the data:

- **without `command`** — the event arrives as a whole: `callback(data, info)`, where `data` contains `command`, `params`, and `extra`
- **with `command`** — the same data arrives unpacked: `callback(params, extra, command, info)`

In both forms, the last value the handler receives is `info` with the `type` and `moduleId` fields.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./custom-push-and-pull-client.md)
- [{#T}](./push-and-pull/pull-application-config-get.md)
- [{#T}](./push-and-pull/pull-application-event-add.md)