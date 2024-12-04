# Set the "read" flag for all chats im.dialog.read.all

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- examples are missing
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.dialog.read.all` sets the "read" flags for all dialogs.

No parameters are passed.

## Examples

{% list tabs %}

- JS

    ```js
    B24.callMethod(
        'im.dialog.read.all',
        {},
        res => console.log(res.data())
    )
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in case of success

```json
{
    "result": true
}
```