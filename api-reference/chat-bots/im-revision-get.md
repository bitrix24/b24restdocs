# Get Information About API Revisions im.revision.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.revision.get` retrieves information about API revisions.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```javascript
    BX24.callMethod(
        'im.revision.get',
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
        'im.revision.get',
        Array(

        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}

## Response on Success

```json
{    
    "result": {
        "rest": 18,
        "web": 117,
        "mobile": 9,
    }
}
```

### Description of Keys

- `rest` – API revision for REST clients.
- `web` – API revision for web/desktop client.
- `mobile` – API revision for mobile client.