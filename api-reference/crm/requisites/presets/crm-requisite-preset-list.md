# Get a list of requisites templates by filter crm.requisite.preset.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of requisites templates based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array of fields to select (see [template fields](#fields)).

If the array is not provided or an empty array is passed, all available template fields will be selected. ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering selected templates in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to [template fields](#fields).

An additional prefix can be set for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal 
||
|| **order**
[`object`](../../../data-types.md) | An object for sorting selected templates in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to [template fields](#fields).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

### Description of requisites template fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the requisite. Automatically created and unique within the account. ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object's type.

The identifiers of CRM object types are provided by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md). ||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of requisites template fields (to get available values, see the method [crm.requisite.preset.countries](./crm-requisite-preset-countries.md)). ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Creation date. ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date. Contains an empty string if the template has not been modified since creation. ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite. ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the requisite. ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the requisite. ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information base. 

The purpose of the field may change by the final developer. 

Each application ensures the uniqueness of values in this field. It is recommended to use a unique prefix to avoid collisions with other applications. 

Values of the form `#CRM_REQUISITE_PRESET_DEF_...` are reserved in CRM for identifying templates that are used by default. These identifiers should not be used for your purposes, as this may lead to logic violations. ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status. Uses values `Y` or `N`. Determines the availability of the template in the selection list when adding requisites. ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting. ||
|#

## Code Examples

{% include [Examples note](../../../../_includes/examples.md) %}

Searching for templates by country binding:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"COUNTRY_ID":"1"},"select":["ID","NAME"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"ASC"},"filter":{"COUNTRY_ID":"1"},"select":["ID","NAME"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to get the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.requisite.preset.list',
        {
          order: { "ID": "ASC" },
          filter: { "COUNTRY_ID": "1"},
          select: [ "ID", "NAME"]
        },
        (progress) => { 
          if(progress.error())
            console.error(progress.error());
          else
          {
            console.dir(progress.data());
            if(progress.more())
              progress.next();
          }
        }
      )
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.requisite.preset.list', { order: { "ID": "ASC" }, filter: { "COUNTRY_ID": "1"}, select: [ "ID", "NAME"] }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { 
          console.dir(entity);
        }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.requisite.preset.list', { order: { "ID": "ASC" }, filter: { "COUNTRY_ID": "1"}, select: [ "ID", "NAME"] }, 0)
      const result = response.getData().result || []
      for (const entity of result) { 
        console.dir(entity);
      }
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
                'crm.requisite.preset.list',
                [
                    'order'  => ['ID' => 'ASC'],
                    'filter' => ['COUNTRY_ID' => '1'],
                    'select' => ['ID', 'NAME'],
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
        echo 'Error fetching requisite presets: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.preset.list",
        {
            order: { "ID": "ASC" },
            filter: { "COUNTRY_ID": "1"},
            select: [ "ID", "NAME"]
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.preset.list',
        [
            'order' => ['ID' => 'ASC'],
            'filter' => ['COUNTRY_ID' => '1'],
            'select' => ['ID', 'NAME']
        ]
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
            "ID": "1",
            "NAME": "Organization"
        },
        {
            "ID": "2",
            "NAME": "Sole Proprietor"
        },
        {
            "ID": "3",
            "NAME": "Individual"
        },
        {
            "ID": "4",
            "NAME": "Organization (additional)"
        }
    ],
    "total": 4,
    "time": {
        "start": 1716564019.674636,
        "finish": 1716564020.067474,
        "duration": 0.3928380012512207,
        "processing": 0.02863287925720215,
        "date_start": "2024-05-24T17:20:19+02:00",
        "date_finish": "2024-05-24T17:20:20+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md)| An array of objects with information about the selected templates. Each element contains the selected [template fields](#fields). ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found. ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error":0,
    "error_description":"Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `Access denied` | Insufficient access permissions to retrieve the list of templates. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-requisite-preset-add.md)
- [{#T}](./crm-requisite-preset-update.md)
- [{#T}](./crm-requisite-preset-countries.md)
- [{#T}](./crm-requisite-preset-get.md)
- [{#T}](./crm-requisite-preset-delete.md)
- [{#T}](./crm-requisite-preset-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-company-with-requisite.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contact-with-requisite.md)