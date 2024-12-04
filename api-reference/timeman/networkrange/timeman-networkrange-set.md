# Set Network Address Range timeman.networkrange.set

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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
> Who can execute the method: administrator

The method `timeman.networkrange.set` is used to set the ranges of network addresses that belong to the office network.

## Parameters

#|
|| **Parameter** | **Example** | **Required** | **Description** ||
|| **RANGES^*^**
[`unknown`](../../data-types.md) | `[{"ip_range":"10.0.0.0-10.255.255.255","name":"Office Network 10.x.x.x"}]` | Yes | Network address ranges. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

The range can contain a block of addresses, for example `10.0.0.0-10.255.255.255` or just a single address `10.10.0.1`.

## Example Call

{% list tabs %}

- JS

    ```js
    BX24.callMethod('timeman.networkrange.set',
    {
        ranges: '[{"ip_range":"10.0.0.0-10.255.255.255","name":"Office Network 10.x.x.x"},{"ip_range":"172.16.0.0-172.31.255.255","name":"Office Network 172.x.x.x"},{"ip_range":"192.168.0.0-192.168.255.255","name":"Office Network 192.168.x.x"}]'
    },
    function(result){
        if(result.error())
        {
            console.error(result.error().ex);
        }
        else
        {
            var answer = result.data();
            if (answer.result)
            {
                console.log('range saved');
            }
            else
            {
                console.warn('An error occurred while saving, the following ranges are incorrect', answer.error_ranges);
            }
        }
    });
    ```

- PHP

    ```php
    $result = restCommand('timeman.networkrange.set', Array(
        'RANGES' => Array(
            Array("ip_range" => "10.0.0.0-10.255.255.255", "name" => "Office Network 10.x.x.x"),
            Array("ip_range" => "172.16.0.0-172.31.255.255", "name" => "Office Network 172.x.x.x"),
            Array("ip_range" => "192.168.0.0-192.168.255.255", "name" => "Office Network 192.168.x.x")
        )
    ), $_REQUEST["auth"]);
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Successful Response

**On successful save**

> 200 OK
```json
{
    "result": {
        "result": true
    }
}
```

**On parsing error of ranges**

> 200 OK
```json
{
    "result": {
        "result": false,
        "error_range": [
            {"ip_range": "a10.0.0.0-10.255.255.255", "name": "Office Network 10.x.x.x"}
        ]
    }
}
```

### Key Descriptions

- **result** - the result of the save operation.
- **error_range** - an array of ranges where errors were found:
    - **ip_range** - the network address range.
    - **name** - the name of the range.

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
|| **ACCESS_ERROR** | The specified method is available only to administrators. ||
|| **INVALID_FORMAT** | An incorrect format was provided in the RANGE field. ||
|#