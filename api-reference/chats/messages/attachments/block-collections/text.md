# MESSAGE Block

The `MESSAGE` block displays the text portion of an attachment.

![MESSAGE Block](./_images/text.png)

## Block Parameters

#|
|| **Name**
`type` | **Description** ||
|| **MESSAGE***  
[`string`](../../../../data-types.md) | Text of the block. Supports BB codes ||
|#

## Supported BB Codes

#|
|| **Code** | **Purpose** ||
|| `USER` | Mention a user with a link to their profile in the chat ||
|| `CHAT` | Link to the chat ||
|| `SEND` | Clickable action "send text to chat" ||
|| `PUT` | Clickable action "insert text into input field" ||
|| `CALL` | Clickable action for making a call ||
|| `BR` | Line break ||
|| `B` | Bold text ||
|| `U` | Underlined text ||
|| `I` | Italic text ||
|| `S` | Strikethrough text ||
|| `URL` | Link ||
|#

## Example

{% include [Example Notes](../../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    {
        MESSAGE: 'The API will be available in the update [B]im 24.0.0[/B]'
    }
    ```

- PHP

    ```php
    [
        'MESSAGE' => 'The API will be available in the update [B]im 24.0.0[/B]'
    ]
    ```

{% endlist %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./delimiter.md)
- [{#T}](./grid.md)