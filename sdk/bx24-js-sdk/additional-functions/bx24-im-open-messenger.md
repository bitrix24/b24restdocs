# Open Messenger Window BX24.im.openMessenger

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response not provided
- error response not provided
- links to pages not yet created (imopenlines.network.join) are missing

{% endnote %}

{% endif %}

The method `BX24.im.openMessenger` opens the messenger window.

## Function Parameters

#|
|| **Parameter** | **Description** ||
|| **dialogId**
[`unknown`](../../../api-reference/data-types.md) | Identifier of the dialog:
- **userId** or **chatXXX** - chat, where XXX is the chat identifier, which can simply be a number.
- **sgXXX** - group chat, where XXX is the social network group number (the chat must be enabled in this group).
- **imol|XXXX** - open line, where XXX is the code obtained via the Rest method imopenlines.network.join.

If nothing is passed, the chat interface will open with the last opened dialog. ||
|#