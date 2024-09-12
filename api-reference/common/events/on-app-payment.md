# Event onAppPayment

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onAppPayment` event is triggered when an application is paid for.

## Response Fields

#|
|| **Field** | **Description** ||
|| **CODE**
[`unknown`](../../data-types.md) | application code ||
|| **VERSION**
[`unknown`](../../data-types.md) | installed version of the application ||
|| **STATUS**
[`unknown`](../../data-types.md) | application status. Possible values:
- **F** (Free) - free
- **D** (Demo) - demo version
- **T** (Trial) - trial version (time-limited)
- **P** (Paid) - paid application ||
|| **PAYMENT_EXPIRED**
[`unknown`](../../data-types.md) | [Y\|N] flag indicating whether the paid period or trial period has expired. ||
|| **DAY**
[`unknown`](../../data-types.md) | number of days remaining until the end of the paid period or trial period. ||
|#

{% note tip "Related methods and topics" %}

[app.info](../system/app-info.md)

{% endnote %}