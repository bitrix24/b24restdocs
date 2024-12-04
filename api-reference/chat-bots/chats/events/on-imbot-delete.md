# Deleting the ONIMBOTDELETE Chatbot

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet writing standards
- parameter types are not specified
- parameter requirements are not indicated

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONIMBOTDELETE` event is for deleting a chatbot.

{% note warning %}

The fields described below are the contents of the [data] field in the event. The authorization data in the [auth] key contains the data of the user who initiated the event. To obtain the bot's authorization data, you need to use [data][BOT][__BOT_CODE__].

{% endnote %}

#|
|| **Field** | **Description** | **Revision** ||
|| **BOT_ID** 
[`unknown`](../../../data-types.md) | Chatbot identifier | ||
|| **BOT_CODE** 
[`unknown`](../../../data-types.md) | Chatbot code | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    [BOT_ID] => 39
    [BOT_CODE] => giphy
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}