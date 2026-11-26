# Update Delivery Service Settings sale.delivery.config.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

The method `sale.delivery.config.update` sets the values of the delivery service settings.

{% note info "" %}

The method works only with services and profiles created from REST handlers. For other services, the method returns the `ACCESS_DENIED` error.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_delivery_service.ID`](../../data-types.md#sale_delivery_service) | Identifier of the delivery service or profile.

You can retrieve the identifier using the [sale.delivery.getlist](./sale-delivery-get-list.md) method or from the [sale.delivery.add](./sale-delivery-add.md) response ||
|| **CONFIG**
[`object[]`](../../../data-types.md) | The complete set of service settings values. [Element description](#config).

The set of settings is declared in the handler's `SETTINGS.CONFIG`. You can retrieve it using the [sale.delivery.handler.list](../handler/sale-delivery-handler-list.md) method, and the current values using the [sale.delivery.config.get](./sale-delivery-config-get.md) method ||
|#

{% note warning "" %}

The method replaces the entire set of settings. Settings not passed in `CONFIG` are deleted. If `CONFIG` is not passed, all values are deleted. To change one setting, retrieve the current values using the [sale.delivery.config.get](./sale-delivery-config-get.md) method and pass them together with the new value.

For a profile, also pass the `PROFILE_TYPE` setting with the handler profile code; otherwise, it will be deleted.

{% endnote %}

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
    -d '{"ID":196,"CONFIG":[{"CODE":"SETTING_1","VALUE":"New SETTING_1 string value"},{"CODE":"SETTING_2","VALUE":"N"},{"CODE":"SETTING_3","VALUE":999.99},{"CODE":"SETTING_4","VALUE":"Option2Code"},{"CODE":"SETTING_5","VALUE":"03/25/2023"},{"CODE":"SETTING_6","VALUE":"0000144962"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.config.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":196,"CONFIG":[{"CODE":"SETTING_1","VALUE":"New SETTING_1 string value"},{"CODE":"SETTING_2","VALUE":"N"},{"CODE":"SETTING_3","VALUE":999.99},{"CODE":"SETTING_4","VALUE":"Option2Code"},{"CODE":"SETTING_5","VALUE":"03/25/2023"},{"CODE":"SETTING_6","VALUE":"0000144962"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.config.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'sale.delivery.config.update',
        params: {
          ID: 196,
          CONFIG: [
            { CODE: 'SETTING_1', VALUE: 'New SETTING_1 string value' },
            { CODE: 'SETTING_2', VALUE: 'N' },
            { CODE: 'SETTING_3', VALUE: 999.99 },
            { CODE: 'SETTING_4', VALUE: 'Option2Code' },
            { CODE: 'SETTING_5', VALUE: '25.03.2023' },
            { CODE: 'SETTING_6', VALUE: '0000144962' },
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Config updated:', result)
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
      async function updateDeliveryConfig() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.delivery.config.update',
            params: {
              ID: 196,
              CONFIG: [
                { CODE: 'SETTING_1', VALUE: 'New SETTING_1 string value' },
                { CODE: 'SETTING_2', VALUE: 'N' },
                { CODE: 'SETTING_3', VALUE: 999.99 },
                { CODE: 'SETTING_4', VALUE: 'Option2Code' },
                { CODE: 'SETTING_5', VALUE: '25.03.2023' },
                { CODE: 'SETTING_6', VALUE: '0000144962' },
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Config updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateDeliveryConfig)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    config = [
        {
            "CODE": "SETTING_1",
            "VALUE": "New SETTING_1 string value",
        },
        {
            "CODE": "SETTING_2",
            "VALUE": "N",
        },
        {
            "CODE": "SETTING_3",
            "VALUE": 999.99,
        },
        {
            "CODE": "SETTING_4",
            "VALUE": "Option2Code",
        },
        {
            "CODE": "SETTING_5",
            "VALUE": "25.03.2023",
        },
        {
            "CODE": "SETTING_6",
            "VALUE": "0000144962",
        },
    ]

    try:
        bitrix_response = client.sale.delivery.config.update(
            bitrix_id=196,
            config=config,
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
                'sale.delivery.config.update',
                [
                    'ID'     => 196,
                    'CONFIG' => [
                        [
                            'CODE'  => "SETTING_1",
                            'VALUE' => "New SETTING_1 string value",
                        ],
                        [
                            'CODE'  => "SETTING_2",
                            'VALUE' => "N",
                        ],
                        [
                            'CODE'  => "SETTING_3",
                            'VALUE' => 999.99,
                        ],
                        [
                            'CODE'  => "SETTING_4",
                            'VALUE' => "Option2Code",
                        ],
                        [
                            'CODE'  => "SETTING_5",
                            'VALUE' => "03/25/2023",
                        ],
                        [
                            'CODE'  => "SETTING_6",
                            'VALUE' => "0000144962",
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating delivery config: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.config.update', {
            ID: 196,
            CONFIG: [{
                    CODE: "SETTING_1",
                    VALUE: "New SETTING_1 string value",
                },
                {
                    CODE: "SETTING_2",
                    VALUE: "N",
                },
                {
                    CODE: "SETTING_3",
                    VALUE: 999.99,
                },
                {
                    CODE: "SETTING_4",
                    VALUE: "Option2Code",
                },
                {
                    CODE: "SETTING_5",
                    VALUE: "03/25/2023",
                },
                {
                    CODE: "SETTING_6",
                    VALUE: "0000144962",
                },
            ],

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
        'sale.delivery.config.update',
        [
            'ID' => 196,
            'CONFIG' => [
                ['CODE' => "SETTING_1", 'VALUE' => "New SETTING_1 string value"],
                ['CODE' => "SETTING_2", 'VALUE' => "N"],
                ['CODE' => "SETTING_3", 'VALUE' => 999.99],
                ['CODE' => "SETTING_4", 'VALUE' => "Option2Code"],
                ['CODE' => "SETTING_5", 'VALUE' => "03/25/2023"],
                ['CODE' => "SETTING_6", 'VALUE' => "0000144962"],
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
    res, err := client.Core().Call(ctx, "sale.delivery.config.update", b24.Params{
    	"ID": 196,
    	"CONFIG": []b24.Params{
    		{
    			"CODE":  "SETTING_1",
    			"VALUE": "New SETTING_1 string value",
    		},
    		{
    			"CODE":  "SETTING_2",
    			"VALUE": "N",
    		},
    		{
    			"CODE":  "SETTING_3",
    			"VALUE": 999.99,
    		},
    		{
    			"CODE":  "SETTING_4",
    			"VALUE": "Option2Code",
    		},
    		{
    			"CODE":  "SETTING_5",
    			"VALUE": "25.03.2023",
    		},
    		{
    			"CODE":  "SETTING_6",
    			"VALUE": "0000144962",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.delivery.config.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
   "result":true,
   "time":{
      "start":1714134319.861777,
      "finish":1714134320.202336,
      "duration":0.3405590057373047,
      "processing":0.07335114479064941,
      "date_start":"2024-04-26T15:25:19+02:00",
      "date_finish":"2024-04-26T15:25:20+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of updating the delivery service settings ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "ERROR_CHECK_FAILURE",
    "error_description": "Config item VALUE is not specified"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CHECK_FAILURE` | Parameter ID is not defined | The `ID` parameter is not passed ||
|| `400` | `ERROR_CHECK_FAILURE` | Parameter CONFIG must be of array type | The `CONFIG` parameter is not passed as an array ||
|| `400` | `ERROR_CHECK_FAILURE` | Config item CODE is not specified | A `CONFIG` element has no `CODE` or it is an empty string ||
|| `400` | `ERROR_CHECK_FAILURE` | Config item VALUE is not specified | A `CONFIG` element has no `VALUE` or it is an empty string ||
|| `400` | `ERROR_DELIVERY_NOT_FOUND` | Delivery not found | The delivery service with the specified `ID` is not found ||
|| `400` | `ERROR_DELIVERY_CONFIG_UPDATE` | Save error text | The settings could not be saved; the error text is in `error_description` ||
|| `403` | `ACCESS_DENIED` | Access denied! | The user does not have CRM administrator permissions, the service is not created from a REST handler, or the handler was created by another application ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-add.md)
- [{#T}](./sale-delivery-update.md)
- [{#T}](./sale-delivery-get-list.md)
- [{#T}](./sale-delivery-delete.md)
- [{#T}](./sale-delivery-config-get.md)
