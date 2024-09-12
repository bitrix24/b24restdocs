# ONIMBOTDELETE Chatbot Deletion

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- parameter requirements are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONIMBOTDELETE` is for deleting a chatbot.

{% note warning %}

The fields described below are the contents of the [data] field in the event. The authorization data in the [auth] key contains the data of the user who initiated the event. To obtain the authorization data of the bot, you need to use [data][BOT][__BOT_CODE__].

{% endnote %}

#|
|| **Field** | **Description** | **Revision** ||
|| **BOT_ID** 
[`unknown`](../../../data-types.md) | Chatbot identifier | ||
|| **BOT_CODE** 
[`unknown`](../../../data-types.md) | Chatbot code | ||
|#

## Examples

```js
[BOT_ID] => 39
[BOT_CODE] => giphy
```

{% include [Example Note](../../../../_includes/examples.md) %}