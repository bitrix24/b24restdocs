# Get a Link to the Invoice crm.invoice.getexternallink

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal Methods for Invoices`](../../universal/invoice.md)

{% endnote %}

The method returns a public link for the online invoice.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Invoice identifier ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_invoice_id"}' \ # Replace 'your_invoice_id' with the actual invoice ID
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.getexternallink
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_invoice_id","auth":"**put_access_token_here**"}' \ # Replace 'your_invoice_id' with the actual invoice ID
    https://**put_your_bitrix24_address**/rest/crm.invoice.getexternallink
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.invoice.getexternallink",
        { "id": id },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $id = 'your_invoice_id'; // Replace 'your_invoice_id' with the actual invoice ID

    $result = CRest::call(
        'crm.invoice.getexternallink',
        [
            'id' => $id
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}