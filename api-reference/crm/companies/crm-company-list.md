# Get a list of companies by filter crm.company.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- missing examples
- missing success response
- missing error response

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.list` returns a list of companies based on the filter. It is an implementation of the list method for companies.

When querying, use masks:
- "*" - to select all fields (excluding custom and multiple fields)
- "UF_*" - to select all custom fields (excluding multiple fields)

There is no mask for selecting multiple fields. To select multiple fields, specify the required ones in the selection list ("PHONE", "EMAIL", etc.).

## Parameters

See the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

## Examples

**Searching for companies by industry and type**

{% list tabs %}

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.company.list',
        {
          order: { "DATE_CREATE": "ASC" },
          filter: { "INDUSTRY": "MANUFACTURING", "COMPANY_TYPE": "CUSTOMER" },
          select: [ "ID", "TITLE", "CURRENCY_ID", "REVENUE" ]
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.company.list', {
        order: { "DATE_CREATE": "ASC" },
        filter: { "INDUSTRY": "MANUFACTURING", "COMPANY_TYPE": "CUSTOMER" },
        select: [ "ID", "TITLE", "CURRENCY_ID", "REVENUE" ]
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.company.list', {
        order: { "DATE_CREATE": "ASC" },
        filter: { "INDUSTRY": "MANUFACTURING", "COMPANY_TYPE": "CUSTOMER" },
        select: [ "ID", "TITLE", "CURRENCY_ID", "REVENUE" ]
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
                'crm.company.list',
                [
                    'order'  => ['DATE_CREATE' => 'ASC'],
                    'filter' => ['INDUSTRY' => 'MANUFACTURING', 'COMPANY_TYPE' => 'CUSTOMER'],
                    'select' => ['ID', 'TITLE', 'CURRENCY_ID', 'REVENUE'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
        if ($result->more()) {
            $result->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching company list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.list",
        {
            order: { "DATE_CREATE": "ASC" },
            filter: { "INDUSTRY": "MANUFACTURING", "COMPANY_TYPE": "CUSTOMER" },
            select: [ "ID", "TITLE", "CURRENCY_ID", "REVENUE" ]
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

**Searching for a company by phone**

{% list tabs %}

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.company.list',
        {
          filter: { "PHONE": "555888" },
          select: [ "ID", "TITLE" ]
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.company.list', { filter: { "PHONE": "555888" }, select: [ "ID", "TITLE" ] }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.company.list', { filter: { "PHONE": "555888" }, select: [ "ID", "TITLE" ] }, 0)
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
                'crm.company.list',
                [
                    'filter' => ['PHONE' => '555888'],
                    'select' => ['ID', 'TITLE'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
        if ($result->more()) {
            $result->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching company list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.list",
        {
            filter: { "PHONE": "555888" },
            select: [ "ID", "TITLE" ]
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

{% include [Note about examples](../../../_includes/examples.md) %}

## Continue exploring

- [{#T}](../../../tutorials/crm/how-to-get-lists/search-by-phone-and-email.md)