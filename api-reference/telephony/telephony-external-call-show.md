# Show Call Detail to User telephony.externalcall.show

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](./_includes/scope-telephony-all.md) %}

The method `telephony.externalcall.show` displays the call detail to the user.

#|
|| **Parameter** / **Type** | **Description** ||
|| **CALL_ID**^*^ 
[`string`](../data-types.md) | The identifier of the call from the method [telephony.externalcall.register](telephony-external-call-register.md). ||
|| **USER_ID** 
[`int`](../data-types.md) | The identifier or an array of user identifiers in the standard PHP format: `USER_ID[0]=11&USER_ID[1]=24&USER_ID[2]=31`. ||
|#

{% include [Footnote on parameters](../../_includes/required.md) %}