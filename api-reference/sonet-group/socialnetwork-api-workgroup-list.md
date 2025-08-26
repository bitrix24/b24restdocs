# Get a list of workgroups socialnetwork.api.workgroup.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no response in case of error
- no response in case of success
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`socialnetwork`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of groups.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **filter** | Corresponds to the arFilter parameter for the API method CSocNetGroup::getList. | ||
|| **select** | An array defining the fields to be selected. Contains a list of fields that should be returned by the method. If the array is empty, the fields ID, SITE_ID, NAME, DESCRIPTION, DATE_CREATE, DATE_UPDATE, DATE_ACTIVITY, ACTIVE, VISIBLE, OPENED, CLOSED, SUBJECT_ID, OWNER_ID, KEYWORDS, IMAGE_ID, NUMBER_OF_MEMBERS, INITIATE_PERMS, SPAM_PERMS, SUBJECT_NAME will be selected. Any fields from the list of fields are allowed in the array. | ||
|| **IS_ADMIN** | When Y is passed, it checks if the current user is an administrator of the social network and, if so, disables permission checks when selecting groups. | ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'socialnetwork.api.workgroup.list',
        {
          filter: {
            ID: 157,
          },
          select: ['ID', 'NAME']
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('socialnetwork.api.workgroup.list', {
        filter: {
          ID: 157,
        },
        select: ['ID', 'NAME']
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the process of paginated data retrieval through the start parameter. Suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('socialnetwork.api.workgroup.list', {
        filter: {
          ID: 157,
        },
        select: ['ID', 'NAME']
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
                'socialnetwork.api.workgroup.list',
                [
                    'filter' => [
                        'ID' => 157,
                    ],
                    'select' => [ 'ID', 'NAME' ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching workgroup list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('socialnetwork.api.workgroup.list', {
        filter: {
            ID: 157,
        },
        select: [ 'ID', 'NAME' ]
    }, result => {
        console.log(result);
    });
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}