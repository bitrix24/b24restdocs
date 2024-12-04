# Delete Notifications im.notify.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- links to pages that have not yet been created are not provided

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.delete` deletes a notification.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | `123` | Notification identifier | 18 ||
|| **TAG^*^**
[`unknown`](../../data-types.md) | `TEST` | Notification tag, unique within the system. | 18 ||
|| **SUB_TAG^*^**
[`unknown`](../../data-types.md) | `SUB`\|`TEST` | Additional tag, without uniqueness check | 18 ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

{% note warning %}

You need to specify **one of three** required parameters: `ID` (notification identifier), `TAG` (notification tag), or `SUB_TAG` (additional tag).

{% endnote %}

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'im.notify.delete',
        Array(
            'ID' => 13,
            'TAG' => 'TEST',
            'SUB_TAG' => 'SUB|TEST'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

- PHP (B24PhpSdk)

    ```php
    try {
        $notificationId = 123; // Replace with actual notification ID
        $notificationTag = null; // Replace with actual notification tag if needed
        $subTag = null; // Replace with actual sub tag if needed

        $result = $serviceBuilder->getIMScope()
            ->notify()
            ->delete($notificationId, $notificationTag, $subTag);

        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print("Failed to delete notification.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": true
}
```

**Execution result**: `true` or an error.

## Error Response

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "Error deleting notification"
}
```

### Key Descriptions:

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **PARAMS_ERROR** | Error deleting notification ||
|#

## Related Links

- [{#T}](../messages/attachments/index.md)