# Add Delivery Service sale.delivery.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

The method `sale.delivery.add` adds a delivery service.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **REST_CODE***
[`string`](../../../data-types.md) | Symbolic code of the REST handler of the delivery service — the `CODE` field of the [sale_delivery_handler](../../data-types.md#sale_delivery_handler) object.

You can retrieve handler codes using the [sale.delivery.handler.list](../handler/sale-delivery-handler-list.md) method and create a handler using the [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md) method ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the delivery service ||
|| **CURRENCY***
[`crm_currency.CURRENCY`](../../../crm/data-types.md#crm_currency) | Symbolic code of the delivery service currency, for example, `USD`.

You can retrieve currency codes using the [crm.currency.list](../../../crm/currency/crm-currency-list.md) method. The method does not check whether the currency exists and saves any code passed ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description of the delivery service.

The default value is an empty string ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting. A non-numeric value is saved as `0`.

The default value is `100` ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Activity of the delivery service. Possible values:
- `Y` — active
- `N` — inactive

Any other value causes the `ERROR_DELIVERY_ADD` error.

The default value is `Y` ||
|| **CONFIG**
[`object[]`](../../../data-types.md) | Array of delivery service settings values. [Element description](#config).

The set of settings is declared in the handler's `SETTINGS.CONFIG`. You can retrieve it using the [sale.delivery.handler.list](../handler/sale-delivery-handler-list.md) method ||
|| **LOGOTYPE**
[`string`](../../../data-types.md) \| [`array`](../../../data-types.md) | Logo of the delivery service. Pass the file content in Base64 as a string or as an array `["file_name.png", "Base64 content"]` ||
|#

Service profiles are created automatically: for each handler profile, the method creates a child delivery service with the same currency. The profile name and description are taken from the handler.

### CONFIG Element {#config}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../../data-types.md) | Symbolic code of the setting — the `CODE` field of an element of the handler's `SETTINGS.CONFIG`, for example, `SETTING_1`.

The method does not match the code against the handler settings and saves any code passed ||
|| **VALUE***
[`any`](../../../data-types.md) | Value of the setting. An empty string is not allowed.

The method does not validate the value against the setting type; for example, any value is saved for `ENUM` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"REST_CODE":"uber","NAME":"Uber Taxi","CURRENCY":"USD","DESCRIPTION":"Uber Taxi Description","SORT":"500","ACTIVE":"Y","CONFIG":[{"CODE":"SETTING_1","VALUE":"SETTING_1 string value"},{"CODE":"SETTING_2","VALUE":"Y"},{"CODE":"SETTING_3","VALUE":199.99},{"CODE":"SETTING_4","VALUE":"Option3Code"},{"CODE":"SETTING_5","VALUE":"10.04.2024"},{"CODE":"SETTING_6","VALUE":"0000144961"}],"LOGOTYPE":"iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"REST_CODE":"uber","NAME":"Uber Taxi","CURRENCY":"USD","DESCRIPTION":"Uber Taxi Description","SORT":"500","ACTIVE":"Y","CONFIG":[{"CODE":"SETTING_1","VALUE":"SETTING_1 string value"},{"CODE":"SETTING_2","VALUE":"Y"},{"CODE":"SETTING_3","VALUE":199.99},{"CODE":"SETTING_4","VALUE":"Option3Code"},{"CODE":"SETTING_5","VALUE":"10.04.2024"},{"CODE":"SETTING_6","VALUE":"0000144961"}],"LOGOTYPE":"iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SaleDeliveryAddResult = {
      parent: {
        NAME: string,
        ACTIVE: string,
        DESCRIPTION: string,
        CURRENCY: string,
        ID: number,
        PARENT_ID: number | null,
        SORT: number,
        LOGOTYPE: number | null,
      },
      profiles: Array<{
        NAME: string,
        ACTIVE: string,
        DESCRIPTION: string,
        CURRENCY: string,
        ID: number,
        PARENT_ID: number | null,
        SORT: number,
        LOGOTYPE: number | null,
      }>,
    }

    try {
      const response = await $b24.actions.v2.call.make<SaleDeliveryAddResult>({
        method: 'sale.delivery.add',
        params: {
          REST_CODE: 'uber',
          NAME: 'Uber Taxi',
          CURRENCY: 'USD',
          DESCRIPTION: 'Uber Taxi Description',
          SORT: '500',
          ACTIVE: 'Y',
          CONFIG: [
            { CODE: 'SETTING_1', VALUE: 'SETTING_1 string value' },
            { CODE: 'SETTING_2', VALUE: 'Y' },
            { CODE: 'SETTING_3', VALUE: 199.99 },
            { CODE: 'SETTING_4', VALUE: 'Option3Code' },
            { CODE: 'SETTING_5', VALUE: '10.04.2024' },
            { CODE: 'SETTING_6', VALUE: '0000144961' },
          ],
          LOGOTYPE: 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added delivery service ID:', result.parent.ID, 'Profiles:', result.profiles.length)
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
      async function addDelivery() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.delivery.add',
            params: {
              REST_CODE: 'uber',
              NAME: 'Uber Taxi',
              CURRENCY: 'USD',
              DESCRIPTION: 'Uber Taxi Description',
              SORT: '500',
              ACTIVE: 'Y',
              CONFIG: [
                { CODE: 'SETTING_1', VALUE: 'SETTING_1 string value' },
                { CODE: 'SETTING_2', VALUE: 'Y' },
                { CODE: 'SETTING_3', VALUE: 199.99 },
                { CODE: 'SETTING_4', VALUE: 'Option3Code' },
                { CODE: 'SETTING_5', VALUE: '10.04.2024' },
                { CODE: 'SETTING_6', VALUE: '0000144961' },
              ],
              LOGOTYPE: 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Added delivery service ID:', result.parent.ID, 'Profiles:', result.profiles.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addDelivery)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    config = [
        {
            "CODE": "SETTING_1",
            "VALUE": "SETTING_1 string value",
        },
        {
            "CODE": "SETTING_2",
            "VALUE": "Y",
        },
        {
            "CODE": "SETTING_3",
            "VALUE": 199.99,
        },
        {
            "CODE": "SETTING_4",
            "VALUE": "Option3Code",
        },
        {
            "CODE": "SETTING_5",
            "VALUE": "10.04.2024",
        },
        {
            "CODE": "SETTING_6",
            "VALUE": "0000144961",
        },
    ]

    try:
        bitrix_response = client.sale.delivery.add(
            rest_code='uber',
            name='Uber Taxi',
            currency='USD',
            description='Uber Taxi Description',
            sort=500,
            active=True,
            config=config,
            logotype='iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==',
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
                'sale.delivery.add',
                [
                    'REST_CODE'    => "uber",
                    'NAME'         => "Uber Taxi",
                    'CURRENCY'     => "USD",
                    'DESCRIPTION'  => "Uber Taxi Description",
                    'SORT'         => "500",
                    'ACTIVE'       => "Y",
                    'CONFIG'       => [
                        [
                            'CODE'  => "SETTING_1",
                            'VALUE' => "SETTING_1 string value",
                        ],
                        [
                            'CODE'  => "SETTING_2",
                            'VALUE' => "Y",
                        ],
                        [
                            'CODE'  => "SETTING_3",
                            'VALUE' => 199.99,
                        ],
                        [
                            'CODE'  => "SETTING_4",
                            'VALUE' => "Option3Code",
                        ],
                        [
                            'CODE'  => "SETTING_5",
                            'VALUE' => "10.04.2024",
                        ],
                        [
                            'CODE'  => "SETTING_6",
                            'VALUE' => "0000144961",
                        ],
                    ],
                    'LOGOTYPE'     => "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==",
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding delivery method: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.add', {
            REST_CODE: "uber",
            NAME: "Uber Taxi",
            CURRENCY: "USD",
            DESCRIPTION: "Uber Taxi Description",
            SORT: "500",
            ACTIVE: "Y",
            CONFIG: [{
                    CODE: "SETTING_1",
                    VALUE: "SETTING_1 string value",
                },
                {
                    CODE: "SETTING_2",
                    VALUE: "Y",
                },
                {
                    CODE: "SETTING_3",
                    VALUE: 199.99,
                },
                {
                    CODE: "SETTING_4",
                    VALUE: "Option3Code",
                },
                {
                    CODE: "SETTING_5",
                    VALUE: "10.04.2024",
                },
                {
                    CODE: "SETTING_6",
                    VALUE: "0000144961",
                },
            ],
    LOGOTYPE: "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==",
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
        'sale.delivery.add',
        [
            'REST_CODE' => "uber",
            'NAME' => "Uber Taxi",
            'CURRENCY' => "USD",
            'DESCRIPTION' => "Uber Taxi Description",
            'SORT' => "500",
            'ACTIVE' => "Y",
            'CONFIG' => [
                ['CODE' => "SETTING_1", 'VALUE' => "SETTING_1 string value"],
                ['CODE' => "SETTING_2", 'VALUE' => "Y"],
                ['CODE' => "SETTING_3", 'VALUE' => 199.99],
                ['CODE' => "SETTING_4", 'VALUE' => "Option3Code"],
                ['CODE' => "SETTING_5", 'VALUE' => "10.04.2024"],
                ['CODE' => "SETTING_6", 'VALUE' => "0000144961"],
            ],
            'LOGOTYPE' => "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.delivery.add", b24.Params{
    	"REST_CODE":   "uber",
    	"NAME":        "Uber Taxi",
    	"CURRENCY":    "USD",
    	"DESCRIPTION": "Uber Taxi Description",
    	"SORT":        "500",
    	"ACTIVE":      "Y",
    	"CONFIG": []b24.Params{
    		{
    			"CODE":  "SETTING_1",
    			"VALUE": "SETTING_1 string value",
    		},
    		{
    			"CODE":  "SETTING_2",
    			"VALUE": "Y",
    		},
    		{
    			"CODE":  "SETTING_3",
    			"VALUE": 199.99,
    		},
    		{
    			"CODE":  "SETTING_4",
    			"VALUE": "Option3Code",
    		},
    		{
    			"CODE":  "SETTING_5",
    			"VALUE": "10.04.2024",
    		},
    		{
    			"CODE":  "SETTING_6",
    			"VALUE": "0000144961",
    		},
    	},
    	"LOGOTYPE": "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==",
    })
    if err != nil {
    	return fmt.Errorf("sale.delivery.add: %w", err)
    }

    // The response shape is shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
   "result":{
      "parent":{
         "NAME":"Uber Taxi",
         "ACTIVE":"Y",
         "DESCRIPTION":"Uber Taxi Description",
         "CURRENCY":"USD",
         "ID":196,
         "PARENT_ID":null,
         "SORT":500,
         "LOGOTYPE":973
      },
      "profiles":[
         {
            "NAME":"Taxi",
            "ACTIVE":"Y",
            "DESCRIPTION":"Taxi Delivery",
            "CURRENCY":"USD",
            "ID":197,
            "PARENT_ID":196,
            "SORT":500,
            "LOGOTYPE":null
         },
         {
            "NAME":"Cargo",
            "ACTIVE":"Y",
            "DESCRIPTION":"Cargo Delivery",
            "CURRENCY":"USD",
            "ID":198,
            "PARENT_ID":196,
            "SORT":500,
            "LOGOTYPE":null
         }
      ]
   },
   "time":{
      "start":1713946187.579457,
      "finish":1713946187.925688,
      "duration":0.34623098373413086,
      "processing":0.06604909896850586,
      "date_start":"2024-04-24T11:09:47+02:00",
      "date_finish":"2024-04-24T11:09:47+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The created delivery service and its profiles [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **parent**
[`object`](../../../data-types.md) | The created delivery service [(detailed description)](#delivery) ||
|| **profiles**
[`object[]`](../../../data-types.md) | Delivery service profiles created from the handler profiles. The element structure is the same as for `parent` [(detailed description)](#delivery).

If the handler has no profiles, an empty array is returned ||
|#

#### Delivery Service Object {#delivery}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Delivery service identifier ||
|| **PARENT_ID**
[`integer`](../../../data-types.md) | Identifier of the parent service. For a top-level service — `null`, for a profile — the identifier of the service from `parent` ||
|| **NAME**
[`string`](../../../data-types.md) | Name ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Activity: `Y` — active, `N` — inactive ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description ||
|| **CURRENCY**
[`string`](../../../data-types.md) | Symbolic code of the currency ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **LOGOTYPE**
[`integer`](../../../data-types.md) | Logo file identifier or `null` if no logo is set ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "ERROR_HANDLER_NOT_FOUND",
    "error_description": "Handler \"nosuch\" not exists"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CHECK_FAILURE` | Parameter REST_CODE is not defined | The `REST_CODE` parameter is not passed, or the parameter name is not in uppercase ||
|| `400` | `ERROR_CHECK_FAILURE` | Parameter NAME is not defined | The `NAME` parameter is not passed ||
|| `400` | `ERROR_CHECK_FAILURE` | Parameter CURRENCY is not defined | The `CURRENCY` parameter is not passed ||
|| `400` | `ERROR_CHECK_FAILURE` | Parameter CONFIG must be of array type | The `CONFIG` parameter is not passed as an array ||
|| `400` | `ERROR_CHECK_FAILURE` | Config item CODE is not specified | A `CONFIG` element has no `CODE` or it is an empty string ||
|| `400` | `ERROR_CHECK_FAILURE` | Config item VALUE is not specified | A `CONFIG` element has no `VALUE` or it is an empty string ||
|| `400` | `ERROR_HANDLER_NOT_FOUND` | Handler "nosuch" not exists | The handler with the code from `REST_CODE` is not found ||
|| `400` | `ERROR_DELIVERY_ADD` | The value of "Active" is invalid | `ACTIVE` contains a value other than `Y` or `N`. The code is also returned for other errors when saving the service; the error text is in `error_description` ||
|| `403` | `ACCESS_DENIED` | Access denied! | The user does not have CRM administrator permissions, or the handler was created by another application ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-update.md)
- [{#T}](./sale-delivery-get-list.md)
- [{#T}](./sale-delivery-delete.md)
- [{#T}](./sale-delivery-config-update.md)
- [{#T}](./sale-delivery-config-get.md)
- [{#T}](../../../../tutorials/sale/delivery-in-crm.md)
