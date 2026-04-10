# Set Page Title BX24.setTitle

The method `BX24.setTitle` sends a command to change the title of the application page.

```js
void BX24.setTitle(String title[, Function callback])
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`Type` | **Description** ||
|| **title*** 
`string` | The new title of the page. The value is converted to a string using `toString()` within the method. ||
|| **callback** 
`function` | A callback function that is executed after the command to change the title is sent. ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    BX24.setTitle('New Title', function () {
        console.log('Command to change the title has been sent');
    });
});
```

## Response Handling

The method does not return any data (`void`).

## Continue Learning

- [{#T}](./bx24-scroll-parent-window.md)
- [{#T}](./bx24-reload-window.md)