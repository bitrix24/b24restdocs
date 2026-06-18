# Get the list of order properties sale.property.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

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
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for a substring at any position in the string.
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
[`integer`](../../data-types.md) | This parameter is used to manage pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the value of the `start` parameter:
 
`start = (N-1) * 50`, where `N` — the number of the desired page
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SalePropertyListResult = {
      properties: {
        id: number
        active: string
        code: string
        defaultValue: string
        description: string
        inputFieldLocation: string
        isAddress: string
        isAddressFrom: string
        isAddressTo: string
        isEmail: string
        isFiltered: string
        isLocation: string
        isPayer: string
        isPhone: string
        isProfileName: string
        isZip: string
        multiple: string
        name: string
        personTypeId: number
        propsGroupId: number
        required: string
        settings: unknown
        sort: number
        type: string
        userProps: string
        util: string
        xmlId: string
      }[]
    }

    try {
      // sale.property.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<SalePropertyListResult>({
        method: 'sale.property.list',
        params: {
          select: [
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
          filter: {
            '@type': 'STRING',
            '%code': 'EMAIL',
          },
          order: {
            id: 'desc',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Properties on page:', result.properties.length, result.properties[0]?.name)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function fetchPropertyList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.property.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.property.list',
            params: {
              select: [
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
              filter: {
                '@type': 'STRING',
                '%code': 'EMAIL',
              },
              order: {
                id: 'desc',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Properties on page:', result.properties.length, result.properties[0]?.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchPropertyList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.property.list',
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
        echo 'Error fetching sale properties: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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
      "date_start":"2024-04-11T10:01:21+02:00",
      "date_finish":"2024-04-11T10:01:21+02:00"
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
|| `200040300010` | Insufficient rights to read order properties ||
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

