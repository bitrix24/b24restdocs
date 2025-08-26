# Get a list of shipment properties sale.shipmentproperty.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method retrieves a list of shipment properties.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see the fields of the [sale_shipment_property](../data-types.md) object).
 
If not provided or an empty array is passed, all available property fields will be selected.
 ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected shipment properties in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_shipment_property](../data-types.md) object.

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:

- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be passed. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol needs to be passed in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol needs to be passed in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected shipment properties in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_shipment_property](../data-types.md) object.
 
Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` — the desired page number
 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","active","code","defaultValue","description","inputFieldLocation","isAddress","isAddressFrom","isAddressTo","isEmail","isFiltered","isLocation","isLocation4tax","isPayer","isPhone","isProfileName","isZip","multiple","name","personTypeId","propsGroupId","required","settings","sort","type","userProps","util","xmlId"],"filter":{"@type":"STRING","%code":"EMAIL"},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentproperty.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","active","code","defaultValue","description","inputFieldLocation","isAddress","isAddressFrom","isAddressTo","isEmail","isFiltered","isLocation","isLocation4tax","isPayer","isPhone","isProfileName","isZip","multiple","name","personTypeId","propsGroupId","required","settings","sort","type","userProps","util","xmlId"],"filter":{"@type":"STRING","%code":"EMAIL"},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentproperty.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'sale.shipmentproperty.list',
        {
          "select": [
            "id",
            "active",
            "code",
            "defaultValue",
            "description",
            "inputFieldLocation",
            "isAddress",
            "isAddressFrom",
            "isAddressTo",
            "isEmail",
            "isFiltered",
            "isLocation",
            "isLocation4tax",
            "isPayer",
            "isPhone",
            "isProfileName",
            "isZip",
            "multiple",
            "name",
            "personTypeId",
            "propsGroupId",
            "required",
            "settings",
            "sort",
            "type",
            "userProps",
            "util",
            "xmlId",
          ],
          "filter": {
            "@type": "STRING",
            "%code": "EMAIL",
          },
          "order": {
            "id": "desc",
          }
        },
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('sale.shipmentproperty.list', {
        "select": [
          "id",
          "active",
          "code",
          "defaultValue",
          "description",
          "inputFieldLocation",
          "isAddress",
          "isAddressFrom",
          "isAddressTo",
          "isEmail",
          "isFiltered",
          "isLocation",
          "isLocation4tax",
          "isPayer",
          "isPhone",
          "isProfileName",
          "isZip",
          "multiple",
          "name",
          "personTypeId",
          "propsGroupId",
          "required",
          "settings",
          "sort",
          "type",
          "userProps",
          "util",
          "xmlId",
        ],
        "filter": {
          "@type": "STRING",
          "%code": "EMAIL",
        },
        "order": {
          "id": "desc",
        }
      }, 'id');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('sale.shipmentproperty.list', {
        "select": [
          "id",
          "active",
          "code",
          "defaultValue",
          "description",
          "inputFieldLocation",
          "isAddress",
          "isAddressFrom",
          "isAddressTo",
          "isEmail",
          "isFiltered",
          "isLocation",
          "isLocation4tax",
          "isPayer",
          "isPhone",
          "isProfileName",
          "isZip",
          "multiple",
          "name",
          "personTypeId",
          "propsGroupId",
          "required",
          "settings",
          "sort",
          "type",
          "userProps",
          "util",
          "xmlId",
        ],
        "filter": {
          "@type": "STRING",
          "%code": "EMAIL",
        },
        "order": {
          "id": "desc",
        }
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
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
                'sale.shipmentproperty.list',
                [
                    'select' => [
                        'id',
                        'active',
                        'code',
                        'defaultValue',
                        'description',
                        'inputFieldLocation',
                        'isAddress',
                        'isAddressFrom',
                        'isAddressTo',
                        'isEmail',
                        'isFiltered',
                        'isLocation',
                        'isLocation4tax',
                        'isPayer',
                        'isPhone',
                        'isProfileName',
                        'isZip',
                        'multiple',
                        'name',
                        'personTypeId',
                        'propsGroupId',
                        'required',
                        'settings',
                        'sort',
                        'type',
                        'userProps',
                        'util',
                        'xmlId',
                    ],
                    'filter' => [
                        '@type' => 'STRING',
                        '%code' => 'EMAIL',
                    ],
                    'order' => [
                        'id' => 'desc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching shipment properties: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.shipmentproperty.list", {
            "select": [
                "id",
                "active",
                "code",
                "defaultValue",
                "description",
                "inputFieldLocation",
                "isAddress",
                "isAddressFrom",
                "isAddressTo",
                "isEmail",
                "isFiltered",
                "isLocation",
                "isLocation4tax",
                "isPayer",
                "isPhone",
                "isProfileName",
                "isZip",
                "multiple",
                "name",
                "personTypeId",
                "propsGroupId",
                "required",
                "settings",
                "sort",
                "type",
                "userProps",
                "util",
                "xmlId",
            ],
            "filter": {
                "@type": "STRING",
                "%code": "EMAIL",
            },
            "order": {
                "id": "desc",
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.shipmentproperty.list',
        [
            'select' => [
                "id",
                "active",
                "code",
                "defaultValue",
                "description",
                "inputFieldLocation",
                "isAddress",
                "isAddressFrom",
                "isAddressTo",
                "isEmail",
                "isFiltered",
                "isLocation",
                "isLocation4tax",
                "isPayer",
                "isPhone",
                "isProfileName",
                "isZip",
                "multiple",
                "name",
                "personTypeId",
                "propsGroupId",
                "required",
                "settings",
                "sort",
                "type",
                "userProps",
                "util",
                "xmlId",
            ],
            'filter' => [
                "@type" => "STRING",
                "%code" => "EMAIL",
            ],
            'order' => [
                "id" => "desc",
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
   "result": {
      "properties": [
         {
            "active": "Y",
            "code": "EMAIL",
            "defaultValue": "",
            "description": "",
            "id": 47,
            "inputFieldLocation": "0",
            "isAddress": "N",
            "isAddressFrom": "N",
            "isAddressTo": "N",
            "isEmail": "Y",
            "isFiltered": "Y",
            "isLocation": "N",
            "isPayer": "N",
            "isPhone": "N",
            "isProfileName": "N",
            "isZip": "N",
            "multiple": "N",
            "name": "E-Mail",
            "personTypeId": 3,
            "propsGroupId": 5,
            "required": "Y",
            "settings": [],
            "sort": 300,
            "type": "STRING",
            "userProps": "Y",
            "util": "N",
            "xmlId": ""
         },
         {
            "active": "Y",
            "code": "EMAIL",
            "defaultValue": "",
            "description": "",
            "id": 33,
            "inputFieldLocation": "0",
            "isAddress": "N",
            "isAddressFrom": "N",
            "isAddressTo": "N",
            "isEmail": "Y",
            "isFiltered": "N",
            "isLocation": "N",
            "isPayer": "N",
            "isPhone": "N",
            "isProfileName": "N",
            "isZip": "N",
            "multiple": "N",
            "name": "E-Mail",
            "personTypeId": 4,
            "propsGroupId": 8,
            "required": "Y",
            "settings": {
               "size": 40
            },
            "sort": 250,
            "type": "STRING",
            "userProps": "Y",
            "util": "N",
            "xmlId": ""
         },
         {
            "active": "Y",
            "code": "EMAIL",
            "defaultValue": "",
            "description": "",
            "id": 21,
            "inputFieldLocation": "0",
            "isAddress": "N",
            "isAddressFrom": "N",
            "isAddressTo": "N",
            "isEmail": "Y",
            "isFiltered": "Y",
            "isLocation": "N",
            "isPayer": "N",
            "isPhone": "N",
            "isProfileName": "N",
            "isZip": "N",
            "multiple": "N",
            "name": "E-Mail",
            "personTypeId": 3,
            "propsGroupId": 5,
            "required": "Y",
            "settings": {
               "size": 40
            },
            "sort": 110,
            "type": "STRING",
            "userProps": "Y",
            "util": "N",
            "xmlId": ""
         }
      ]
   },
   "total": 3,
   "time": {
      "start": 1712818881.719586,
      "finish": 1712818881.960037,
      "duration": 0.24045109748840332,
      "processing": 0.06902408599853516,
      "date_start": "2024-04-11T10:01:21+02:00",
      "date_finish": "2024-04-11T10:01:21+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **properties**
[`sale_shipment_property[]`](../data-types.md) | An array of objects with information about the selected properties ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error": 0,
   "error_description": "error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient rights to read shipment properties ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-property-add.md)
- [{#T}](./sale-shipment-property-update.md)
- [{#T}](./sale-shipment-property-get.md)
- [{#T}](./sale-shipment-property-delete.md)
- [{#T}](./sale-shipment-property-get-fields-by-type.md)