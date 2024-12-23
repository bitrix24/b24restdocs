# Get a list of all SIP lines created by the application voximplant.sip.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.sip.get` returns a list of all SIP lines created by the application. It is a listing method. The method is available to the holder of the [access permissions](https://helpdesk.bitrix24.com/open/18216960/) `Manage numbers - modify - any`.

#|
|| **Parameter** | **Description** ||
|| **FILTER** | Fields for sorting. ||
|| **SORT** | The field by which sorting is performed. ||
|| **ORDER** | Sorting order (ASC/DESC). ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'voximplant.sip.get',
        {
            "FILTER": {"CONFIG_ID":12},
            "SORT": "CONFIG_ID",
            "ORDER": "DESC",
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}

## Returned data

#|
|| **Field** | **Description** ||
|| **CONFIG_ID** | Identifier of the SIP line configuration. ||
|| **TYPE** | Type of PBX, see the list of PBX types. ||
|| **TITLE** | Name of the connection. ||
|| **SERVER** | Address of the SIP registration server. ||
|| **LOGIN** | Login for the server. ||
|| **PASSWORD** | Password for the server. ||
|| **REG_ID** | Identifier of the SIP registration (only for Cloud PBX). ||
|| **INCOMING_SERVER** | Address of the server for connection during outgoing calls (only for Cloud PBX). ||
|| **INCOMING_LOGIN** | Login for the connection (only for Cloud PBX). ||
|| **INCOMING_PASSWORD** | Password for the connection (only for Cloud PBX). ||
|#