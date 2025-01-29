# Interaction with CRM Card

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Article about reloadData https://dev.1c-bitrix.com/rest_help/application_embedding/application_embedding/js_interface.php needs to be thoroughly reviewed.
- Edits are needed to meet writing standards.
- Parameters or fields are missing.
- Parameter types are not specified.
- Required parameters are not indicated.
- Examples are missing.
- Success response is absent.
- Error response is absent.

{% endnote %}

{% endif %}

In the CRM card, an additional command `reloadData` is available, which will refresh the CRM interface data. This command is solely for transmitting data to the CRM card; therefore, until the card is actually saved by the user, the values will not be stored in the database.

```js
BX24.placement.call('reloadData', function(){console.log('reload call')});
```

The js interface is exclusively for sending data to the form. Until the form is saved by the user, the value will not be stored in the database.

## Continue Your Exploration

- [{#T}](bx24-placement-call.md)