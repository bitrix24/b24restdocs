# Get a List of CRM Payer Types crm.persontype.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

This method returns a list of payer types.

{% note info %}

For payment systems used in CRM (for invoices, deals), payer types should be retrieved using the `crm.persontype.list` method. If a payment system is being created for orders, then the [`sale.persontype.list`](../../../sale/person-type/sale-person-type-list.md) method should be used.

{% endnote %}

#|
|| **Name**
`type` | **Description** ||
|| **order** | Sorting fields ||
|| **filter** | Filter fields ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"NAME":"CRM_COMPANY"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.persontype.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"NAME":"CRM_COMPANY"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.persontype.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small datasets (< 1000 items) due to high memory load.
    
    try {
      const response = await $b24.callListMethod(
        'crm.persontype.list',
        {
          order: { "ID": "ASC" },
          filter: {
            "NAME": "CRM_COMPANY",
          }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in chunks using an iterator. Use for large datasets for efficient memory consumption.
    
    try {
      const generator = $b24.fetchListMethod('crm.persontype.list', {
        order: { "ID": "ASC" },
        filter: {
          "NAME": "CRM_COMPANY",
        }
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.persontype.list', {
        order: { "ID": "ASC" },
        filter: {
          "NAME": "CRM_COMPANY",
        }
      }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.persontype.list',
                [
                    'order' => ['ID' => 'ASC'],
                    'filter' => [
                        'NAME' => 'CRM_COMPANY',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        if ($response->more()) {
            $response->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing payer types: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.persontype.list", {
            order: {"ID": "ASC"},
            filter: {
                "NAME": "CRM_COMPANY",
            }
        },
        function (result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.dir(result.data());
                if (result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.persontype.list',
        [
            'order' => ['ID' => 'ASC'],
            'filter' => ['NAME' => 'CRM_COMPANY']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}