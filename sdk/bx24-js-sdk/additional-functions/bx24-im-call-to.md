# Call via Internal Communication BX24.im.callTo

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

```js
void BX24.im.callTo(userId[, video=true])
```

The method `BX24.im.callTo` initiates a call via internal communication.

## Function Parameters

#|
|| **Parameter** | **Description** ||
|| **userId**
[`unknown`](../../../api-reference/data-types.md) | The identifier of the account user. ||
|| **video**
[`unknown`](../../../api-reference/data-types.md) | **true** - video call, **false** - audio call. Optional parameter. ||
|#