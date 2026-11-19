# Update Order Property sale.property.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.property.update` updates the order property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_property.id`](../data-types.md#sale_order_property) | Identifier of the order property. You can retrieve it using the [sale.property.list](./sale-property-list.md) method ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the order property [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

The `type`, `personTypeId`, and `propsGroupId` fields cannot be changed. Pass the required `name` field with every update.

Omitted `Y/N` flags receive the default values listed below, and `settings` is rebuilt from the passed settings. To retain these values, pass them explicitly.

Updating a property removes its links to payment systems and delivery services. Restore the required links using the [sale.propertyRelation.*](../property-relation/index.md) methods.

General parameters for order properties of any type:

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the order property ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the order property ||
|| **active**
[`string`](../../data-types.md) | Indicator of the order property’s activity.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `Y` ||
|| **util**
[`string`](../../data-types.md) | Indicator of whether the order property is a service property. Service properties are not displayed in the public part.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **userProps**
[`string`](../../data-types.md) | Indicator of whether the order property is included in the customer profile.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isFiltered**
[`string`](../../data-types.md) | Indicator of whether the order property is available in the filter on the order list page.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **sort**
[`integer`](../../data-types.md) | Sort order. Default — `100` ||
|| **description**
[`string`](../../data-types.md) | Description of the order property ||
|| **required**
[`string`](../../data-types.md) | Indicator of whether filling in the order property value is mandatory.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **multiple**
[`string`](../../data-types.md) | Indicator of whether the order property is multiple. For multiple properties, it is possible to specify several values.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the order property ||
|| **defaultValue**
[`any`](../../data-types.md) | Default value of the order property.

For properties of type `FILE`, you need to provide an object in the format `{"fileData": ["value1", "value2"]}`:
- `value1` — file name with extension
- `value2` — file in [base64](../../files/how-to-upload-files.md) format

To delete a file, use the object in the format `{"remove": "Y"}`.

For multiple properties (`multiple: "Y"`), pass an array of values; for the `FILE` type, only the first successfully uploaded file from it is retained.

If you pass `null` or omit `defaultValue`, the default value is cleared ||
|| **settings**
[`object`](../../data-types.md) | An object in the format `{"field_1": "value_1", ... "field_N": "value_N"}` for passing additional settings for the order property.

