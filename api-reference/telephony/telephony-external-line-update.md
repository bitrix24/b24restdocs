# Change the Name of the External Line telephony.externalLine.update

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

The method `telephony.externalLine.update` allows you to change the name of the external line.

#|
|| **Parameter** / **Type** | **Description** ||
|| **NUMBER** 
[`string`](../data-types.md) | The number of the external line. ||
|| **NAME** 
[`string`](../data-types.md) | The name of the external line. ||
|| **CRM_AUTO_CREATE** 
[`boolean`](../data-types.md) | This parameter controls the creation of CRM entities (deal or lead, depending on the CRM operating mode) during outgoing calls from the Bitrix interface (for example, the dialer in the chat panel). Accepts values Y\|N. Default is Y. ||
|#