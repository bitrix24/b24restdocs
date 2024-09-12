# Get a list of custom field types crm.userfield.types

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters and their types are not specified
- response in case of error and success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.types` returns descriptions of fields for custom fields.

List of custom field types. Contains descriptions of types:

- string
- integer
- double
- boolean
- datetime
- enumeration
- iblock_section
- iblock_element
- employee
- crm_status
- crm
- address
- money
- url

It will also return [types](../user-defined-field-types/index.md) of custom fields registered by the current application.

## Example

{% list tabs %}

- JS
  
    ```
    BX24.callMethod(
        "crm.userfield.types",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP
  
    ```
    require_once('crest.php');

    $result = CRest::call(
        'crm.userfield.types',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- CURL (oauth)

    ```
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/crm.userfield.types?auth=**put_access_token_here**

    ```

- CURL (webhook)

    ```
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.userfield.types
    ```

{% endlist %}

{% include [Examples note](../../../../_includes/examples.md) %}