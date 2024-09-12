# Get the identifier of the directory where the stages of deals are stored crm.dealcategory.status

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use the funnel methods [`crm.category.*`](../../universal/category/index.md)

{% endnote %}

The method returns the identifier of the directory for storing stages based on the deal direction identifier.

This is a string of the form `DEAL_STAGE_[Direction Identifier]`. For example, for a direction with identifier 1, the string `"DEAL_STAGE_1"` will be returned.

The identifier is intended for use with the family of methods [`crm.status.*`](.). For example, to create a new stage for a direction, it needs to be passed to the method [`crm.status.add`](../../status/crm-status-add.md) as the `ENTITY_ID` parameter.

# Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md)| Direction identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"1"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.dealcategory.status
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"1","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.dealcategory.status
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.dealcategory.status",
        { id: id },
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

    $id = 1; // Replace 1 with the actual ID

    $result = CRest::call(
        'crm.dealcategory.status',
        [
            'id' => $id
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}