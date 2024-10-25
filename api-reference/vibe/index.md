# Widgets and Home Page: Our Vibe

{% note warning "We are still working on the tool" %}

The functionality will be released soon.

{% endnote %}

Vibe is the home page of *Bitrix24*, which replaces the familiar [News Feed](../log/index.md) and allows for a more personalized interface (details in [article](https://helpdesk.bitrix24.com/open/22548692/)).

The administrator configures the page in the builder using ready-made blocks and widgets, just like in [Sites](../landing/index.md). In addition to the system widgets of *Bitrix24*, developers can add their own using the appropriate REST API methods.

{% note info %}

**Widget** — a dynamic element of Vibe that displays data obtained through the corresponding server handler. This allows widgets to be interactive, showing changing data in response to user actions.

For example, you can sort data in a table, load new data, open a slider with additional interface options for your solution, etc.

{% endnote %}

Additionally, the Vibe created on your *Bitrix24* can be exported as a ready-made solution for publication in the [Market](../../market/index.md).

## How Widgets Work

The front-end of the widget is based on the [Vue templating engine](https://vuejs.org). The widget retrieves data for display by making a request to an external handler.

In fact, when adding your widget to *Bitrix24*, you need to provide the Vue template markup, the necessary CSS classes, and the address of the handler that will be responsible for passing data to the Vue template.

### Vue Directives

In the widgets for Vibe, we support the following Vue directives and constructs:

- variables `not_var{{ your_variable_name }}`
- conditional operator `v-if` / `v-else`
- conditional attributes `:class`, `:disabled`, etc.
- loop `v-for`
- expressions, for example, `not_var{{ number + 1 }}`
- handlers `@click` and `v-on:click`

### Interactive Actions

Custom JS code cannot be added to the widgets, so only the following predefined functions can be used as event handlers for `@click` and `v-on:click`:

- `fetch(?params)` — calls the widget handler to obtain new data, passing the specified parameters `params` to the handler;
- `openApplication(?params)` — opens your application's slider with the ability to pass arbitrary parameters `params`. Essentially, this is a way to invoke [openApplication](../widgets/open-application.md) from the Vibe widget interface;
- `openPath(url)` — opens a page of *Bitrix24* in the slider at the specified `url`. Essentially, this is a way to invoke [openPath](../widgets/open-path.md) from the Vibe widget interface.

## How the Widget Works {#anchor-handler}

The most important part of the functionality lies in using `fetch(?params)` and your widget handler.

1. If you did not specify initial data in the parameter \[`WIDGET_PARAMS`\]\[`data`\] when registering the widget, then when the *Bitrix24* administrator adds your widget to the Vibe page or when a regular user opens an already published Vibe page on their account, *Bitrix24* makes a request to the widget handler URL that you specified during widget registration.
2. Your handler returns a certain structure of data defined by you in the form of a JSON string.
3. Vue executes the template code of your widget, substituting the data received from the handler in step 2. This forms the initial complete appearance of the widget.
4. If you used the `fetch` function (for example, with parameters `{'action': 'getItems'}`) in your template when clicking on a certain link, then *Bitrix24* will call your widget handler again, passing the same parameters `{'action': 'getItems'}` in a POST request.
5. Your handler, upon receiving the request and analyzing the incoming parameters, should respond with a new set of data and return it again in the form of a JSON string. This set can be either complete or partial to save traffic and improve the widget's performance.
6. The newly received data will be substituted back into your template.

In this simple way, your widget can change its appearance and displayed data in response to user actions.

## Examples of Vue Syntax for Vibe Widgets

### Localization

Use the standard Vue variable syntax to call `$Bitrix.Loc.getMessage` to display the value of a constant in the visible part of the markup:

```php
<h3 class="w-title">{{ $Bitrix.Loc.getMessage('W_TITLE') }}</h3>
```

Use the Vue "conditional" attribute syntax to assign the value of `$Bitrix.Loc.getMessage` to the desired node attribute:

```php
<input
    type="text"
    class="task-widget__filter"
    :placeholder="$Bitrix.Loc.getMessage('W_FILTER')"
    v-model="taskFilter"
/>
```

### Variables

Use the standard Vue variable syntax to display values in the visible part of the markup:

```php
<td class="task-widget__cell">not_var{{ task.id }}</td>
```

Use the Vue "conditional" attribute syntax to assign the value of a variable to the desired node attribute:

```php
<img :src="user.avatar" class="task-widget__avatar" />
```

Use logical expressions to apply optional classes:

```php
<a href="#" :class="{ 'task-widget__link--disabled': currentPage === 1 }">
```

### Using Variables in openPath

Use backticks to reference Vue variables when forming the desired path:

```php
<tr
    v-for="(task, index) in taskItems"
    :key="task.id"
    @click="openPath(`/company/personal/user/${task.responsibleId}/tasks/task/view/${task.id}/`)"
>
```

Use string concatenation to form the path:

```php
<tr
    v-for="(task, index) in taskItems"
    :key="task.id"
    @click="openPath('/company/personal/user/' + task.responsibleId + '/tasks/task/view/' + task.id)"
>
```

## Methods for Working with Widgets

#|
|| **Method** | **Description** ||
|| [landing.repowidget.register](./landing-repowidget-register.md) | Adds a widget ||
|| [landing.repowidget.unregister](./landing-repowidget-unregister.md) | Removes a widget ||
|| [landing.repowidget.getlist](./landing-repowidget-get-list.md) | Returns a list of widgets ||
|| [landing.repowidget.debug](./landing-repowidget-debug.md) | Enables debug mode ||
|#