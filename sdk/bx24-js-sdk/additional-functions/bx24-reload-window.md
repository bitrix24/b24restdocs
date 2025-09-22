# Reload the page BX24.reloadWindow

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

```js
void BX24.reloadWindow()
```

The method `BX24.reloadWindow` reloads the page with the application and works only when the application is opened in a separate window. The method does not work when the application is opened in a slider or as a widget.