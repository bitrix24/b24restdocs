# Get Field Settings Description for Custom Field Type crm.userfield.settings.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- response in case of error and success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.settings.fields` returns the description of the settings fields for the custom field type.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **type** | The type of the custom field. Value from the list returned by the method [crm.userfield.types](crm-userfield-types.md).  ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS
  
    ```
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.userfield.settings.fields",
        {
            type: "string"
        },
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

    $type = 'string'; // Replace with the desired type

    $result = CRest::call(
        'crm.userfield.settings.fields',
        ['type' => $type]
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
    -d '{"type":"string"}' \
    https://**put_your_bitrix24_address**/rest/crm.userfield.settings.fields?auth=**put_access_token_here**
    ```

- CURL (webhook)

    ```
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"string"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.userfield.settings.fields
    ```

{% endlist %}

{% include [Example Note](../../../../_includes/examples.md) %}