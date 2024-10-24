# Connect an external open channel to the account imopenlines.network.join

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines, imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method connects an external open channel to the account.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`Type` | **Example** | **Description** | **Revision** ||
|| **CODE*** 
[`unknown`](../../data-types.md) | `ab515f5d85a8b844d484f6ea75a2e494` | Yes | Code for searching from the connectors page | 1 ||
|#

## Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    // example for JS

- PHP

    {% include [Explanation about restCommand](../../chat-bots/_includes/rest-command.md) %}
    
    ```php
    $result = restCommand(
        'imopenlines.network.join',
        Array(
            'CODE' => 'ab515f5d85a8b844d484f6ea75a2e494'
        ),
        $_REQUEST["auth"]
    );
    ```

{% endlist %}

## Success Response

```json
{
    "result": 42
}
```
**Execution result**: `ID` of the bot or an error.

## Error Response

```json
{
    "error": "NOT_FOUND",
    "error_description": "Open channel is not found"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#| 
|| Code | Description ||
|| **IMBOT_ERROR**| Bot management module is not installed ||
|| **NOT_FOUND** | Open channel not found ||
|| **INACTIVE** | Open channel is currently unavailable ||
|#