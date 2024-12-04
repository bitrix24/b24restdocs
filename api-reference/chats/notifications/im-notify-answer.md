# Responding to a notification that supports quick response im.notify.answer

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.answer` provides a response to a notification that supports quick response.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | `270` | Identifier of the notification that supports quick response | `30` ||
|| **ANSWER_TEXT^*^**
[`unknown`](../../data-types.md) | `'Hello'` | Text of the quick response | `30` ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    B24.callMethod(
        'im.notify.answer',
        {
            ID: 270,
            ANSWER_TEXT: 'Hello'
        },
        res => {
            if (res.error())
            {
            console.error(result.error().ex);
            }
            else
            {
            console.log(res.data())
            }
        }
    )
    ```

- PHP (B24PhpSdk)

    ```php
    try {
        $notificationId = 123; // Replace with your actual notification ID
        $answerText = "This is an answer text"; // Replace with your actual answer text

        $result = $serviceBuilder
            ->getIMScope()
            ->notify()
            ->answer($notificationId, $answerText);

        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print("Failed to send answer.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Successful response

```json
{
    "result_message": [
        "Your response has been successfully sent."
    ]
}
```

An array of messages regarding your response is returned.

Example of a response if a notification identifier that does not support quick response is provided:

```json
{
    "result_message": false
}
```

## Error response

```json
{
    "error":"NOTIFY_ID_ERROR",
    "error_description":"Notification ID can't be empty"
}
```

### Key descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **ID_ERROR** | The `ID` parameter was not provided or is not a number ||
|| **ANSWER_TEXT_ERROR** | The `ANSWER_TEXT` parameter was not provided or is not a non-empty string ||
|#