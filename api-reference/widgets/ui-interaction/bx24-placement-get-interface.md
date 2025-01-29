# Get Information About the JS Interface of the Current Placement BX24.placement.getInterface

> Scope: [`placement`](../../scopes/permissions.md)

The method `BX24.placement.getInterface` allows you to retrieve information about the JS interface of the current placement: a list of available commands and events.

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **callback*** 
[`callable`](../../data-types.md) | Callback function. 

The `callback` handler will receive an object of the form `{command: array, event: array}`, where: 
- `command` — list of available commands
- `event` — list of available events

For example: the placement `CALL_CARD` is intended for working with the call card in the CRM
 ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.ready(function () {
    BX24.init(function () {
        BX24.placement.getInterface((result) => {
            console.info(result);
        });
    });
});
```

## Result

```json
{"command":["getStatus", "disableAutoClose", "enableAutoClose" …],"event":[{"CallCard::EntityChanged", "CallCard::CallStateChanged", "CallCard::BeforeClose" …}]}
```

## Continue Your Learning

- [{#T}](bx24-placement-info.md)
- [{#T}](bx24-placement-call.md)
- [{#T}](bx24-placement-bind-event.md)