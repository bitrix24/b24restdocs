# Get Network Address Ranges timeman.networkrange.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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

The method `timeman.networkrange.get` is used to retrieve the ranges of network addresses that belong to the office network.

## Parameters

No parameters.

## Example Call

{% list tabs %}

- JS

    ```js
    BX24.callMethod('timeman.networkrange.get', {}, function(result){
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
    $result = restCommand(
        'timeman.networkrange.get',
        Array(),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Successful Response

> 200 OK
```json
{
    "result": [
        {
            "ip_range":"10.0.0.0-10.255.255.255",
            "name":"Office Network 10.x.x.x"
        },
        {
            "ip_range":"172.16.0.0-172.31.255.255",
            "name":"Office Network 172.x.x.x"
        },
        {
            "ip_range":"192.168.0.0-192.168.255.255",
            "name":"Office Network 192.168.x.x"
        }
    ]
}
```

### Key Descriptions

- **ip_range** - range of network addresses.
- **name** - name of the range.

## Error Response

> 200 Error, 50x Error
```json
{
    "error": "ACCESS_ERROR",
    "error_description": "You don't have access to use this method"
}
```
### Key Descriptions

- Key **error** - code of the occurred error.
- Key **error_description** - brief description of the occurred error.

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_ERROR** | The specified method is only available to administrators. ||
|#