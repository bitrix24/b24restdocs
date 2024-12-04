# Get Current Status of SIP Connector voximplant.sip.connector.status

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.sip.connector.status` returns the current status of the SIP Connector. This method is available to the holder of the [access permissions](https://helpdesk.bitrix24.com/open/18216960/) `Manage numbers - change - any`.

There are no input parameters.

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'voximplant.sip.connector.status',
        {},
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

## Returned Data

#|
|| **Field** | **Description** ||
|| **FREE_MINUTES** | Number of free minutes for setting up and testing the integration. ||
|| **PAID** | Whether the connector is paid or not. ||
|| **PAID_DATE_END** | Until what date the connector is paid (if payment has been made). ||
|#