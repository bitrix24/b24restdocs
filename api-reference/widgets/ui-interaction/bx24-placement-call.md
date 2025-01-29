# Call the Registered Interface Command BX24.placement.call

> Scope: [`placement`](../../scopes/permissions.md)

The method `BX24.placement.call` invokes a registered interface command.

## Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **command***  
[`string`](../../data-types.md) | The command to be invoked ||
|| **parameters**  
[`object`](../../data-types.md) | Parameters to be passed ||
|| **callback***  
[`callable`](../../data-types.md) | Callback function ||
|#

## Code Example

{% include [Note on Examples](../../../_includes/examples.md) %}

```js
BX24.ready(function () {
    BX24.init(function () {
        BX24.placement.call('getStatus', {}, function (result) {
            console.log(result);
        });

        BX24.placement.call('CallCardSetCardTitle', {title: 'hello world!'}, function (result) {
            console.log(result);
        });
    });
});
```

## Continue Learning 

- [{#T}](bx24-placement-info.md)
- [{#T}](bx24-placement-get-interface.md)
- [{#T}](bx24-placement-bind-event.md)