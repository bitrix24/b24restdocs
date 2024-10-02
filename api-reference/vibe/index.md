# Vibe and Widgets of the "Main Page"

{% note warning "We are still working on the tool" %}

The functionality will be released soon.

{% endnote %}

Vibe is the main page of Bitrix24, which replaces the familiar [News Feed](../log/index.md) and allows for a more personalized interface (details in the [article](https://helpdesk.bitrix24.com/open/22548692/)).

The administrator configures the page in the builder using ready-made blocks and widgets, just like in [Sites](../landing/index.md). In addition to the system widgets of Bitrix24, developers can add their own using the appropriate REST API methods.

{% note info %}

**Widget** - a dynamic element of Vibe that displays data obtained through the corresponding server handler. This allows widgets to be interactive by displaying changing data in response to user actions.

For example, you can sort data in a table, load new data, open a slider with additional interfaces for your solution, etc.

{% endnote %}

Additionally, the Vibe created in your Bitrix24 can be exported as a ready-made solution for publication in the [Market](../../market/index.md).

## How Widgets Work

The front-end of the widget is implemented based on the [Vue templating engine](https://vuejs.org). The widget retrieves data for display by making a request to an external handler.

In fact, when adding your widget to Bitrix24, you need to provide the layout of the template based on Vue, the necessary CSS classes for it, and the address of the handler that will be responsible for passing data to the Vue template.

### Vue Directives

In the widgets for Vibe, we support the following Vue directives and constructs:

- variables `not_var{{ your_variable_name }}`;
- conditional operator `v-if` / `v-else`;
- conditional attributes `:class`, `:disabled`, etc.;
- loop `v-for`;
- expressions, for example `not_var{{ number + 1 }}`;
- handlers `@click` and `v-on:click`.

### Interactive Actions

Custom JS code cannot be added to the widgets, so only the following predefined functions can be used as event handlers for `@click` and `v-on:click`:

- `fetch(?params)` - calls the widget handler to obtain new data, passing the specified parameters `params` to the handler;
- `openApplication(?params)` — opens the slider of your application with the ability to pass arbitrary parameters `params`. In fact, this is a way to call [openApplication](../widgets/open-application.md) from the Vibe widget interface;
- `openPath(url)` — opens a Bitrix24 page in the slider at the specified `url`. In fact, this is a way to call [openPath](../widgets/open-path.md) from the Vibe widget interface.

## How the Widget Works {#anchor-handler}

The most important part of the functionality lies in using `fetch(?params)` and your widget handler.

1. When the widget is placed on the Vibe page, Bitrix24 makes a request to the URL of the widget handler that you specified during widget registration.
2. Your handler returns a certain structure of data defined by you in the form of a JSON string.
3. Vue executes the template code of your widget, substituting the data obtained from the handler in step 2. This forms the initial complete appearance of the widget.
4. If you used the `fetch` function (for example, with parameters `{'action': 'getItems'}`) in your template when clicking on a certain link, Bitrix24 will again call your widget handler, passing the same parameters `{'action': 'getItems'}` in a POST request.
5. Your handler, upon receiving the request and analyzing the input parameters, must respond with a new **complete** set of data and return it again as a JSON string.
6. The newly obtained data will be substituted back into your template.

In this simple way, your widget can change its appearance and displayed data in response to user actions.

## Methods for Working with Widgets

#|
|| **Method** | **Description** ||
|| [landing.repowidget.register](./landing-repowidget-register.md) | Method adds a widget ||
|| [landing.repowidget.unregister](./landing-repowidget-unregister.md) | Method removes a widget ||
|#