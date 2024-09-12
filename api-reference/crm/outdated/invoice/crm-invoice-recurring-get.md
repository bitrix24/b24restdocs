# Get the recurring invoice settings by ID crm.invoice.recurring.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method returns the fields of the recurring invoice template settings by ID.

## Method Parameters

{% include [Note about required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | The ID of the recurring invoice template settings ||
|#

## Code Examples

{% include [Note about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_recurring_invoice_id"}' \ # Replace 'your_recurring_invoice_id' with the actual recurring invoice ID
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.recurring.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"your_recurring_invoice_id","auth":"**put_access_token_here**"}' \ # Replace 'your_recurring_invoice_id' with the actual recurring invoice ID
    https://**put_your_bitrix24_address**/rest/crm.invoice.recurring.get
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.invoice.recurring.get",
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

    $id = 'your_recurring_invoice_id'; // Replace 'your_recurring_invoice_id' with the actual recurring invoice ID

    $result = CRest::call(
        'crm.invoice.recurring.get',
        [
            'id' => $id
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}