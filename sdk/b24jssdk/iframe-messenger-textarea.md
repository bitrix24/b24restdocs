# Interaction of the Widget with the Messenger Input Field

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

To work with text in the chat input field, use the methods `$b24.parent.message.send`. This is convenient when you need to:

- retrieve the current text from the chat
- insert prepared text into the input field
- do this safely, without directly interacting with the messenger interface

## Call Format

```js
$b24.parent.message.send(method, params)
```

## Prerequisites Before Calls

1. The application must be running inside the Bitrix24 frame.
2. The SDK must be initialized via [initializeB24Frame()](https://bitrix-tools.github.io/b24jssdk/reference/frame-initialize-b24-frame.html).

Example:

```js
const $b24 = await B24Js.initializeB24Frame()
```

## Method im:getImTextareaContent

The method `im:getImTextareaContent` returns the current text from the input field of the active chat.

### Parameters

{% include [Parameters Note](../../_includes/required.md) %}

#| 
|| **Parameter** | **Description** ||
|| **requestId***  
`string` | Unique request ID. Create it using [B24Js.Text.getUuidRfc4122()](https://bitrix-tools.github.io/b24jssdk/reference/tools-text.html#getuuidrfc4122) ||
|| **isSafely**  
`boolean` | If `true`, a timeout for waiting for a response is applied. Use together with `safelyTime`. If `false`, no timeout is applied for this parameter ||
|| **safelyTime**  
`integer` | How long to wait for a response in milliseconds ||
|#

### Example

```js
const responseGet = await $b24.parent.message.send(
    'im:getImTextareaContent',
    {
        requestId: B24Js.Text.getUuidRfc4122(),
        isSafely: true,
        safelyTime: 1500
    }
)
```

The `responseGet` will contain the text from the input field. If an error occurs, the response will return an object with the following fields:

- `message` — error text (JavaScript system message)
- `requestId` — the same `requestId` that was passed in the request

Possible reasons for the error:

- failed to determine the current dialog
- the text field is unavailable

## Method im:setImTextareaContent

The method `im:setImTextareaContent` inserts text into the input field of the active chat.

### Parameters

{% include [Parameters Note](../../_includes/required.md) %}

#| 
|| **Parameter** | **Description** ||
|| **text***  
`string` | Text to insert into the input field ||
|| **requestId***  
`string` | Unique request ID. Create it using [`B24Js.Text.getUuidRfc4122()`](https://bitrix-tools.github.io/b24jssdk/reference/tools-text.html#getuuidrfc4122) ||
|| **withNewLine**  
`boolean` | If `true`, the text will be added on a new line ||
|| **replace**  
`boolean` | If `true`, the current content of the field will be completely replaced ||
|| **isSafely**  
`boolean` | If `true`, a timeout for waiting for a response is applied. Use together with `safelyTime`. If `false`, no timeout is applied for this parameter ||
|| **safelyTime**  
`integer` | How long to wait for a response in milliseconds ||
|#

### Example

```js
const responseSet = await $b24.parent.message.send(
    'im:setImTextareaContent',
    {
        text: 'Hello from iframe!',
        requestId: B24Js.Text.getUuidRfc4122(),
        withNewLine: false,
        replace: true,
        isSafely: true,
        safelyTime: 1500
    }
)
```

The `responseSet` will contain the result of the insertion. If an error occurs, the response will return an object with the following fields:

- `message` — error text (JavaScript system message)
- `requestId` — the same `requestId` that was passed in the request

Possible reasons for the error:

- failed to determine the current dialog
- the text field is unavailable

## Implementation Example

An example widget demonstrating both methods can be [downloaded](https://helpdesk.bitrix24.com/examples/iframe_content.zip).

### How the Example Works

1. The SDK is connected in the browser via the UMD script `@bitrix24/b24jssdk`.
2. Upon page load, `B24Js.initializeB24Frame()` is called.
3. The **Get text** button sends `im:getImTextareaContent` and retrieves the current text from the input field.
4. The **Set text** button sends `im:setImTextareaContent` and inserts text into the chat.
5. The flags `withNewLine` and `replace` are taken from checkboxes in the form.
6. The results of the requests are displayed in the `#log` block and in the `console`.