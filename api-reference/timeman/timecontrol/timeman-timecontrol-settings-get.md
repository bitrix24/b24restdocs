# Get Time Control Tool Settings timeman.timecontrol.settings.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `timeman.timecontrol.settings.get` is used to retrieve the settings of the time control tool.

## Parameters

No parameters.

## Example Call

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'timeman.timecontrol.settings.get',
        {},
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    $result = restCommand(
        'timeman.timecontrol.settings.get',
        Array(),
        $_REQUEST["auth"]
    );    
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

## Successful Response

> 200 OK
```json
{
    "result": {
        "active": true,
        "minimum_idle_for_report": 1,
        "register_offline": true,
        "register_idle": true,
        "register_desktop": true,
        "report_request_type": "all",
        "report_request_users": [],
        "report_simple_type": "all",
        "report_simple_users": [],
        "report_full_type": "all",
        "report_full_users": []
    }
}
```

### Key Descriptions

- **active** - availability of the time control tool.
- **minimum_idle_for_report** - minimum idle time for report request (in minutes).
- **register_offline** - log when the user goes offline.
- **register_idle** - log when the user goes idle.
- **register_desktop** - log when the desktop application is turned on or off.
- **report_request_type** - who to request the report from (`all` - from everyone, `user` - only from specified users, `none` - from no one).
- **report_request_users** - list of users to request the report from (if `report_request_type == user`).
- **report_simple_type** - who has access to the simplified report (`all` - everyone, `user` - only specified users).
- **report_simple_users** - list of users who have access to the simplified report (if `report_simple_type == user`).
- **report_full_type** - who has access to the extended report (`all` - everyone, `user` - only specified users).
- **report_full_users** - list of users who have access to the extended report (if `report_simple_type == user`).

## Error Response

> 200 Error, 50x Error
```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to use this method"
}
```

- **error** - code of the occurred error.
- **error_description** - brief description of the occurred error.

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_ERROR** | The specified method is only available to administrators. ||
|#