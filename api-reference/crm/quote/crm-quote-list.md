# Get a list of estimates by filter crm.quote.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters need to be described here
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method Development Stopped" %}

The method `crm.quote.list` continues to function, but there is a more relevant alternative [crm.item.list](../universal/crm-item-list.md).

{% endnote %}

The method `crm.quote.list` returns a list of estimates by filter. It is an implementation of the list method for estimates.

See the description of [list methods](../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

## Example

{% list tabs %}

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'crm.quote.list',
        {
          order: { "STATUS_ID": "ASC" },
          filter: { "=COMPANY_ID": 1 },
          select: [ "ID", "TITLE", "STATUS_ID", "OPPORTUNITY", "CURRENCY_ID" ]
        },
        (progress) => { 
          if (progress.error()) {
            console.error(progress.error());
          } else {
            console.dir(progress.data());
            if (progress.more()) {
              progress.next();
            }
          }
        }
      );
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('crm.quote.list', { order: { "STATUS_ID": "ASC" }, filter: { "=COMPANY_ID": 1 }, select: [ "ID", "TITLE", "STATUS_ID", "OPPORTUNITY", "CURRENCY_ID" ] }, 'ID');
      for await (const page of generator) {
        for (const entity of page) {
          console.dir('Entity:', entity);
        }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.quote.list', { order: { "STATUS_ID": "ASC" }, filter: { "=COMPANY_ID": 1 }, select: [ "ID", "TITLE", "STATUS_ID", "OPPORTUNITY", "CURRENCY_ID" ] }, 0);
      const result = response.getData().result || [];
      for (const entity of result) {
        console.dir('Entity:', entity);
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.list',
                [
                    'order'  => ['STATUS_ID' => 'ASC'],
                    'filter' => ['=COMPANY_ID' => 1],
                    'select' => ['ID', 'TITLE', 'STATUS_ID', 'OPPORTUNITY', 'CURRENCY_ID'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            print_r($result->data());
            if ($result->more()) {
                $result->next();
            }
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching quote list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "crm.quote.list",
        {
            order: { "STATUS_ID": "ASC" },
            filter: { "=COMPANY_ID": 1 },
            select: [ "ID", "TITLE", "STATUS_ID", "OPPORTUNITY", "CURRENCY_ID" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

