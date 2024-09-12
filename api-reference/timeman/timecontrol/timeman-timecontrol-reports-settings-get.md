# Get User Settings timeman.timecontrol.reports.settings.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.timecontrol.reports.settings.get` is used to retrieve user settings for generating the report interface of the time control tool.

## Parameters

No parameters.

## Example Call

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod('timeman.timecontrol.reports.settings.get', {}, function(result){
        if(result.error())
        {
            console.error(result.error().ex);
        }
        else
        {
            console.log(result.data());
        }
    });
    ```

- PHP

    ```php
    $result = restCommand('timeman.timecontrol.reports.settings.get', Array(), $_REQUEST["auth"]);    
    ```

{% endlist %}

{% include [Example Note](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{
    "result": {
        "active":true,
        "user_id":2,
        "user_admin":true,
        "user_head":true,
        "departments":[
            {"id":"92","name":"Kaliningrad Branch"},
            {"id":"93","name":"Administration"},
            {"id":"106","name":"IT Department"}
        ],
        "minimum_idle_for_report":1,
        "report_view_type":"head"
    }
}
```

### Key Descriptions

- **active** - availability of the time control tool.
- **user_id** - current user identifier.
- **user_admin** - flag indicating if you are an administrator.
- **user_head** - flag indicating if you are a manager.
- **departments** - list of available departments (available only if you are a manager)
- **id** - department identifier
- **name** - department name
- **minimum_idle_for_report** - minimum idle time for requesting a report (in minutes)
- **report_view_type** - level of report detail (head, full, simple)