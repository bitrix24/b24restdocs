# Set the Employee's SIP Device Status voximplant.user.activatePhone

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- required parameters are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.user.activatePhone` sets the SIP device status for an employee. This method checks for the access permission to modify the user.

The method is available to the holder of the [permission](https://helpdesk.bitrix24.com/open/18216960/) `User Settings - Modification` according to the value of this permission.

#|
|| **Parameter** / **Type** | **Description** ||
|| **USER_ID**^*^
[`int`](../../../data-types.md) | User identifier. ||
|#

{% include [Parameter notes](../../../../_includes/required.md) %}