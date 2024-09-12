# Get the list of order properties sale.property.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of order properties.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see the fields of the [sale_order_property](../data-types.md) object).
 
If not provided or an empty array is passed, all available property fields will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected payment bindings to shipments in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_order_property](../data-types.md) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:

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
[`object`](../../data-types.md) | An object for sorting the selected items in the shipment table in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_order_property](../data-types.md).
 
Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` is the desired page number
 ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","active","code","defaultValue","description","inputFieldLocation","isAddress","isAddressFrom","isAddressTo","isEmail","isFiltered","isLocation","isLocation4tax","isPayer","isPhone","isProfileName","isZip","multiple","name","personTypeId","propsGroupId","required","settings","sort","type","userProps","util","xmlId"],"filter":{"@type":"STRING","%code":"EMAIL"},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.property.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","active","code","defaultValue","description","inputFieldLocation","isAddress","isAddressFrom","isAddressTo","isEmail","isFiltered","isLocation","isLocation4tax","isPayer","isPhone","isProfileName","isZip","multiple","name","personTypeId","propsGroupId","required","settings","sort","type","userProps","util","xmlId"],"filter":{"@type":"STRING","%code":"EMAIL"},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.property.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.property.list", {
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.property.list',
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
   "result":{
      "properties":[
         {
            "active":"Y",
            "code":"EMAIL",
            "defaultValue":"",
            "description":"",
            "id":47,
            "inputFieldLocation":"0",
            "isAddress":"N",
            "isAddressFrom":"N",
            "isAddressTo":"N",
            "isEmail":"Y",
            "isFiltered":"Y",
            "isLocation":"N",
            "isPayer":"N",
            "isPhone":"N",
            "isProfileName":"N",
            "isZip":"N",
            "multiple":"N",
            "name":"E-Mail",
            "personTypeId":3,
            "propsGroupId":5,
            "required":"Y",
            "settings":[
               
            ],
            "sort":300,
            "type":"STRING",
            "userProps":"Y",
            "util":"N",
            "xmlId":""
         },
         {
            "active":"Y",
            "code":"EMAIL",
            "defaultValue":"",
            "description":"",
            "id":33,
            "inputFieldLocation":"0",
            "isAddress":"N",
            "isAddressFrom":"N",
            "isAddressTo":"N",
            "isEmail":"Y",
            "isFiltered":"N",
            "isLocation":"N",
            "isPayer":"N",
            "isPhone":"N",
            "isProfileName":"N",
            "isZip":"N",
            "multiple":"N",
            "name":"E-Mail",
            "personTypeId":4,
            "propsGroupId":8,
            "required":"Y",
            "settings":{
               "size":40
            },
            "sort":250,
            "type":"STRING",
            "userProps":"Y",
            "util":"N",
            "xmlId":""
         },
         {
            "active":"Y",
            "code":"EMAIL",
            "defaultValue":"",
            "description":"",
            "id":21,
            "inputFieldLocation":"0",
            "isAddress":"N",
            "isAddressFrom":"N",
            "isAddressTo":"N",
            "isEmail":"Y",
            "isFiltered":"Y",
            "isLocation":"N",
            "isPayer":"N",
            "isPhone":"N",
            "isProfileName":"N",
            "isZip":"N",
            "multiple":"N",
            "name":"E-Mail",
            "personTypeId":3,
            "propsGroupId":5,
            "required":"Y",
            "settings":{
               "size":40
            },
            "sort":110,
            "type":"STRING",
            "userProps":"Y",
            "util":"N",
            "xmlId":""
         }
      ]
   },
   "total":3,
   "time":{
      "start":1712818881.719586,
      "finish":1712818881.960037,
      "duration":0.24045109748840332,
      "processing":0.06902408599853516,
      "date_start":"2024-04-11T10:01:21+03:00",
      "date_finish":"2024-04-11T10:01:21+03:00"
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
[`sale_order_property[]`](../data-types.md) | An array of objects with information about the selected properties ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read order properties ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-add.md)
- [{#T}](./sale-property-update.md)
- [{#T}](./sale-property-get.md)
- [{#T}](./sale-property-delete.md)
- [{#T}](./sale-property-get-fields-by-type.md)