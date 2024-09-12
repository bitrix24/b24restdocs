# Add Search to History im.search.last.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.last.add` adds an item to the history of the last search.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `chat17`
or
`256` | Identifier of the dialog. Format: **chatXXX** – chat of the recipient if the message is for a chat, or **XXX** – identifier of the recipient if the message is for a private dialog | 18 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.search.last.add',
        {
            'DIALOG_ID': 'chat17'
        },
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

    {% include [Explanation of restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.search.last.add',
        Array(
            'DIALOG_ID' => 'chat17'
        ),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result": true
}
```

## Error Response

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **DIALOG_ID_EMPTY** | Dialog identifier was not provided. ||
|#