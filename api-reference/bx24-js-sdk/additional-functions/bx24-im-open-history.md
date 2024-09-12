# Open History Window BX24.im.openHistory

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

```js
void BX24.im.openHistory(dialogId)
```

The method `BX24.im.openHistory` opens the history window.

## Function Parameters

#|
|| **Parameter** | **Description** ||
|| **dialogId**
[`unknown`](../../data-types.md) | Identifier of the dialog:
- **userId** or **chatXXX** - chat, where XXX is the chat identifier, which can simply be a number.
- **imol\|XXXX** - open line, where XXX is the session number of the open line. ||
|#