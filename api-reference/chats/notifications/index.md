# About Notifications

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no content (only a list of methods is available)
- from Sergey's file: how to apply, where it's useful

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [im.notify.answer](./im-notify-answer.md) | Responds to a notification that supports quick reply ||
|| [im.notify.confirm](./im-notify-confirm.md) | Interacts with notification buttons ||
|| [im.notify.delete](./im-notify-delete.md) | Deletes notifications ||
|| [im.notify.personal.add](./im-notify-personal-add.md) | Sends a personal notification ||
|| [im.notify.read.list](./im-notify-read-list.md) | Reads the list of notifications (except CONFIRM) ||
|| [im.notify.read](./im-notify-read.md) | Sets the cancellation of read notifications ||
|| [im.notify.system.add](./im-notify-system-add.md) | Sends a system notification ||
|#