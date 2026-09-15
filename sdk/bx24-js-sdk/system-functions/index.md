# Initialization and Authorization in BX24.js: Feature Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The initialization and authorization functions of the BX24.js library prepare an embedded application for operation and supply its code with authorization data.

{% note info "" %}

The functions in this section work only inside the frame of an [application](../../../settings/app-installation/index.md).

{% endnote %}

> Quick Navigation: [All Functions](#all-methods)

## Getting Started

1. Connect the BX24.js library to the application page — the [Library Overview](../index.md) describes how to do this.
2. Register a [BX24.init](./bx24-init.md) handler for the regular operation of the application and a [BX24.install](./bx24-install.md) handler for the first launch for the current user. Register the first launch handler as the page loads: registered any later, it does not make it into the chain.
3. Complete the setup by calling [BX24.installFinish](./bx24-install-finish.md) if the application has an installation page or a first launch handler — the [Installation Stages](./bx24-install-finish.md#stages) describe what the call does at each stage.
4. Retrieve the authorization data via [BX24.getAuth](./bx24-get-auth.md) if you need to pass it to your own server.

The `BX24.install` chain runs before the `BX24.init` handlers. Within each family, the handlers run in the order they were registered. If there are no first launch handlers, or the user is not launching the application for the first time, the `BX24.init` handlers run right away.

```js
// register BX24.install as the page loads: any later, it does not make it into the chain
BX24.install(function() {
    // first launch for the current user: configure the application for them
    BX24.installFinish();
});

BX24.init(function() {
    // regular operation of the application
});
```

The functions require no scope of their own: they do not call the REST API. [Permissions and Scope](../../../api-reference/scopes/index.md) are checked in the Bitrix24 methods you call after initialization.

## Authorization Data {#auth-data}

[BX24.getAuth](./bx24-get-auth.md) and [BX24.refreshAuth](./bx24-refresh-auth.md) supply an object with the same structure of five fields: `getAuth` returns it synchronously, while `refreshAuth` passes it to a handler. The composition and types of the fields are described in [Returned Data of BX24.getAuth](./bx24-get-auth.md#returns).

Refreshing the token manually is usually unnecessary — [BX24.refreshAuth](./bx24-refresh-auth.md) covers the cases where it is still required. A server you have passed the tokens to renews the authorization differently: [Automatic Authorization Renewal](../../../settings/oauth/auto-renewal.md).

## Error Handling {#errors}

The functions in this section have no REST error codes, and a failure shows up in two ways.

**Silent failure.** The function does nothing and gives no notice: before the library is ready, [BX24.refreshAuth](./bx24-refresh-auth.md) sends no request, and [BX24.getAuth](./bx24-get-auth.md) returns `false` instead of an object. An uncalled [BX24.installFinish](./bx24-install-finish.md) halts the first launch chain just as quietly.

**Noisy failure.** The developer sees an error in the console or a browser `alert`:

- `Unable to initialize Bitrix24 JS library!` — the page is open outside the application frame
- `BX24 is not defined` — the library is not connected. The browser produces the message text, so it differs: Safari writes `Can't find variable: BX24`
- `Installation failed!` — an unhandled synchronous exception in the [BX24.install](./bx24-install.md) handler
- `Unable to get new token! Reload page, please!` — Bitrix24 could not issue a new token in response to the [BX24.refreshAuth](./bx24-refresh-auth.md) request

The "Error Handling" sections on the function pages cover what to do in each situation.

## Relationship with Other Objects

**Application.** Authorization data is issued to an application registered in Bitrix24. The `member_id` and `domain` identifiers from [BX24.getAuth](./bx24-get-auth.md) show which Bitrix24 the application runs in — they are retained on your side to link a user to an installation. The installation procedure is described in the [Application Installation](../../../settings/app-installation/index.md) section.

**Bitrix24 Methods.** After initialization, the library substitutes the token into requests on its own, so passing `auth` separately is not required. The [Calling REST Methods](../how-to-call-rest-methods/index.md) section helps you call methods from the client side of the application.

**App Configurations.** The values that an application retains in Bitrix24 between launches are read and written by the functions of the [App Configurations](../options/index.md) section. Inside the [BX24.install](./bx24-install.md) first launch handler, these functions are not connected yet — at that stage, configurations are read and written with Bitrix24 methods.

**Interface and Context.** Library readiness is not the same as the readiness of the page's DOM structure: [BX24.ready](../additional-functions/bx24-ready.md) and [BX24.isReady](../additional-functions/bx24-is-ready.md) from the [Interface, Navigation, and Context](../additional-functions/index.md) section are responsible for that.

**Bitrix24 and User Data.** The [BX24.getDomain](../additional-functions/bx24-get-domain.md) function also returns the Bitrix24 address from the `domain` field. The authorization object carries no permission flag — [BX24.isAdmin](../additional-functions/bx24-is-admin.md) helps you find out whether the current user is an administrator.

## Feature Overview {#all-methods}

### Initialization and First Launch

#|
|| **Function** | **Description** ||
|| [BX24.init](./bx24-init.md) | Registers an event handler for "library ready for use" ||
|| [BX24.install](./bx24-install.md) | Registers a handler for the first launch of the application for the current user ||
|| [BX24.installFinish](./bx24-install-finish.md) | Reports the completion of the installer or the application setup wizard ||
|#

None of these functions return data. A handler is required for `BX24.init` and `BX24.install`, while `BX24.installFinish` does not accept one.

### Application Authorization

#|
|| **Function** | **Description** ||
|| [BX24.getAuth](./bx24-get-auth.md) | Retrieves the current authorization data via OAuth 2.0 ||
|| [BX24.refreshAuth](./bx24-refresh-auth.md) | Forcefully refreshes the authorization data ||
|#

Both functions work after the library has been initialized.
