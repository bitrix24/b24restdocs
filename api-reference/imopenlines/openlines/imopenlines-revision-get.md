# Get Information About API Revisions imopenlines.revision.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves information about the Open Lines API revisions.

No parameters.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'imopenlines.revision.get',
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

    {% include [Explanation about restCommand](../../chat-bots/_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'imopenlines.revision.get',
        array(),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in case of success

```json
{    
    "result": {
        "rest": 2,
        "web": 1,
        "mobile": 1
    }
}
```

**Description of keys**:

- `rest` – API revision for REST clients
- `web` – API revision for web/desktop client
- `mobile` – API revision for mobile client