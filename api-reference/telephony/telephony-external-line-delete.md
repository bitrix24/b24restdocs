# Delete External Line telephony.externalLine.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Success response is absent
- Error response is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony admin](./_includes/scope-telephony-admin.md) %}

The method `telephony.externalLine.delete` removes an external line.

#|
|| **Parameter** / **Type** | **Description** ||
|| **NUMBER** 
[`string`](../data-types.md) | The number of the external line. ||
|#