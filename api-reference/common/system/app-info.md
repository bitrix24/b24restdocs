# Show Application Information app.info

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- missing examples
- missing success response

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `app.info` method displays information about the application. It supports secure calls.

## Examples

Request
```http
http://my.bitrix24.com/rest/app.info?auth=d161f25928c3184678924ec127edd29a
```

{% include [Example notes](../../../_includes/examples.md) %}

## Success Response

> 200 OK
```json
{
    "result": {
        "ID":"7",
        "CODE":"local.56017020f07e17.98523769",
        "VERSION":1,
        "STATUS":"L",
        "INSTALLED":true,
        "PAYMENT_EXPIRED":"N",
        "DAYS":null,
        "LICENSE":"com_project"
    }
}
```

### Returned Data
#|
|| **Field** | **Description** ||
|| **ID** | local identifier of the application on the account ||
|| **CODE** | application code ||
|| **VERSION** | installed version of the application ||
|| **STATUS** | status of the application. Possible values:
- **F** (Free) - free;
- **D** (Demo) - demo version;
- **T** (Trial) - trial version (time-limited);
- **P** (Paid) - paid application;
- **L** (Local) - local application;
- **S** (Subscription) - subscription application. ||
|| **INSTALLED** | [true\|false] Status of the application's installation. If the application is not installed, it is only available to account administrators and should signal the completion of installation by calling [BX24.installFinish()](700086) ||
|| **PAYMENT_EXPIRED** | [Y\|N] flag indicating whether the paid period or trial period has expired. ||
|| **DAYS** | Number of days remaining until the end of the paid period or trial period. ||
|| **LICENSE** | Designation of the plan with the region indicated as a prefix. Consists of the base language of the account and the plan identifier. In cases where the plans have changed while retaining the public name (like CRM+, Team, and Company), it is not possible to determine which plan is active based on this field. Examples of possible values:
- **com_project** - Project plan
- **com_basic** - Basic plan
- **com_std** - Standard plan
- **com_pro100** - Professional plan
- **com_ent250** - Enterprise 250
- **com_ent500** - Enterprise 500
- **com_ent1000** - Enterprise 1000
- **com_ent2000** - Enterprise 2000
- **com_ent10000** - Enterprise 10000 ||
|#

After the paid period expires, the application will continue to operate during the grace period allowed for potential payment delays, after which it will automatically switch to demo mode or be blocked. At this point, the value of the PAYMENT_EXPIRED flag will be Y, and the DAYS field will contain a negative number.