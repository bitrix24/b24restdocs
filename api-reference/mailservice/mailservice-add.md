# Create a new mail service mailservice.add

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing parameter type descriptions
- no response examples
- parameter versions not specified

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.add` adds a new mail service.

## Parameters

#|
||  **Parameter** / **Type**| **Description** | **From version** ||
|| **ACTIVE**
[`unknown`](../data-types.md) | Service activity (Y / N) | ||
|| **NAME**
[`unknown`](../data-types.md) | Name of the mail service being added | ||
|| **SERVER**
[`unknown`](../data-types.md) | Address of the mail server being added | ||
|| **PORT**
[`unknown`](../data-types.md) | Port number | ||
|| **ENCRYPTION**
[`unknown`](../data-types.md) | Need for encryption (Y / N) | ||
|| **LINK**
[`unknown`](../data-types.md) | Link to the mail service | ||
|| **SORT**
[`unknown`](../data-types.md) | Sort index | ||
|#

## Example

```js
BX24.callMethod(
    "mailservice.add",
    {
        'ACTIVE': 'Y',
        'NAME': 'Mail service Yandex',
        'SERVER': 'imap.yandex.com',
        'PORT': '993',
        'ENCRYPTION': 'Y',
        'LINK': 'https://mail.yandex.com/',
        'SORT': '500'
    },
    function(result)
    {
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            console.info(result.data());
        }
    }
);
```
{% include [Footnote about examples](../../_includes/examples.md) %}