# BX24.js: Library Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

BX24.js is a JavaScript library for applications embedded within the Bitrix24 interface. It allows you to call Bitrix24 methods from the client side of your application and automatically adds OAuth 2.0 authorization data.

The library also links the application interface with the Bitrix24 interface. Using it, you can manage the frame size, open standard Bitrix24 dialogs and pages, retrieve runtime environment data, and store user and application configurations.

> Quick links: [BX24.js Page Overview](#all-pages)

## Getting Started

1. Include the library on the application page:

   ```html
   <script src="//api.bitrix24.com/api/v1/"></script>
   ```

2. Wait for the library to initialize using [BX24.init](./system-functions/bx24-init.md)
3. Select a function group from the [BX24.js Page Overview](#all-pages) table
4. Call the required function and handle the result in your application's client-side code

For example, this is how an application finds out who opened it — via the [user.current](../../api-reference/user/user-current.md) method. For this method, the application must have the `user`, `user_brief`, or `user_basic` scope:

```html
<script src="//api.bitrix24.com/api/v1/"></script>
<script>
    BX24.init(function () {
        BX24.callMethod('user.current', {}, function (result) {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.log('Current user:', result.data().NAME, result.data().LAST_NAME);
        });
    });
</script>
```

## Integration With Other Tools

**Embedding Locations.** An application can be opened in a selected location within the Bitrix24 interface, for example, in a tab of a CRM detail form. To do this, register an embedding location handler with the [placement.bind](../../api-reference/widgets/placement-bind.md) method: pass the embedding location code in the `PLACEMENT` parameter and the address of the application page in the `HANDLER` parameter — Bitrix24 opens this page in the embedding location. This method is available to an administrator.

**UI Kit.** While BX24.js handles the interaction between the application and Bitrix24, the [Bitrix24 UI Kit](../../api-reference/widgets/ui-kit/index.md) helps you build an interface using ready-made components, design tokens, and icons.

## Key Considerations

- For external applications and webhooks, select a different library in the [SDK Overview](../index.md)
- The required scope depends on the method being called. See scope values in the [Permissions](../../api-reference/scopes/permissions.md) guide
- Data access depends on the permissions of the user on whose behalf the application performs the request
- The request rate to Bitrix24 is limited by [REST API Limits](../../settings/performance/limits.md), and a [BX24.callBatch](./how-to-call-rest-methods/bx24-call-batch.md) batch holds no more than 50 commands
- BX24.js functions do not return a Promise, so `.then()` after a call does not work. The result of a REST method and the selection in a system dialog arrive in a callback function that you pass as a separate parameter
- After initialization, functions return environment data and retained configurations synchronously, without a callback: for example, [BX24.getLang](./additional-functions/bx24-get-lang.md), [BX24.getAuth](./system-functions/bx24-get-auth.md), and [BX24.appOption.get](./options/bx24-app-option-get.md) work this way
- The handler of a single REST method call receives an [ajaxResult](./how-to-call-rest-methods/bx24-call-method.md#ajax-result) object: `result.data()` returns the data, and `result.error()` returns the error object, or `undefined` if there is no error. In a batch call, check the error in the result of each command. The [Error Handling](./how-to-call-rest-methods/index.md#errors) section describes which failures do not reach the callback

## BX24.js Page Overview {#all-pages}

#|
|| **If you need to** | **Open the page** ||
|| Call one or more Bitrix24 methods from the client side of the application | [Calling REST methods](./how-to-call-rest-methods/index.md) ||
|| Initialize the library, handle the first run, or get authorization data | [Initialization and authorization](./system-functions/index.md) ||
|| Save user settings or general application settings | [Application settings](./options/index.md) ||
|| Open the standard dialog for selecting users, permissions, or CRM items | [System dialogs](./system-dialogues/index.md) ||
|| Manage the application window, navigation, page events, and call context | [Interface, navigation, and context](./additional-functions/index.md) ||
|#