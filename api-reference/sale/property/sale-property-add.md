# Add Order Property sale.property.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.property.add` adds an order property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating an order property [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

Common parameters applicable to order properties of any type. When `multiple` = `Y`, explicitly pass `isFiltered` = `N`.

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **personTypeId***
[`sale_person_type.id`](../data-types.md#sale_person_type) | Identifier of the payer type ||
|| **propsGroupId***
[`sale_order_property_group.id`](../data-types.md#sale_order_property_group) | Identifier of the property group for the specified payer type ||
|| **name***
[`string`](../../data-types.md) | Name of the order property ||
|| **type***
[`string`](../../data-types.md) | Type of the order property.
Possible values:
- `STRING`
- `Y/N`
- `NUMBER`
- `ENUM`
- `FILE`
- `DATE`
- `LOCATION`
- `ADDRESS`
 ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the order property ||
|| **active**
[`string`](../../data-types.md) | Indicator of the order property’s activity.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `Y` (the property is added as active) ||
|| **util**
[`string`](../../data-types.md) | Indicator of whether the order property is a system property. System properties are not displayed in the public part.
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
[`integer`](../../data-types.md) | Sorting. Default — `100` ||
|| **description**
[`string`](../../data-types.md) | Description of the order property ||
|| **required**
[`string`](../../data-types.md) | Indicator of whether the order property value is required.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **multiple**
[`string`](../../data-types.md) | Indicator of whether the order property is multiple. For multiple properties, several values can be specified.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the order property ||
|| **defaultValue**
[`any`](../../data-types.md) | Default value of the order property.

For properties of type `FILE`, pass an object in the format `{"fileData": ["value1", "value2"]}`:
- `value1` — file name with extension
- `value2` — file in [base64](../../files/how-to-upload-files.md) format

For multiple order properties (`multiple` = `Y`), an array of values is supported. For the `FILE` type, only the first successfully uploaded file from the array is retained ||
|| **settings**
[`object`](../../data-types.md) | An object in the format `{"field_1": "value_1", ... "field_N": "value_N"}` for passing additional settings for the order property.

The list of supported keys for this object depends on the property type. For some property types (e.g., `Y/N`), additional properties are not provided. The description of the **settings** parameter for different property types is provided [below](#settings) ||
|#

Parameters applicable to order properties of type `STRING`. When `isProfileName` = `Y`, explicitly pass `required` = `Y`.

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

If not provided, the default value is `N`||
|| **isEmail**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as an e-mail (e.g., when registering a new user during order placement).
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N`||
|| **isPhone**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as a phone number.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N`||
|| **isZip**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as a postal code.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N`||
|| **isAddress**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as an address.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N`||
|#

Parameters applicable to order properties of type `LOCATION`. When `isLocation` = `Y` or `isLocation4tax` = `Y`, explicitly pass `multiple` = `N`.

#|
||**Name**
`type` | **Description** ||
|| **isLocation**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as the buyer's location for calculating shipping costs.
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
[`string`](../../data-types.md) | Identifier of the string property for entering the location manually. Retained when `isLocation` = `Y` and `multiple` = `N`; default — `"0"` ||
|#

Parameters applicable to order properties of type `ADDRESS`

#|
|| **Name**
`type` | **Description** ||
|| **isAddressFrom**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as the buyer's address from which the order needs to be picked up for calculating shipping costs.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **isAddressTo**
[`string`](../../data-types.md) | Indicator of whether to use the value of this order property as the buyer's address to which the order needs to be delivered for calculating shipping costs.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|#

### Parameter settings {#settings}

Parameters applicable to order properties of type `STRING`

#|
|| **Name**
`type` | **Description** ||
|| **minlength**
[`integer`](../../data-types.md) | Minimum allowable length (number of characters) of the order property value ||
|| **maxlength**
[`integer`](../../data-types.md) | Maximum allowable length (number of characters) of the order property value ||
|| **pattern**
[`string`](../../data-types.md) | Regular expression for validating the order property value.
Examples:
Regular expression for validating a phone number ```^((8|\+1)[\- ]?)?(\(?\d{3}\)?[\- ]?)?[\d\- ]{7,10}$```
Regular expression for validating date format DD/MM/YYYY:
```^(0?[1-9]|[12][0-9]|3[01])[\/\-](0?[1-9]|1[012])[\/\-]\d{4}$``` ||
|| **multiline**
[`string`](../../data-types.md) | Indicator of displaying a multiline input field for the property value. Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **cols**
[`integer`](../../data-types.md) | Width of the multiline field in characters. Retained when `multiline` = `Y` ||
|| **rows**
[`integer`](../../data-types.md) | Number of rows in the multiline field. Retained when `multiline` = `Y` ||
|| **size**
[`integer`](../../data-types.md) | Width of the single-line field in characters. Retained when `multiline` is not `Y` ||
|#

Parameters applicable to order properties of type `NUMBER`

#|
|| **Name**
`type` | **Description** ||
|| **min**
[`integer`](../../data-types.md) | Minimum allowable value for this order property ||
|| **max**
[`integer`](../../data-types.md) | Maximum allowable value for this order property ||
|| **step**
[`integer`](../../data-types.md) | Step for changing the value. Used in some user interfaces for convenience in changing the order property value ||
|#

Parameters applicable to order properties of type `ENUM`

#|
|| **Name**
`type` | **Description** ||
|| **multielement**
[`string`](../../data-types.md) | Indicator of displaying the order property options as radio buttons, or as checkboxes when `multiple` = `Y`.
Value is used in some user interfaces.
Possible values:
- `Y` — yes
- `N` — no

If not provided, the default value is `N` ||
|| **size**
[`integer`](../../data-types.md) | Number of visible rows in the list. Retained when `multielement` is not `Y` ||
|#

Parameters applicable to order properties of type `FILE`

#|
|| **Name**
`type` | **Description** ||
|| **maxsize**
[`integer`](../../data-types.md) | Maximum allowable size of the uploaded file in bytes ||
|| **accept**
[`string`](../../data-types.md) | List of file extensions that are allowed to be uploaded for this order property value. Example: png, doc, zip ||
|#

Parameters applicable to order properties of type `DATE`

#|
|| **Name**
`type` | **Description** ||
|| **time**
[`string`](../../data-types.md) | Indicator of whether to add the option to select time when working with the value of this order property. Value is used in some user interfaces.
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
    -d '{"fields":{"personTypeId":3,"propsGroupId":6,"name":"Phone (for contacting the courier)","type":"STRING","code":"PHONE","active":"Y","util":"N","userProps":"Y","isFiltered":"N","sort":500,"description":"property description","required":"Y","multiple":"N","settings":{"multiline":"Y","maxlength":100},"xmlId":"","defaultValue":"","isProfileName":"Y","isPayer":"Y","isEmail":"N","isPhone":"N","isZip":"N","isAddress":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.property.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"personTypeId":3,"propsGroupId":6,"name":"Phone (for contacting the courier)","type":"STRING","code":"PHONE","active":"Y","util":"N","userProps":"Y","isFiltered":"N","sort":500,"description":"property description","required":"Y","multiple":"N","settings":{"multiline":"Y","maxlength":100},"xmlId":"","defaultValue":"","isProfileName":"Y","isPayer":"Y","isEmail":"N","isPhone":"N","isZip":"N","isAddress":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.property.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PropertyAddResult = {
      property: {
        active: string
        code: string
        defaultValue: string
        description: string
        id: number
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
      const response = await $b24.actions.v2.call.make<PropertyAddResult>({
        method: 'sale.property.add',
        params: {
          fields: {
            personTypeId: 3,
            propsGroupId: 6,
            name: 'Phone (for contacting the courier)',
            type: 'STRING',
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
        console.info('Added property id:', result.property.id, 'name:', result.property.name)
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
      async function addProperty() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.property.add',
            params: {
              fields: {
                personTypeId: 3,
                propsGroupId: 6,
                name: 'Phone (for contacting the courier)',
                type: 'STRING',
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
          console.info('Added property id:', result.property.id, 'name:', result.property.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addProperty)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "personTypeId": 3,
        "propsGroupId": 6,
        "name": "Phone (for contacting the courier)",
        "type": "STRING",
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
        bitrix_response = client.sale.property.add(
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
                'sale.property.add',
                [
                    'fields' => [
                        'personTypeId'  => 3,
                        'propsGroupId'  => 6,
                        'name'          => 'Phone (for contacting the courier)',
                        'type'          => 'STRING',
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
                            'maxlength' => 100
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
        echo 'Error adding property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'sale.property.add', {
        fields: {
            personTypeId: 3,
            propsGroupId: 6,
            name: 'Phone (for contacting the courier)',
            type: 'STRING',
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
        'sale.property.add',
        [
            'fields' => [
                'personTypeId' => 3,
                'propsGroupId' => 6,
                'name' => 'Phone (for contacting the courier)',
                'type' => 'STRING',
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
    res, err := client.Core().Call(ctx, "sale.property.add", b24.Params{
    	"fields": b24.Params{
    		"personTypeId": 3,
    		"propsGroupId": 6,
    		"name":         "Phone (for contacting the courier)",
    		"type":         "STRING",
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
    	return fmt.Errorf("sale.property.add: %w", err)
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

## Successful Response

HTTP Status: **200**

```json
{
   "result":{
      "property":{
         "active":"Y",
         "code":"PHONE",
         "defaultValue":"",
         "description":"property description",
         "id":96,
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
      "start":1712818563.754118,
      "finish":1712818566.840385,
      "duration":3.0862669944763184,
      "processing":1.0286660194396973,
      "date_start":"2024-04-11T09:56:03+02:00",
      "date_finish":"2024-04-11T09:56:06+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **property**
[`sale_order_property`](../data-types.md#sale_order_property) | Data of the added order property in `result.property`. The `id` field is the identifier of the created property; the other fields are described in the `fields` input parameter. For the `FILE` type, the `defaultValue` field contains information about the saved file ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":"0",
   "error_description":"Required fields: propsGroupId"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200850000005` | `person type id is empty`

An empty payer type value is specified ||
|| `200850000006` | Validation or save error message

The property could not be validated or saved, for example, the group does not belong to the specified payer type ||
|| `200850000009` | `Require fields: isFiltered.`

`isFiltered` is not passed when `multiple` = `Y` ||
|| `200850000010` | `Allowed values: isFiltered - [N]`

`isFiltered` other than `N` is passed when `multiple` = `Y` ||
|| `200850000011` | `Require fields: multiple.`

`multiple` is not passed for `LOCATION` with `isLocation` = `Y` ||
|| `200850000012` | `Allowed values: multiple - [N]`

`multiple` other than `N` is passed for `LOCATION` with `isLocation` = `Y` ||
|| `200850000013` | `Require fields: multiple.`

`multiple` is not passed for `LOCATION` with `isLocation4tax` = `Y` ||
|| `200850000014` | `Allowed values: multiple - [N]`

`multiple` other than `N` is passed for `LOCATION` with `isLocation4tax` = `Y` ||
|| `200850000015` | `Require fields: require.`

`required` is not passed for `STRING` with `isProfileName` = `Y` ||
|| `200850000016` | `Allowed values: require - [Y]`

`required` other than `Y` is passed for `STRING` with `isProfileName` = `Y` ||
|| `200850000017` | ```Wrong format of field `defaultValue` ```

An invalid `defaultValue` format is passed for `FILE`. Use the `fileData` object with two strings: the file name and the base64-encoded content ||
|| `200040300020` | `Access Denied`

Insufficient permissions to add an order property ||
|| `100` | `Could not find value for parameter {fields}`

Parameter `fields` is not specified ||
|| `0` | `Required fields: personTypeId, propsGroupId, name, type`

Required fields are not provided in `fields`. The message lists the missing fields ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-update.md)
- [{#T}](./sale-property-get.md)
- [{#T}](./sale-property-list.md)
- [{#T}](./sale-property-delete.md)
- [{#T}](./sale-property-get-fields-by-type.md)
