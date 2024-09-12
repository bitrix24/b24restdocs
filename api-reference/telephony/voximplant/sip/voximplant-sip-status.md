# Get Current SIP Registration Status (for Cloud PBXs only) voximplant.sip.status

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

The method `voximplant.sip.status` returns the current status of SIP registration (for Cloud PBXs only). This method is available to the holder of the [access permission](https://helpdesk.bitrix24.com/open/18216960/) `Manage numbers - modify - any`.

#|
|| **Parameter** | **Description** ||
|| **REG_ID** | SIP registration identifier. ||
|#

## Example

```javascript
BX24.callMethod(
    "voximplant.sip.status",
    {
        "REG_ID": 5505,
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}

## Returned Data

#|
|| **Field** | **Description** ||
|| **REG_ID** | SIP registration identifier. ||
|| **LAST_UPDATED** | Date of the last change to the SIP registration. ||
|| **ERROR_MESSAGE** | Text description of the error code. ||
|| **STATUS_CODE** | Numeric error code. ||
|| **STATUS_RESULT** | Status of the SIP registration, see the status table. ||
|#