# Interaction with the CRM Card

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- article about reloadData https://bitrix24.com/rest_help/application_embedding/application_embedding/js_interface.php needs to be thoroughly reviewed
- edits needed to meet writing standards
- missing parameters or fields
- parameter types not specified
- required parameters not indicated
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

The CRM card has an additional command `reloadData`, which will refresh the CRM interface data. This command is solely for transmitting data to the CRM card; therefore, until the card is actually saved by the user, the values will not be stored in the database.

```js
BX24.placement.call('reloadData', function(){console.log('reload call')});
```

The js interface is solely for transmitting data to the form. Until the form is saved by the user, the value will not be stored in the database.