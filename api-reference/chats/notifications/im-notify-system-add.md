# Send System Notification im.notify.system.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `im.notify.system.add` sends a system notification.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **USER_ID^*^**
[`unknown`](../../data-types.md) | `1` | Identifier of the user to whom the notification will be addressed | 18 ||
|| **MESSAGE^*^**
[`unknown`](../../data-types.md) | System notification | Text of the notification | 18 ||
|| **MESSAGE_OUT**
[`unknown`](../../data-types.md) | Text of the system notification for email | Text of the notification for email. If not specified, the MESSAGE field is used | 18 ||
|| **TAG**
[`unknown`](../../data-types.md) | `TEST` | Notification tag, unique within the system. When adding a notification with an existing tag, other notifications will be deleted | 18 ||
|| **SUB_TAG**
[`unknown`](../../data-types.md) | `SUB`\|`TEST` | Additional tag, without uniqueness check | 18 ||
|| **ATTACH**
[`unknown`](../../data-types.md) | | Attachment | 18 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% include [Explanation of restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'im.notify.system.add',
    Array(
        'USER_ID' => 1,
        'MESSAGE' => 'System notification',
        'MESSAGE_OUT' => 'Text of the system notification for email',
        'TAG' => 'TEST',
        'SUB_TAG' => 'SUB|TEST',
        'ATTACH' => 'Array()'
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Example Notes](../../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": 123
}
```

**Execution result**: notification identifier `ID` or error.

## Response on Error

```json
{
    "error": "USER_ID_EMPTY",
    "error_description": "Recipient identifier is not specified"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **USER_ID_EMPTY** | Recipient identifier is not specified ||
|| **MESSAGE_EMPTY** | Message text is not provided ||
|| **ATTACH_ERROR** | The entire provided attachment object failed validation ||
|| **ATTACH_OVERSIZE** | The maximum allowable attachment size has been exceeded (30 KB) ||
|#

## Related Links

- [{#T}](../messages/attachments/index.md)