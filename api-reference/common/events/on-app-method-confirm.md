# Event on receiving permission to use onAppMethodConfirm methods

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter mandatory status not indicated
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onAppMethodConfirm` event is triggered upon receiving the [administrator's decision](../../scopes/confirmation.md) from the account regarding the request to use methods that require confirmation.

## Event Parameters

#|
|| **Parameter** | **Description** ||
|| **TOKEN**
[`unknown`](../../data-types.md) | Authorization token with which permission was requested ||
|| **METHOD**
[`unknown`](../../data-types.md) | The REST API method for which permission was requested. ||
|| **CONFIRMED**
[`unknown`](../../data-types.md) | Permission result, 0 - denied, 1 - granted ||
|#

## Examples

```js
array (
    'event' => 'ONAPPMETHODCONFIRM',
    'data' =>
    array (
        'TOKEN' => 'fkp963yuv1ggkfbs5z3f5hy8lilm0iw6',
        'METHOD' => 'voximplant.user.get',
        'CONFIRMED' => '1',
        'LANGUAGE_ID' => 'en',
    ),
    'ts' => '1478790852',
    'auth' =>
    array (
        'domain' => 'portal.bitrix24.com',
        'client_endpoint' => 'https://portal.bitrix24.com/rest/',
        'server_andpoint' => 'https://oauth.bitrix.info/rest/',
        'member_id' => '74ef8a46a75104de55d5d4a61b98ab6d',
        'application_token' => 'c289487163b58658eae5e8b42eaf11b8',
    ),
)
```

{% include [Examples note](../../../_includes/examples.md) %}