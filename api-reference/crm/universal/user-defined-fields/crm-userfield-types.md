# Get a list of user field types crm.userfield.types

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters and their types are not specified
- no response in case of error and success

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.types` returns the description of fields for user fields.

List of user field types. Contains descriptions of types:

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

Also, the [types](../user-defined-fields/userfield-type.md) of user fields registered by the current application will be returned.

## Example

{% list tabs %}

- JS
  
    ```js
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
  
    ```php
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

- PHP (B24PhpSdk)

    ```php
    try {
        $userfieldService = $serviceBuilder->getCRMScope()->userfield();
        $userfieldTypesResult = $userfieldService->types();

        foreach ($userfieldTypesResult->getTypes() as $item) {
            print("ID: " . $item->ID . "\n");
            print("Title: " . $item->title . "\n");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage() . "\n");
    }
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

## Continue exploring

- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-precision-to-user-field.md)