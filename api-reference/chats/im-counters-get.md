# Get Counters im.counters.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.counters.get` retrieves counters.

Without parameters.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.counters.get',
        {},
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    {% include [Explanation about restCommand](./_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.counters.get',
        array(),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

{% endlist %}

{% include [Note about examples](../../_includes/examples.md) %}

## Response on Success

```json
{    
    "result": {
        "CHAT": {"18": 1},
        "DIALOG": {"1": 3, "5": 1},
        "LINES": {},
        "TYPE": {
            "ALL": 5,
            "CHAT": 1,
            "DIALOG": 4,
            "LINES": 0,
            "NOTIFY": 0
        }
    }
}
```

### Description of Keys

- `CHAT` – an object containing a list of chats with unread messages.
- `DIALOG` – an object containing a list of dialogs with unread messages.
- `LINES` – an object containing a list of open line chats with unread messages.
- `TYPE` – an object containing total counters:
  - `ALL` – total counter of all entities.
  - `CHAT` – total counter of chats.
  - `DIALOG` – total counter of dialogs.
  - `LINES` – total counter of open lines.
  - `NOTIFY` – total counter of notifications.