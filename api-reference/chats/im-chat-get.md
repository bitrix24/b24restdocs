# Get Chat Identifier im.chat.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- response in case of error is absent
- links to pages that are not yet created are not provided
- from Sergei's file: it's unclear in what context the method is applicable, needs clarification

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.get` retrieves the chat identifier.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ENTITY_TYPE^*^**
[`unknown`](../data-types.md) | `CRM` | Identifier of the entity. Can be used to find the chat and to easily determine the context in event handlers:
- [ONIMBOTMESSAGEADD](.),
- [ONIMBOTMESSAGEUPDATE](.),
- [ONIMBOTMESSAGEDELETE](.) | 18 ||
|| **ENTITY_ID^*^**
[`unknown`](../data-types.md) | `LEAD`\|`13` | Numeric identifier of the entity. Can be used to find the chat and to easily determine the context in event handlers:
- [ONIMBOTMESSAGEADD](.),
- [ONIMBOTMESSAGEUPDATE](.),
- [ONIMBOTMESSAGEDELETE](.) | 18 ||
|#

{% include [Note on parameters](../../_includes/required.md) %}

## Examples

{% include [Explanation about restCommand](./_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'im.chat.get',
        Array(
            'ENTITY_TYPE' => 'CRM',
            'ENTITY_ID' => 'LEAD|13',
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Note on examples](../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": 10
}
```

**Execution result**: returns the chat identifier `CHAT_ID` or `null`.