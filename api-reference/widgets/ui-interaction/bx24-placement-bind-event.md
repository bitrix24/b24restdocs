# Set Up an Event Handler for the Interface BX24.placement.bindEvent

> Scope: [`placement`](../../scopes/permissions.md)

The method `BX24.placement.bindEvent` sets an event handler for the interface. The event must be registered on the calling side; otherwise, nothing will happen.

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***  
[`string`](../../data-types.md) | The name of the event to which the handler subscribes ||
|| **callback***  
[`callable`](../../data-types.md) | The callback function.

The `callback` handler may or may not receive data depending on the event it subscribes to. ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.ready(function () {
    BX24.init(function () {
        BX24.placement.bindEvent('BackgroundCallCard::initialized', event => {
            // some code
        });

        BX24.placement.bindEvent('CallCard::CallStateChanged', (callState) => {
            console.log(callState);
        });
    });
});
```

## Continue Learning

- [{#T}](bx24-placement-info.md)
- [{#T}](bx24-placement-get-interface.md)
- [{#T}](bx24-placement-call.md)