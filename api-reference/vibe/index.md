# Widgets and Start page: the Vibe

The Vibe is the start page of *Bitrix24*, replacing the familiar [Feed](../log/index.md) and allowing for a more personalized interface (details in [article](https://helpdesk.bitrix24.com/open/22548692)).

The administrator configures the page in the builder using ready-made blocks and widgets, similar to how it's done in [Sites](../landing/index.md). In addition to the system widgets of *Bitrix24*, developers can add their own using the appropriate REST API methods.

{% note info %}

**Widget** — a dynamic element of the Vibe that displays data obtained through the corresponding server handler. This allows widgets to be interactive, showing changing data in response to user actions.

For example, you can sort data in a table, load new data, open a slider with additional interface for your solution, etc.

{% endnote %}

Additionally, the Vibe created on your *Bitrix24* can be exported as a ready-made solution for publication in the [Market](../../market/index.md).

## How Widgets Work

The front-end of the widget is implemented based on the [Vue templating engine](https://vuejs.org). The widget retrieves data for display by making a request to an external handler.

In fact, when adding your widget to *Bitrix24*, you need to provide the Vue template markup, the necessary CSS classes, and the address of the handler that will be responsible for passing data to the Vue template.

### Vue Directives

In the widgets for the Vibe, we support the following Vue directives and constructs:

- variables `not_var{{ your_variable_name }}`
- conditional operator `v-if` / `v-else`
- conditional attributes `:class`, `:disabled`, etc.
- loop `v-for`
- expressions, for example, `not_var{{ number + 1 }}`
- handlers `@click` and `v-on:click`

### Interactive Actions

Custom JS code cannot be added to the widgets, so only the following predefined functions can be used as event handlers for `@click` and `v-on:click`:

- `fetch(?params)` — calls the widget handler to obtain new data, passing the specified parameters `params` to the handler;
- `openApplication(?params)` — opens the slider of your application with the ability to pass arbitrary parameters `params`. Essentially, this is a way to call [openApplication](../widgets/open-application.md) from the Vibe widget interface;
- `openPath(url)` — opens the *Bitrix24* page at the specified `url` in the slider. Essentially, this is a way to call [openPath](../widgets/open-path.md) from the Vibe widget interface.

## How the Widget Works {#anchor-handler}

The most important part of the functionality lies in using `fetch(?params)` and your widget handler.

1. When a Bitrix24 administrator adds your widget to the Vibe page or when a regular user opens an already published Vibe page on their account, Bitrix24 makes a request to the widget handler URL you specified during widget registration.
2. Your handler returns a specific data structure defined by you in the form of a JSON string.
3. Vue executes the template code of your widget, substituting the data obtained from the handler in step 2. This forms the initial complete appearance of the widget.
4. If you used the `fetch` function (for example, with parameters `{'action': 'getItems'}`) in your template when clicking on a certain link, *Bitrix24* will again call your widget handler, passing those same parameters `{'action': 'getItems'}` in a POST request.
5. Your handler, upon receiving the request and analyzing the input parameters, should respond with a new set of data and return it again as a JSON string. This set can be either complete or partial to save traffic and improve the widget's performance.
6. The newly obtained data will be substituted back into your template.

In this straightforward manner, your widget can change its appearance and displayed data in response to user actions.

## Examples of Vue Syntax for Vibe Widgets

### Localization

Use the standard syntax for accessing Vue variables to call `$Bitrix.Loc.getMessage` to display the value of a constant in the visible part of the markup:

```php
<h3 class="w-title">{{ $Bitrix.Loc.getMessage('W_TITLE') }}</h3>
```

Use the Vue conditional attribute syntax to assign the value of `$Bitrix.Loc.getMessage` to the desired node attribute:

```php
<input
    type="text"
    class="task-widget__filter"
    :placeholder="$Bitrix.Loc.getMessage('W_FILTER')"
    v-model="taskFilter"
/>
```

### Variables

Use the standard syntax for accessing Vue variables to display values in the visible part of the markup:

```php
<td class="task-widget__cell">not_var{{ task.id }}</td>
```

Use the Vue conditional attribute syntax to assign the value of a variable to the desired node attribute:

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
|| [landing.repowidget.register](./landing-repowidget-register.md) | Registers a widget ||
|| [landing.repowidget.unregister](./landing-repowidget-unregister.md) | Unregisters a widget ||
|| [landing.repowidget.getlist](./landing-repowidget-get-list.md) | Returns a list of widgets ||
|| [landing.repowidget.debug](./landing-repowidget-debug.md) | Enables debug mode ||
|#