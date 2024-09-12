# Get Information About Feature Availability on Account feature.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- required parameters not specified
- examples missing
- response in case of error missing

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `feature.get` returns information about the availability of features on a specific account.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **CODE**
[`string`](../../data-types.md) | Available keys:
- rest_offline_extended - availability of offline events
- rest_auth_connector - availability of the auth_connector key in events ||
|#

## Examples

```js
$result[] = CRest::call(
    'feature.get',
    [
        'CODE' => 'rest_offline_extended',
    ]
);
```

{% include [Example Note](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
'result' =>
    [
        'value' => 'Y'
    ]
```

### Returned Data

- `value Y` - availability of the feature on the account. Y - available on the current account, N - not available on the account
- `lang_selfhosted`, where lang is replaced with en, ru, ua, kz, etc. (for on-premise Bitrix24).