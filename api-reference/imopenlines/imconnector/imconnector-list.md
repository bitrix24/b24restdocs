# Get the list of connectors imconnector.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- from Sergey's file: please note that this returns the complete list of connected connectors in the current B24
- parameters are not specified
- required parameters are not indicated
- examples in other languages are missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method retrieves the list of connectors.

## Example

```js
BX24.callMethod('imconnector.list', {}, function(result) {
    if(result.error())
    {
        console.error(result.error().ex);
    }
    else
    {
        console.log(result.data());
    }
});
```
{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on success

A list of connectors with their names.

```json
{
    "livechat": "Live Chat",
    "whatsappbytwilio": "WhatsApp",
    "avito": "Avito",
    "viber": "Viber",
    "telegrambot": "Telegram",
    "imessage": "Apple Messages for Business",
    "vkgroup": "VK",
    "ok": "Classmates",
    "facebook": "Facebook*: Messages",
    "facebookcomments": "Facebook*: Comments",
    "fbinstagramdirect": "Instagram* Direct",
    "network": "Bitrix24.Network",
    "notifications": "Bitrix24 SMS and WhatsApp",
    "whatsappbyedna": "Edna.com WhatsApp",
    "newcustomconnector": "new super Connector"
}
```

### Possible error codes

#|
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The current user does not have access || 
|#