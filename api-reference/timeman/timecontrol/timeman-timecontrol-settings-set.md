# Set Time Control Tool Settings timeman.timecontrol.settings.set

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `timeman.timecontrol.settings.set` is used to set the settings for the time control tool.

## Parameters

#|
|| **Parameter** | **Default** | **Required** | **Description** ||
|| **ACTIVE**
[`unknown`](../../data-types.md) | false | No | Availability of the time control tool. 
Enabled with `active: true`. Disabled with `active: false` if data is sent as *bool*. If data is sent as text *false* in text form as *true*, it can only be disabled with `active: 0`. ||
|| **MINIMUM_IDLE_FOR_REPORT**
[`unknown`](../../data-types.md) | 15 | No | Minimum amount of time for requesting a report in minutes. ||
|| **REGISTER_OFFLINE**
[`unknown`](../../data-types.md) | true | No | Record the fact that the user has gone offline. ||
|| **REGISTER_IDLE**
[`unknown`](../../data-types.md) | true | No | Record the fact that the user has stepped away. ||
|| **REGISTER_DESKTOP**
[`unknown`](../../data-types.md) | true | No | Record the fact of starting and stopping the desktop application. ||
|| **REPORT_REQUEST_TYPE**
[`unknown`](../../data-types.md) | none | No | Who to request the report from (`all` - from everyone, `user` - only from specified users, none - from no one). ||
|| **REPORT_REQUEST_USERS**
[`unknown`](../../data-types.md) | [] | No* | List of users to request the report from (if `report_request_type == user`). ||
|| **REPORT_SIMPLE_TYPE**
[`unknown`](../../data-types.md) | all | No | Who has access to the simplified report (`all` - everyone, `user` - only specified users). ||
|| **REPORT_SIMPLE_USERS**
[`unknown`](../../data-types.md) | [] | No* | List of users who have access to the simplified report (if `report_simple_type == user`). ||
|| **REPORT_FULL_TYPE**
[`unknown`](../../data-types.md) | user | No | Who has access to the extended report (`all` - everyone, `user` - only specified users). ||
|| **REPORT_FULL_USERS**
[`unknown`](../../data-types.md) | [] | No* | List of users who have access to the extended report (if `report_simple_type == user`). ||
|#

\* - if you pass the parameter `REPORT_REQUEST_TYPE = user` (or `REPORT_SIMPLE_TYPE = user`, or `REPORT_FULL_TYPE = user`), you must also pass `REPORT_REQUEST_USERS` (or `REPORT_SIMPLE_USERS`, or `REPORT_FULL_USERS`) accordingly.

## Example

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod('timeman.timecontrol.settings.set', {
        active: true,
        report_request_type: 'user',
        report_request_users: [1,2,3],
    }, function(result){
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
    $result = restCommand('timeman.timecontrol.settings.set', Array(
        active: true,
        report_request_type: 'user',
        report_request_users: [1,2,3],
    ), $_REQUEST["auth"]);
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Success Response

> 200 OK
```json
{
    "result": true
}
```

## Error Response

> 200 Error, 50x Error
```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to use this method"
}
```

### Key Descriptions

- **error** - code of the occurred error.
- **error_description** - brief description of the occurred error.

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_ERROR** | The specified method is available only to administrators. ||
|| **INVALID_FORMAT** | An incorrect format was provided in the `RANGE` field. ||
|#