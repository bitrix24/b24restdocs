# Deactivate the employee's SIP phone presence with voximplant.user.deactivatePhone

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not to be deployed to prod_" %}

- The required parameters are not specified
- Examples are missing
- Response on success is absent
- Response on error is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.user.deactivatePhone` disables the SIP phone presence for an employee. This method checks for the permission to modify the user.

The method is available to the holder of the [permission](https://helpdesk.bitrix24.com/open/18216960/) `User Settings - Modification` according to the value of this permission.

#|
|| **Parameter** / **Type** | **Description** ||
|| **USER_ID**^*^
[`int`](../../../data-types.md) | User identifier. ||
|#

{% include [Footnote on parameters](../../../../_includes/required.md) %}