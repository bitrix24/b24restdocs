# Vibe: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The Vibe is a Bitrix24 page that replaces the familiar [Feed](../log/index.md) and allows for a more personalized interface.

The administrator configures the page in the builder using ready-made blocks and widgets, similar to how it's done in [Sites](../landing/index.md). In addition to the system widgets of Bitrix24, developers can add their own using the `landing.repowidget.*` methods.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Start page: the Vibe. Select and configure a template](https://helpdesk.bitrix24.com/open/22548692/)

{% note info "" %}

**Widget** — a dynamic element of the Vibe that displays data obtained through the corresponding server handler. This allows widgets to be interactive, showing changing data in response to user actions.

For example, you can sort data in a table, load new data, or open a slider with additional interface for your solution.

{% endnote %}

Additionally, the Vibe created on your Bitrix24 can be exported as a ready-made solution for publication in the [Market](../../market/index.md).

## Getting Started

1. Prepare the widget markup in Vue and a handler — an external script that returns the data for the template as a JSON string.
2. Register the widget using the [landing.repowidget.register](./landing-repowidget-register.md) method. Pass the unique widget code, markup, styles, and handler address to it.
3. Check the list of your widgets using the [landing.repowidget.getlist](./landing-repowidget-get-list.md) method.
4. Enable debug mode using the [landing.repowidget.debug](./landing-repowidget-debug.md) method so that the widget outputs details of data loading errors to the browser console.
5. Add the widget to the Vibe page in the Bitrix24 builder — this is done by the administrator.
6. Remove a widget you no longer need using the [landing.repowidget.unregister](./landing-repowidget-unregister.md) method.

{% note info "" %}

Register widgets from an application: the widget is bound to it by the application code, and Bitrix24 calls the handler on behalf of that application. A widget registered via a webhook is not bound to an application. Its handler is not called, and the widget displays a data loading error.

{% endnote %}

## How Widgets Work

The widget interface is implemented based on the [Vue templating engine](https://vuejs.org).

### Vue Directives

The following Vue directives and constructs are supported in the widgets for the Vibe:

- variables `not_var{{ your_variable_name }}`
- conditional operator `v-if` / `v-else`
- conditional attributes `:class`, `:disabled`, and others
- loop `v-for`
- expressions, for example, `not_var{{ number + 1 }}`
- handlers `@click` and `v-on:click`

### Interactive Actions

Custom JS code cannot be used in the widgets. Only predefined functions can handle the `@click` and `v-on:click` events:

- `fetch(?params)` — calls the widget handler for new data and passes the `params` parameters to it
- `openApplication(?params)` — opens the slider of your application and passes arbitrary `params` parameters to it. This is a way to call [openApplication](../widgets/bx24-widget-methods.md) from the Vibe widget interface
- `openPath(url)` — opens the Bitrix24 page at the specified `url` in the slider. This is a way to call [openPath](../widgets/bx24-widget-methods.md) from the Vibe widget interface

## How the Widget Works {#anchor-handler}

The main part of the widget's operation is built on the `fetch(?params)` function and the widget handler.

1. The administrator adds your widget to the Vibe page, and a user opens the already published page. In both cases, Bitrix24 makes a request to the handler URL you specified during widget registration.
2. The handler returns the data as a JSON string. You define the data structure yourself.
3. Vue executes the template code of your widget, substituting the data obtained from the handler in step 2. This forms the initial complete appearance of the widget.
4. If you used the `fetch` function in the template — for example, with the parameters `{'action': 'getItems'}` — then, when a link is clicked, Bitrix24 calls your widget handler again and passes those same parameters `{'action': 'getItems'}` in a POST request.
5. The handler analyzes the input parameters, forms a new set of data, and returns it as a JSON string.
6. The newly obtained data will be substituted back into your template.

This is how the widget changes its appearance and displayed data in response to user actions.

### Handler Request and Response {#handler-request}

Bitrix24 sends the handler a POST request with form fields in the `application/x-www-form-urlencoded` format, not a JSON body. The request contains the parameters from the `fetch(params)` call and the `auth` array with the application's authorization on behalf of the current user. With this authorization, the handler can call REST API methods. When the widget is first displayed, there are no parameters, and only `auth` is sent.

The handler must respond with status 200 and return a non-empty JSON string of up to 1 MB. The connection to the handler must not stay idle for more than five seconds. Bitrix24 does not send requests to loopback or local network addresses.

Example response from a task list handler:

```json
{
    "title": "Department tasks",
    "items": [
        { "id": 12, "name": "Prepare the report" },
        { "id": 15, "name": "Call the client" }
    ]
}
```

Top-level keys become template variables. On a repeated `fetch` call, the handler can return only some of the keys, and only those are updated. The widget ignores new keys that were not present in the first response, so pass all template keys in the first response.

```html
<div class="w-container">
    <h3>not_var{{ title }}</h3>
    <ul>
        <li v-for="item in items" :key="item.id">not_var{{ item.name }}</li>
    </ul>
    <a href="#" @click="fetch({'action': 'getItems'})">Refresh</a>
</div>
```

If the handler is unavailable, responds with a different status, or returns an empty body, invalid JSON, or JSON with the `error` key, the widget displays the message "Error loading data". The user does not see the text from `error`. To display your own message, pass it as a regular data key and output it in the template.

## Examples of Vue Syntax for Vibe Widgets

### Localization

Use the standard syntax for accessing Vue variables to call `$Bitrix.Loc.getMessage` to display the value of a constant in the visible part of the markup:

```html
<h3 class="w-title">{{ $Bitrix.Loc.getMessage('W_TITLE') }}</h3>
```

Use the Vue conditional attribute syntax to assign the value of `$Bitrix.Loc.getMessage` to the desired node attribute:

```html
<input
    type="text"
    class="task-widget__filter"
    :placeholder="$Bitrix.Loc.getMessage('W_FILTER')"
    v-model="taskFilter"
/>
```

### Variables

Use the standard syntax for accessing Vue variables to display values in the visible part of the markup:

```html
<td class="task-widget__cell">not_var{{ task.id }}</td>
```

Use the Vue conditional attribute syntax to assign the value of a variable to the desired node attribute:

```html
<img :src="user.avatar" class="task-widget__avatar" />
```

Use logical expressions to apply optional classes:

```html
<a href="#" :class="{ 'task-widget__link--disabled': currentPage === 1 }">
```

### Using Variables in openPath

Use a template literal in backticks to reference Vue variables when forming the desired path:

```html
<tr
    v-for="(task, index) in taskItems"
    :key="task.id"
    @click="openPath(`/company/personal/user/${task.responsibleId}/tasks/task/view/${task.id}/`)"
>
```

Use string concatenation to form the path:

```html
<tr
    v-for="(task, index) in taskItems"
    :key="task.id"
    @click="openPath('/company/personal/user/' + task.responsibleId + '/tasks/task/view/' + task.id)"
>
```

## Overview of Methods {#all-methods}

> Scope: [`landing`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [landing.repowidget.register](./landing-repowidget-register.md) | Registers a widget ||
|| [landing.repowidget.getlist](./landing-repowidget-get-list.md) | Returns a list of widgets ||
|| [landing.repowidget.unregister](./landing-repowidget-unregister.md) | Unregisters a widget ||
|| [landing.repowidget.debug](./landing-repowidget-debug.md) | Enables or disables debug mode ||
|#