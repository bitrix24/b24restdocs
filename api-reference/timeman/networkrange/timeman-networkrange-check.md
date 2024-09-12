# Check IP Address timeman.networkrange.check

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `timeman.networkrange.check` is used to check if an IP address falls within the ranges of the office network.

## Parameters

#|
|| **Parameter** | **Example** | **Required** | **Description** ||
|| **IP**
[`unknown`](../../data-types.md) | 10.10.255.255 | No | IP address. ||
|#

If the `IP` parameter is not specified, the check will be performed for the current IP address.

## Example Call

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod('timeman.networkrange.check',
        {
            'IP': '10.10.255.255'
        },
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
        'timeman.networkrange.check',
        Array(
            'IP' => '10.10.255.255'
        ),
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
        "ip": "10.10.255.255",
        "range": "10.0.0.0-10.255.255.255",
        "name": "Office Network 10.x.x.x"
    }
}
```

### Key Descriptions

- **ip** - The IP address that was checked.
- **range** - The range that the specified IP address falls within.
- **name** - The name of the range that the specified IP address falls within.

## Error Response

> 200 Error, 50x Error
```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to use this method"
}
```

### Key Descriptions

- The **error** key - the code of the occurred error.
- The **error_description** key - a brief description of the occurred error.

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_ERROR** | The specified method is only available to administrators. ||
|#