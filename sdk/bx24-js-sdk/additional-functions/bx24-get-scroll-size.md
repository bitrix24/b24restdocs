# Get Frame Dimensions with BX24.getScrollSize

The method `BX24.getScrollSize` returns the dimensions of the current frame's content as an object with the fields `scrollWidth` and `scrollHeight`.

```js
Object BX24.getScrollSize()
```

## Parameters

No parameters.

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const size = BX24.getScrollSize();
    console.log(size.scrollWidth, size.scrollHeight);
});
```

## Response Handling

The method synchronously returns an object containing the dimensions of the content.

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../api-reference/data-types.md) | An object with the dimensions of the frame's content [(detailed description)](#result) ||
|#

### Object result {#result}

#| 
|| **Name**
`type` | **Description** ||
|| **scrollWidth**
[`integer`](../../../api-reference/data-types.md) | The width of the frame's content in pixels ||
|| **scrollHeight**
[`integer`](../../../api-reference/data-types.md) | The height of the frame's content in pixels ||
|#

## Continue Learning

- [{#T}](./bx24-resize-window.md)
- [{#T}](./bx24-fit-window.md)