# App Configurations in BX24.JS: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Two objects are used to work with app configurations:

- `BX24.userOption` — works with the current user's configurations. Setting a user configuration value takes effect immediately
- `BX24.appOption` — works with general app configurations. App configurations may require a completion handler, the `callback` parameter in [BX24.appOption.set](./bx24-app-option-set.md)

> Quick navigation: [All Methods](#all-methods)

## Operating Conditions

- The `BX24.userOption` and `BX24.appOption` functions are available in the app after the library is initialized via the [BX24.init](../system-functions/bx24-init.md) method
- `BX24.userOption` reads and retains the current user's configurations
- `BX24.appOption` reads and retains the general configurations of the current app
- Saving general app configurations is only available to users with app management permissions. Before calling [BX24.appOption.set](./bx24-app-option-set.md), verify permissions using the [BX24.isAdmin](../additional-functions/bx24-is-admin.md) method

## How to Choose a Configuration Object

#|
|| **If needed** | **Use** ||
|| Save setting only for the current user | [BX24.userOption.set](./bx24-user-option-set.md) ||
|| Read current user setting | [BX24.userOption.get](./bx24-user-option-get.md) ||
|| Save global application setting | [BX24.appOption.set](./bx24-app-option-set.md) ||
|| Read global application setting | [BX24.appOption.get](./bx24-app-option-get.md) ||
|#

## Getting Started

1. Initialize the library using the [BX24.init](../system-functions/bx24-init.md) method
2. If an app configuration is required, verify administrator permissions using the [BX24.isAdmin](../additional-functions/bx24-is-admin.md) method
3. Retain the value using the `set` method
4. Retrieve the value using the `get` method

## How Operations Are Performed

#|
|| **Object** | **Write** | **Read** | **Unretained Code** ||
|| `BX24.userOption` | `set` immediately updates the current user's configuration | `get` synchronously returns the value | `undefined` ||
|| `BX24.appOption` | `set` retains the configuration asynchronously and calls `callback`. Application management permissions are required to write | `get` synchronously returns the value loaded during initialization | `undefined` ||
|#

## Example

Retain a configuration for the current user and read its value:

```js
BX24.init(() => {
    BX24.userOption.set('theme', 'dark');

    console.log(BX24.userOption.get('theme')); // returns dark
    console.log(BX24.userOption.get('unknown')); // returns undefined
});
```

Retain a general application configuration and process the write result:

```js
BX24.init(() => {
    if (!BX24.isAdmin()) {
        return;
    }

    BX24.appOption.set('theme', 'dark', (options) => {
        console.log(options.theme); // returns dark
    });
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Overview of Methods {#all-methods}

#|
|| **Method** | **Description** ||
|| [BX24.userOption.set](./bx24-user-option-set.md) | Sets settings for the current user ||
|| [BX24.userOption.get](./bx24-user-option-get.md) | Returns settings for the current user ||
|| [BX24.appOption.set](./bx24-app-option-set.md) | Sets global settings for the current application ||
|| [BX24.appOption.get](./bx24-app-option-get.md) | Returns setting by its code ||
|#
