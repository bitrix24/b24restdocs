# Get User Settings voximplant.user.get

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

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.user.get` retrieves user settings. The method checks for permission to modify the user and requests administrator confirmation before execution.

The method is available to the holder of the [permission](https://helpdesk.bitrix24.com/open/18216960/) `User Settings - Modification` according to the value of this permission.

#|
|| **Parameter** / **Type** | **Description** ||
|| **USER_ID**
[`int`](../../../data-types.md) | Array of user identifiers. ||
|#

## Returned Data

The result will be an array of records with user settings.

#|
|| **Field** | **Description** ||
|| **ID** | User identifier ||
|| **DEFAULT_LINE** | Default outgoing number ||
|| **INNER_NUMBER** | Internal number ||
|| **PHONE_ENABLED** | Flag indicating if the user has a SIP device (Y\|N). ||
|| **SIP_SERVER** | Address of the server for connecting the SIP device. ||
|| **SIP_LOGIN** | Login for connecting the SIP device. ||
|| **SIP_PASSWORD** | Password for the SIP device. ||
|#