# Get Information About the Call Context of BX24.placement.info

> Scope: [`placement`](../../scopes/permissions.md)

The method `BX24.placement.info` retrieves information about the context of the embedding handler call.

No parameters.

## Code Example

{% include [Footnote on examples](../../../_includes/examples.md) %}

```js
BX24.ready(function () {
    BX24.init(function () {
        var placementInfo = BX24.placement.info();
        console.info("placement = " + placementInfo["placement"]
    + ", options = " + placementInfo["options"]);
    });
});
```

## Result

```json
{"placement":"CRM_LEAD_LIST_MENU","options":{"ID":"1348"}}
```

## Continue Your Learning

- [{#T}](bx24-placement-get-interface.md)
- [{#T}](bx24-placement-call.md)
- [{#T}](bx24-placement-bind-event.md)