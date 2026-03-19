# Open Channels Dialogs

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- from Sergey’s file: dialogs/sessions - what it is, how they work

{% endnote %}

{% endif %}

#|
|| **Method** | **Description** ||
|| [imopenlines.crm.lead.create](./imopenlines-crm-lead-create.md) | Creates a CRM lead from an open channel chat. ||
|| [imopenlines.dialog.get](./imopenlines-dialog-get.md) | Returns the operator's dialog data by one of the identifiers. ||
|| [imopenlines.message.session.start](./imopenlines-message-session-start.md) | Starts a new session and transfers the selected message into it. ||
|| [imopenlines.session.head.vote](./imopenlines-session-head-vote.md) | Saves the supervisor's rating for the completed session. ||
|| [imopenlines.session.history.get](./imopenlines-session-history-get.md) | Returns the message history and session data. ||
|| [imopenlines.session.intercept](./imopenlines-session-intercept.md) | Transfers the dialog to the current operator. ||
|| [imopenlines.session.join](./imopenlines-session-join.md) | Joins the current operator to the dialog. ||
|| [imopenlines.session.mode.pinAll](./imopenlines-session-mode-pin-all.md) | Pins all available dialogs to the current operator. ||
|| [imopenlines.session.mode.pin](./imopenlines-session-mode-pin.md) | Pins or unpins the selected dialog. ||
|| [imopenlines.session.mode.silent](./imopenlines-session-mode-silent.md) | Turns the dialog's silent mode on or off. ||
|| [imopenlines.session.mode.unpinAll](./imopenlines-session-mode-unpin-all.md) | Unpins all pinned dialogs from the current operator. ||
|| [imopenlines.session.open](./imopenlines-session-open.md) | Opens the open channel chat by user code. ||
|| [imopenlines.session.start](./imopenlines-session-start.md) | Starts a new session in the current chat. ||
|#