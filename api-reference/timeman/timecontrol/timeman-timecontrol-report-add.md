# Send Absence Report timeman.timecontrol.report.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.timecontrol.report.add` is used to send an absence report.

## Parameters

#|
|| **Parameter** | **Example** | **Required** | **Description** ||
|| **ID**^*^
[`unknown`](../../data-types.md) | 468 | Yes | Record identifier. ||
|| **TYPE**^*^
[`unknown`](../../data-types.md) | work | Yes | Type of absence (`work` - for work-related issues, `private` - personal matters). ||
|| **TEXT**^*^
[`unknown`](../../data-types.md) | 'Was at lunch' | Yes | Description of the reason for absence. ||
|| **CALENDAR**
[`unknown`](../../data-types.md) | true | No | Add absence to the calendar (only for the initial report). ||
|| **USER_ID**
[`unknown`](../../data-types.md) | 2 | No | Identifier of the user for whom the report is generated (this field is available only to administrators). ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Example Call

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod('timeman.timecontrol.report.add', {
        'id': 468,
        'type': 'private',
        'text': 'Was at lunch',
        'calendar': true
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
    $result = restCommand('timeman.timecontrol.report.add', Array(
        'ID' => 468,
        'TYPE' => 'private',
        'TEXT' => 'Was at lunch',
        'CALENDAR' => true
    ), $_REQUEST["auth"]);    
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Successful Response

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
    "error": "TEXT_EMPTY",
    "error_description": "Text can't be empty"
}
```

### Key Descriptions

- Key **error** - code of the occurred error.
- Key **error_description** - brief description of the occurred error.

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **TEXT_EMPTY** | Reason for absence was not provided. ||
|| **ACCESS_ERROR** | You do not have access to this report. ||
|#