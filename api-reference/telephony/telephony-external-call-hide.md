# Hide Call Card for User telephony.externalcall.hide

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](./_includes/scope-telephony-all.md) %}

The method `telephony.externalcall.hide` hides the call card for the user.

#|
|| **Parameter** / **Type** | **Description** ||
|| **CALL_ID**^*^ 
[`string`](../data-types.md) | The identifier of the call from the method [telephony.externalcall.register](telephony-external-call-register.md). ||
|| **USER_ID**^*^ 
[`int`](../data-types.md) | The identifier or an array of identifiers of users for whom the call should be hidden, if the card is displayed for more than one user. ||
|#

{% include [Footnote on parameters](../../_includes/required.md) %}