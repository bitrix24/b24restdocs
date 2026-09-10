# Interaction of the Widget with the Messenger Input Field

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An application widget in the messenger works with the input field of the active chat through two methods. The application calls them from its own frame, while Bitrix24 itself changes the input field. The application needs no access to the messenger interface.

#|
|| **Method** | **What It Does** ||
|| [im:getImTextareaContent](#get-im-textarea-content) | Returns the text the user has typed in the input field ||
|| [im:setImTextareaContent](#set-im-textarea-content) | Inserts the text of the application into the input field ||
|#

The methods cannot send a message: the text stays in the input field until the user clicks *Send*. The methods require no scope of their own — they do not call the REST API.

The methods are called through the `$b24.parent.message` namespace of the [B24JsSDK](./index.md) library. The namespace is available since version 1.1.0, and the examples on this page are given for the second major version.

## Conditions for the Methods to Work {#usage-conditions}

- the application is opened inside the Bitrix24 frame. Bitrix24 accepts messages only from the address of a registered application and ignores the rest
- the SDK is initialized via [initializeB24Frame()](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/frame-initialize-b24-frame/)
- the widget is opened in the messenger, and a chat is opened in it. The suitable placements are [IM_TEXTAREA](../../api-reference/widgets/im/textarea.md), [IM_SIDEBAR](../../api-reference/widgets/im/sidebar.md), and [IM_CONTEXT_MENU](../../api-reference/widgets/im/context-menu.md). To register any of them with the [placement.bind](../../api-reference/widgets/placement-bind.md) method, the application needs the `placement` and `im` scopes
- the user works in the web version of Bitrix24. The mobile application has no handler for these methods — the [IMMOBILE_CONTEXT_MENU](../../api-reference/widgets/mobile-app.md) widget receives no response

In [IM_NAVIGATION](../../api-reference/widgets/im/navigation.md), the current chat is not determined, but the handler is in place and responds. There is no error: `im:getImTextareaContent` returns an empty string, and `im:setImTextareaContent` responds with `success: true`, even though the text does not appear in the input field.

If the widget is opened where the messenger is not loaded, there is no handler at all: no response arrives, and the call ends by the `isSafely` timeout.

## Call Format

```js
$b24.parent.message.send(method, params)
```

The first parameter is the name of the method, the second one is an object with its parameters. The composition of the object is covered in the method sections below.

The call returns a promise. Bitrix24 responds with an object, and the promise resolves with that object — both on success and on a handler error. A handler error does not arrive as an exception, and the SDK does not describe the type of the result, so determine the outcome by the set of fields in the response.

Bitrix24 accepts a message only when the name of the method is known and `requestId` is a non-empty string. Otherwise the message is discarded silently: no response arrives, and a promise without `isSafely` stays unresolved. Therefore, call the methods with `isSafely: true`.

`isSafely` and `safelyTime` are handled by the SDK itself: they are not sent to Bitrix24 and do not affect the work of the method — they only define the behavior of the promise when there is no response.

Parallel calls do not have to be sorted out manually: the SDK marks every message with its own service key and returns the response to the promise that waits for it. Bitrix24 returns `requestId` unchanged — you need it to recognize your request in the logs.

These methods cannot be called from [BX24.js](../bx24-js-sdk/index.md). The `BX24.placement.call()` function sends a message without the `requestId` field, and Bitrix24 discards such a message — the messenger input field can only be worked with through B24JsSDK.

## Method im:getImTextareaContent {#get-im-textarea-content}

The method `im:getImTextareaContent` returns the current text from the input field of the active chat.

### Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **requestId***
[`string`](../../api-reference/data-types.md) | Request identifier, a non-empty string: if the value is empty, Bitrix24 discards the message and sends no response. In the response, the value is returned unchanged. Create it with [B24Js.Text.getUuidRfc4122()](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/tools-text#identifiers) ||
|| **isSafely**
[`boolean`](../../api-reference/data-types.md) | Limits the waiting for a response with a timeout. If `true`, the promise resolves even when Bitrix24 has not responded. The default value is `false` ||
|| **safelyTime**
[`integer`](../../api-reference/data-types.md) | How long to wait for a response, in milliseconds. It works only together with `isSafely: true`. The default value is `900` ||
|#

### Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

Read the draft of the user and output it to the console:

{% list tabs %}

- JS (TS)

    ```ts
    // ESM module: top-level await works in a bundler or with type="module"
    import { initializeB24Frame, Text } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    const responseGet = await $b24.parent.message.send(
        'im:getImTextareaContent',
        {
            requestId: Text.getUuidRfc4122(),
            isSafely: true,
            safelyTime: 1500
        }
    )

    if (typeof responseGet.text === 'string') {
        console.log(responseGet.text)
    }
    ```

- JS (UMD)

    ```js
    // The UMD build is included with a script tag, everything is available through the B24Js variable
    const $b24 = await B24Js.initializeB24Frame()

    const responseGet = await $b24.parent.message.send(
        'im:getImTextareaContent',
        {
            requestId: B24Js.Text.getUuidRfc4122(),
            isSafely: true,
            safelyTime: 1500
        }
    )

    if (typeof responseGet.text === 'string') {
        console.log(responseGet.text)
    }
    ```

{% endlist %}

### Response Handling

```json
{
    "requestId": "019323ac-8ace-725b-a3dc-6a7c333da066",
    "text": "Good afternoon! Could you please confirm the order number"
}
```

The responses that have no `text` field are covered in the [Error Handling](#errors) section.

#### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **requestId**
[`string`](../../api-reference/data-types.md) | Request identifier passed in the call ||
|| **text**
[`string`](../../api-reference/data-types.md) | Text from the input field of the active chat, exactly as the user typed it. An empty string arrives when the field is empty or the current chat is not determined ||
|#

## Method im:setImTextareaContent {#set-im-textarea-content}

The method `im:setImTextareaContent` inserts text into the input field of the active chat.

### Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **requestId***
[`string`](../../api-reference/data-types.md) | Request identifier, a non-empty string: if the value is empty, Bitrix24 discards the message and sends no response. In the response, the value is returned unchanged. Create it with [B24Js.Text.getUuidRfc4122()](https://bitrix-tools.github.io/b24jssdk/docs/working-with-the-rest-api/tools-text#identifiers) ||
|| **text**
[`string`](../../api-reference/data-types.md) | Text to insert into the input field. The method writes the value in full and does not limit its length, but a message longer than 20,000 characters is truncated by Bitrix24 when it is sent. The default value is an empty string: an empty `text` together with `replace: true` clears the input field ||
|| **withNewLine**
[`boolean`](../../api-reference/data-types.md) | If `true`, the text is appended to the end of what has been typed, on a new line, regardless of the cursor position. The default value is `false`: the text is placed at the cursor position and separated from the adjacent text with spaces, while a selected fragment is replaced. In an empty input field, the parameter has no effect ||
|| **replace**
[`boolean`](../../api-reference/data-types.md) | If `true`, the input field is cleared and only the text passed remains in it — in this case the value of `withNewLine` plays no role. If `false`, the text typed by the user is retained, and the new one is added to it by the rule from `withNewLine`. The default value is `false` ||
|| **isSafely**
[`boolean`](../../api-reference/data-types.md) | Limits the waiting for a response with a timeout. If `true`, the promise resolves even when Bitrix24 has not responded. The default value is `false` ||
|| **safelyTime**
[`integer`](../../api-reference/data-types.md) | How long to wait for a response, in milliseconds. It works only together with `isSafely: true`. The default value is `900` ||
|#

### Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

Append text to the end of the draft on a new line, without erasing what the user has typed:

{% list tabs %}

- JS (TS)

    ```ts
    // ESM module: top-level await works in a bundler or with type="module"
    import { initializeB24Frame, Text } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    const responseSet = await $b24.parent.message.send(
        'im:setImTextareaContent',
        {
            requestId: Text.getUuidRfc4122(),
            text: 'Order #1024 has been shipped, the tracking number will arrive during the day',
            withNewLine: true,
            replace: false,
            isSafely: true,
            safelyTime: 1500
        }
    )

    if (responseSet.success !== true) {
        console.log('Failed to insert the text', responseSet)
    }
    ```

- JS (UMD)

    ```js
    // The UMD build is included with a script tag, everything is available through the B24Js variable
    const $b24 = await B24Js.initializeB24Frame()

    const responseSet = await $b24.parent.message.send(
        'im:setImTextareaContent',
        {
            requestId: B24Js.Text.getUuidRfc4122(),
            text: 'Order #1024 has been shipped, the tracking number will arrive during the day',
            withNewLine: true,
            replace: false,
            isSafely: true,
            safelyTime: 1500
        }
    )

    if (responseSet.success !== true) {
        console.log('Failed to insert the text', responseSet)
    }
    ```

{% endlist %}

### Response Handling

```json
{
    "requestId": "019323ac-8ace-725b-a3dc-6a7c333da066",
    "success": true
}
```

The responses that have no `success` field are covered in the [Error Handling](#errors) section.

#### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **requestId**
[`string`](../../api-reference/data-types.md) | Request identifier passed in the call ||
|| **success**
[`boolean`](../../api-reference/data-types.md) | Always `true`. The field confirms that Bitrix24 has accepted the call, not that the text has ended up in the input field: if the current chat is not determined, the response is still `true`. You can check the result with a call to [im:getImTextareaContent](#get-im-textarea-content) ||
|#

## Both Methods in One Scenario

The application takes the typed text, processes it on its own side, and returns the result to the input field. The example rewrites the draft of the user in uppercase.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS (TS)

    ```ts
    import { initializeB24Frame, Text } from '@bitrix24/b24jssdk'

    async function rewriteDraft(): Promise<void> {
        const $b24 = await initializeB24Frame()

        const responseGet = await $b24.parent.message.send(
            'im:getImTextareaContent',
            {
                requestId: Text.getUuidRfc4122(),
                isSafely: true,
                safelyTime: 1500
            }
        )

        // An error and a timeout arrive without the text field
        if (typeof responseGet.text !== 'string') {
            console.log('Failed to retrieve the text', responseGet)
            return
        }

        // An empty string means that the field is empty or the chat is not determined
        if (responseGet.text.length === 0) {
            return
        }

        const responseSet = await $b24.parent.message.send(
            'im:setImTextareaContent',
            {
                text: responseGet.text.toUpperCase(),
                requestId: Text.getUuidRfc4122(),
                replace: true,
                isSafely: true,
                safelyTime: 1500
            }
        )

        if (responseSet.success !== true) {
            console.log('Failed to insert the text', responseSet)
        }
    }

    document.addEventListener('DOMContentLoaded', () => {
        rewriteDraft().catch((error) => console.log('Failed to start the widget', error))
    })
    ```

- JS (UMD)

    ```js
    // The UMD build is included with a script tag, everything is available through the B24Js variable
    async function rewriteDraft() {
        const $b24 = await B24Js.initializeB24Frame()

        const responseGet = await $b24.parent.message.send(
            'im:getImTextareaContent',
            {
                requestId: B24Js.Text.getUuidRfc4122(),
                isSafely: true,
                safelyTime: 1500
            }
        )

        // An error and a timeout arrive without the text field
        if (typeof responseGet.text !== 'string') {
            console.log('Failed to retrieve the text', responseGet)
            return
        }

        // An empty string means that the field is empty or the chat is not determined
        if (responseGet.text.length === 0) {
            return
        }

        const responseSet = await $b24.parent.message.send(
            'im:setImTextareaContent',
            {
                text: responseGet.text.toUpperCase(),
                requestId: B24Js.Text.getUuidRfc4122(),
                replace: true,
                isSafely: true,
                safelyTime: 1500
            }
        )

        if (responseSet.success !== true) {
            console.log('Failed to insert the text', responseSet)
        }
    }

    document.addEventListener('DOMContentLoaded', () => {
        rewriteDraft().catch((error) => console.log('Failed to start the widget', error))
    })
    ```

{% endlist %}

## Error Handling {#errors}

The methods return no error codes. The outcome of a call is determined by the set of fields in the response.

#|
|| **Outcome** | **How to Tell It Apart** ||
|| Successful response | There is a `text` field for [im:getImTextareaContent](#get-im-textarea-content) or a `success` field for [im:setImTextareaContent](#set-im-textarea-content) ||
|| Handler error | There is a `message` field ||
|| No response arrived | There is an `isSafely` field ||
|| No response will ever arrive | The promise does not resolve: the call was made without `isSafely`, and Bitrix24 discarded the message ||
|| The frame was destroyed | The promise is rejected with an `SdkError` error with the code `JSSDK_FRAME_DISPOSED` ||
|#

An initialization failure is not on this list: outside the Bitrix24 frame, the `initializeB24Frame()` promise is rejected with an `SdkError` error with the code `JSSDK_CLIENT_SIDE_WARNING` before the methods are called at all. How to handle this is described on the [Installation and Usage of B24JsSDK](./index.md) page.

### Handler Error

Bitrix24 has accepted the call, but the handler has ended with an exception. Its text arrives in the `message` field. There is no standard scenario that leads to this: an undetermined chat does not cause an error, and that case is covered in the [Conditions for the Methods to Work](#usage-conditions) section.

```json
{
    "requestId": "019323ac-8ace-725b-a3dc-6a7c333da066",
    "message": "Cannot read properties of undefined"
}
```

#|
|| **Name**
`type` | **Description** ||
|| **requestId**
[`string`](../../api-reference/data-types.md) | Request identifier passed in the call ||
|| **message**
[`string`](../../api-reference/data-types.md) | Text of the JavaScript error. The value comes from the browser, so the wording may change — do not use the error text as a condition in your code ||
|#

### No Response Arrived

Bitrix24 has not responded within `safelyTime`. This happens when the message was discarded because of an empty `requestId`, or when the widget is opened where the messenger is not loaded. Such an object has no `requestId` field: it is formed by the SDK on its own timer, not by Bitrix24.

```json
{
    "isSafely": true
}
```

### The Frame Was Destroyed

This is the only case when a call arrives as an exception: the application destroyed the frame with the `destroy()` method without waiting for a response. The SDK rejects all pending calls with an `SdkError` error with the code `JSSDK_FRAME_DISPOSED`. Widgets built with Vue, React, and Nuxt run into this when a component is unmounted during a request — wrap the call in `try/catch` or attach a `catch()` to the promise.

## Implementation Example

Download the [example widget](https://helpdesk.bitrix24.com/examples/iframe_content.zip) — it shows both methods at work.

### How the Example Works

1. The SDK is connected in the browser via the UMD script `@bitrix24/b24jssdk`.
2. Upon page load, `B24Js.initializeB24Frame()` is called.
3. The **Get text** button sends `im:getImTextareaContent` and displays the `text` field from the response.
4. The **Set text** button sends `im:setImTextareaContent` and inserts text into the chat input field.
5. The flags `withNewLine` and `replace` are taken from checkboxes in the form.
6. The results of the requests are displayed in the `#log` block and in the `console`.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../../api-reference/widgets/im/index.md)
- [{#T}](../../api-reference/widgets/im/textarea.md)
- [{#T}](../../api-reference/widgets/im/sidebar.md)
- [{#T}](../../api-reference/widgets/im/context-menu.md)
- [{#T}](../../api-reference/widgets/placement-bind.md)
- [{#T}](../../api-reference/widgets/ui-interaction/index.md)
