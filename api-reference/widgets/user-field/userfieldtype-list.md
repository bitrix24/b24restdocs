# Get a list of registered user field types userfieldtype.list

> Scope: [`depending on the embedding location`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method retrieves a list of user field types registered by the application. It returns a list of field types with pagination.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldtype.list
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/userfieldtype.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'userfieldtype.list',
        {},
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('userfieldtype.list', {}, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. It is suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    try {
      const response = await $b24.callMethod('userfieldtype.list', {}, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php        
    try {
        $userFieldTypesResult = $serviceBuilder->getPlacementScope()->userFieldType()->list();
        $userFieldTypes = $userFieldTypesResult->getUserFieldTypes();
        foreach ($userFieldTypes as $userFieldType) {
            print("Description: " . $userFieldType->DESCRIPTION . "\n");
            print("Handler: " . $userFieldType->HANDLER . "\n");
            print("Title: " . $userFieldType->TITLE . "\n");
            print("User Type ID: " . $userFieldType->USER_TYPE_ID . "\n");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'userfieldtype.list',
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldtype.list',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "USER_TYPE_ID": "my_custom_type_2",
            "HANDLER": "http:\/\/test.com\/test2.php",
            "TITLE": "test title 2",
            "DESCRIPTION":"test desc 2"
        },
        {
            "USER_TYPE_ID": "my_custom_type_1",
            "HANDLER": "http:\/\/test.com\/test1.php",
            "TITLE": "test title 1",
            "DESCRIPTION": "test desc 1"
        },
        {
            "USER_TYPE_ID": "test_user_type",
            "HANDLER": "http:\/\/test.com\/test.php",
            "TITLE": "test title",
            "DESCRIPTION": "test desc"
        }
    ],
    "total": 3,
    "time": {
        "start": 1724423274.842117,
        "finish": 1724423275.558021,
        "duration": 0.7159039974212646,
        "processing": 0.0018908977508544922,
        "date_start": "2024-08-23T16:27:54+02:00",
        "date_finish": "2024-08-23T16:27:55+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **total**
[`integer`](../../data-types.md) | Number of processed records ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Continue Learning

- [{#T}](./userfieldtype-add.md)
- [{#T}](./userfieldtype-update.md)
- [{#T}](./userfieldtype-delete.md)