The list of supported keys for this object depends on the type of property. For some property types (e.g., Y/N), additional properties are not provided. The description of the **settings** parameter for different property types is provided in the method description [`sale.property.add`](sale-property-add.md#settings) ||
|#

Parameters relevant for order properties of type `STRING`

#|
|| **Name**
`type` | **Description** ||
|| **isProfileName**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as the user profile name.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isPayer**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as the payer's name.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isEmail**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as an e-mail (e.g., when registering a new user during checkout).
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isPhone**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as a phone number.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isZip**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as a postal code.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isAddress**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as an address.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|#

Parameters relevant for order properties of type `LOCATION`

#|
|| **Name**
`type` | **Description** ||
|| **isLocation**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as the buyer's location for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isLocation4tax**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as the buyer's location for determining tax rates.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **inputFieldLocation**
[`string`](../../data-types.md) | Identifier of an active `STRING` property of the same payer type for alternative location input. Used when `isLocation: "Y"` and `multiple: "N"`; default — `0` ||
|#

Parameters relevant for order properties of type `ADDRESS`

#|
|| **Name**
`type` | **Description** ||
|| **isAddressFrom**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as the buyer's address from where the order needs to be picked up for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isAddressTo**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as the buyer's address where the order needs to be delivered for calculating delivery costs.
Possible values: 
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":93,"fields":{"name":"Phone (for contacting the courier)","code":"PHONE","active":"Y","util":"N","userProps":"Y","isFiltered":"N","sort":500,"description":"property description","required":"Y","multiple":"N","settings":{"multiline":"Y","maxlength":100},"xmlId":"","defaultValue":"","isProfileName":"Y","isPayer":"Y","isEmail":"N","isPhone":"N","isZip":"N","isAddress":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.property.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":93,"fields":{"name":"Phone (for contacting the courier)","code":"PHONE","active":"Y","util":"N","userProps":"Y","isFiltered":"N","sort":500,"description":"property description","required":"Y","multiple":"N","settings":{"multiline":"Y","maxlength":100},"xmlId":"","defaultValue":"","isProfileName":"Y","isPayer":"Y","isEmail":"N","isPhone":"N","isZip":"N","isAddress":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.property.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PropertyUpdateResult = {
      property: {
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
        isLocation4tax: string
        isPayer: string
        isPhone: string
        isProfileName: string
        isZip: string
        multiple: string
        name: string
        personTypeId: number
        propsGroupId: number
        required: string
        settings: Record<string, unknown>
        sort: number
        type: string
        userProps: string
        util: string
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<PropertyUpdateResult>({
        method: 'sale.property.update',
        params: {
          id: 93,
          fields: {
            name: 'Phone (for contacting the courier)',
            code: 'PHONE',
            active: 'Y',
            util: 'N',
            userProps: 'Y',
            isFiltered: 'N',
            sort: 500,
            description: 'property description',
            required: 'Y',
            multiple: 'N',
            settings: {
              multiline: 'Y',
              maxlength: 100,
            },
            xmlId: '',
            defaultValue: '',
            isProfileName: 'Y',
            isPayer: 'Y',
            isEmail: 'N',
            isPhone: 'N',
            isZip: 'N',
            isAddress: 'N',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated property:', result.property.id, result.property.name)
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
      async function updateSaleProperty() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.property.update',
            params: {
              id: 93,
              fields: {
                name: 'Phone (for contacting the courier)',
                code: 'PHONE',
                active: 'Y',
                util: 'N',
                userProps: 'Y',
                isFiltered: 'N',
                sort: 500,
                description: 'property description',
                required: 'Y',
                multiple: 'N',
                settings: {
                  multiline: 'Y',
                  maxlength: 100,
                },
                xmlId: '',
                defaultValue: '',
                isProfileName: 'Y',
                isPayer: 'Y',
                isEmail: 'N',
                isPhone: 'N',
                isZip: 'N',
                isAddress: 'N',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated property:', result.property.id, result.property.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateSaleProperty)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "name": "Phone (for contacting the courier)",
        "code": "PHONE",
        "active": "Y",
        "util": "N",
        "userProps": "Y",
        "isFiltered": "N",
        "sort": 500,
        "description": "property description",
        "required": "Y",
        "multiple": "N",
        "settings": {
            "multiline": "Y",
            "maxlength": 100,
        },
        "xmlId": "",
        "defaultValue": "",
        "isProfileName": "Y",
        "isPayer": "Y",
        "isEmail": "N",
        "isPhone": "N",
        "isZip": "N",
        "isAddress": "N",
    }

    try:
        bitrix_response = client.sale.property.update(
            bitrix_id=93,
            fields=fields,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.property.update',
                [
                    'id' => 93,
                    'fields' => [
                        'name'          => 'Phone (for contacting the courier)',
                        'code'          => 'PHONE',
                        'active'        => 'Y',
                        'util'          => 'N',
                        'userProps'     => 'Y',
                        'isFiltered'    => 'N',
                        'sort'          => 500,
                        'description'   => 'property description',
                        'required'      => 'Y',
                        'multiple'      => 'N',
                        'settings'      => [
                            'multiline' => 'Y',
                            'maxlength' => 100,
                        ],
                        'xmlId'         => '',
                        'defaultValue'  => '',
                        'isProfileName' => 'Y',
                        'isPayer'       => 'Y',
                        'isEmail'       => 'N',
                        'isPhone'       => 'N',
                        'isZip'         => 'N',
                        'isAddress'     => 'N',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating sale property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.property.update', {
            id: 93,
            fields: {
                name: 'Phone (for contacting the courier)',
                code: 'PHONE',
                active: 'Y',
                util: 'N',
                userProps: 'Y',
                isFiltered: 'N',
                sort: 500,
                description: 'property description',
                required: 'Y',
                multiple: 'N',
                settings: {
                    multiline: 'Y',
                    maxlength: 100
                },
                xmlId: '',
                defaultValue: '',
                isProfileName: 'Y',
                isPayer: 'Y',
                isEmail: 'N',
                isPhone: 'N',
                isZip: 'N',
                isAddress: 'N',
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
        'sale.property.update',
        [
            'id' => 93,
            'fields' => [
                'name' => 'Phone (for contacting the courier)',
                'code' => 'PHONE',
                'active' => 'Y',
                'util' => 'N',
                'userProps' => 'Y',
                'isFiltered' => 'N',
                'sort' => 500,
                'description' => 'property description',
                'required' => 'Y',
                'multiple' => 'N',
                'settings' => [
                    'multiline' => 'Y',
                    'maxlength' => 100
                ],
                'xmlId' => '',
                'defaultValue' => '',
                'isProfileName' => 'Y',
                'isPayer' => 'Y',
                'isEmail' => 'N',
                'isPhone' => 'N',
                'isZip' => 'N',
                'isAddress' => 'N',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.property.update", b24.Params{
    	"id": 93,
    	"fields": b24.Params{
    		"name":         "Phone (for contacting the courier)",
    		"code":         "PHONE",
    		"active":       "Y",
    		"util":         "N",
    		"userProps":    "Y",
    		"isFiltered":   "N",
    		"sort":         500,
    		"description":  "property description",
    		"required":     "Y",
    		"multiple":     "N",
    		"settings": b24.Params{
    			"multiline": "Y",
    			"maxlength": 100,
    		},
    		"xmlId":         "",
    		"defaultValue":  "",
    		"isProfileName": "Y",
    		"isPayer":       "Y",
    		"isEmail":       "N",
    		"isPhone":       "N",
    		"isZip":         "N",
    		"isAddress":     "N",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.property.update: %w", err)
    }

    // The method wraps the response in an object with the "property" key.
    raw, ok := b24.Unwrap(res.Result, "property")
    if !ok {
    	return fmt.Errorf("no property key in the response")
    }

    var item struct {
    	Active             string `json:"active"`
    	Code               string `json:"code"`
    	DefaultValue       json.RawMessage `json:"defaultValue"`
    	Description        string `json:"description"`
    	ID                 b24.ID `json:"id"`
    	InputFieldLocation string `json:"inputFieldLocation"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Active, item.Code)
    ```

{% endlist %}

## Response on Success

HTTP Status: **200**

```json
{
   "result":{
      "property":{
         "active":"Y",
         "code":"PHONE",
         "defaultValue":"",
         "description":"property description",
         "id":93,
         "inputFieldLocation":"0",
         "isAddress":"N",
         "isAddressFrom":"N",
         "isAddressTo":"N",
         "isEmail":"N",
         "isFiltered":"N",
         "isLocation":"N",
         "isLocation4tax":"N",
         "isPayer":"Y",
         "isPhone":"N",
         "isProfileName":"Y",
         "isZip":"N",
         "multiple":"N",
         "name":"Phone (for contacting the courier)",
         "personTypeId":3,
         "propsGroupId":6,
         "required":"Y",
         "settings":{
            "maxlength":100,
            "multiline":"Y"
         },
         "sort":500,
         "type":"STRING",
         "userProps":"Y",
         "util":"N",
         "xmlId":""
      }
   },
   "time":{
      "start":1712818736.235335,
      "finish":1712818736.611224,
      "duration":0.3758888244628906,
      "processing":0.18679594993591309,
      "date_start":"2024-04-11T09:58:56+02:00",
      "date_finish":"2024-04-11T09:58:56+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.property**
[`sale_order_property`](../data-types.md#sale_order_property) | The updated order property. In addition to the input fields, it contains `id`, `type`, `personTypeId`, and `propsGroupId` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":"200840400001",
   "error_description":"property is not exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200840400001` | `property is not exists`

The order property being updated was not found ||
|| `200850000003` | Order property validation or save error. The reason is given in `error_description` ||
|| `200850000009` | `Require fields: isFiltered.`

`isFiltered` is not passed when `multiple: "Y"` ||
|| `200850000010` | `Allowed values: isFiltered - [N]`

When `multiple: "Y"`, `isFiltered` must be `N` ||
|| `200850000011` | `Require fields: multiple.`

`multiple` is not passed for the `LOCATION` type when `isLocation: "Y"` ||
|| `200850000012` | `Allowed values: multiple - [N]`

For the `LOCATION` type, when `isLocation: "Y"`, `multiple` must be `N` ||
|| `200850000013` | `Require fields: multiple.`

`multiple` is not passed for the `LOCATION` type when `isLocation4tax: "Y"` ||
|| `200850000014` | `Allowed values: multiple - [N]`

For the `LOCATION` type, when `isLocation4tax: "Y"`, `multiple` must be `N` ||
|| `200850000015` | `Require fields: require.`

`required` is not passed for the `STRING` type when `isProfileName: "Y"` ||
|| `200850000016` | `Allowed values: require - [Y]`

For the `STRING` type, when `isProfileName: "Y"`, `required` must be `Y` ||
|| `200850000017` | ``Wrong format of field `defaultValue` ``

Invalid `defaultValue` format for the `FILE` type ||
|| `200040300020` | `Access Denied`

Insufficient permissions to update the order property ||
|| `100` | The `id` or `fields` parameter is not specified ||
|| `0` | `Required fields: name`

The `name` field is not passed in `fields`, including when the `fields` object is empty ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-add.md)
- [{#T}](./sale-property-get.md)
- [{#T}](./sale-property-list.md)
- [{#T}](./sale-property-delete.md)
- [{#T}](./sale-property-get-fields-by-type.md)
