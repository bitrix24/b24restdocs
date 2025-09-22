# Get the language identifier of the current account BX24.getLang

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- success response is missing
- error response is missing
- are there exactly three language options? maybe there are more now?

{% endnote %}

{% endif %}

```js
String BX24.getLang()
```

The method `BX24.getLang` returns the language identifier of the current account. Options: ru|en|de.

The current language is determined during the initialization of the js library; if called before [BX24.init](../system-functions/bx24-init.md), the function will return an empty value.

## Example

Loading language messages from an external js file:

```js
BX24.init(function()
{
    BX24.loadScript('lang/' + BX24.getLang() + '.js', function()
    {
        alert('Loading completed!');
    });
});
```

{% include [Examples note](../../../_includes/examples.md) %}