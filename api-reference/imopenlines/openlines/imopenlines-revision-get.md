# Get Information About API Revisions imopenlines.revision.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

No parameters required.

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.revision.get',
            {}
        );
        
        const result = response.getData().result;
        console.log(result);
    }
    catch( error )
    {
        console.error('Error:', error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.revision.get',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error()->ex;
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting revision: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

    {% include [Explanation about restCommand](../../chat-bots/_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'imopenlines.revision.get',
        Array(),
        $_REQUEST["auth"]
    );
    ```

- cURL

    // example for cURL

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on Success

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