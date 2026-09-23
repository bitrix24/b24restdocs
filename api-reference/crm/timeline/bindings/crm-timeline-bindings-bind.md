# Add a Timeline Record Binding with a CRM Entity crm.timeline.bindings.bind

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify the CRM entity the record is bound to

Adds a binding between a timeline record and a CRM entity. After that, the record is displayed in the timeline of the specified entity.

{% note info "" %}

The binding with the entity in which the timeline record was created appears automatically — there is no need to call the method for it separately.

A repeated call with the same `OWNER_ID`, `ENTITY_TYPE`, and `ENTITY_ID` values does not create a duplicate and returns `true`. The method does not check whether the CRM entity and the timeline record exist — verify the identifiers before calling it.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values (detailed description provided [below](#parameter-fields)) for adding a binding of a timeline record to a CRM entity in the form of a structure:

```js
fields: {
    "OWNER_ID": 1110,
    "ENTITY_ID": 10,
    "ENTITY_TYPE": "deal",
},
```
 ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **OWNER_ID***
[`integer`](../../../data-types.md) | Identifier of the timeline record. Obtain it from the response of the method [crm.timeline.comment.add](../comments/crm-timeline-comment-add.md) or [crm.timeline.logmessage.add](../logmessage/crm-timeline-logmessage-add.md), or retrieve it from the list using the method [crm.timeline.comment.list](../comments/crm-timeline-comment-list.md) or [crm.timeline.logmessage.list](../logmessage/crm-timeline-logmessage-list.md) ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Identifier of the CRM entity to which the timeline record is bound ||
|| **ENTITY_TYPE***
[`string`](../../../data-types.md) | String code of the CRM object type `entityTypeName` to which the timeline record is bound. The value is case-insensitive. Possible values:
- `lead` — lead
- `deal` — deal
- `contact` — contact
- `company` — company
- `quote` — quote
- `smart_invoice` — invoice
- `order` — order
- `activity` — activity
- `invoice` — invoice in the old format
- `order_payment` — order payment
- `order_shipment` — order shipment
- `dynamic_<entityTypeId>` — smart process item, for example `dynamic_128`

How the string type codes are structured is described in the section [CRM Object Type](../../data-types.md#object_type). Not all codes from that table are suitable: the method does not accept the requisites code `requisite`.

If the type code is not supported or there is no smart process with such `entityTypeId` in Bitrix24, the method returns the error `ENTITY_TYPE is not defined or invalid.` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"OWNER_ID":1110,"ENTITY_ID":10,"ENTITY_TYPE":"deal"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.bindings.bind
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"OWNER_ID":1110,"ENTITY_ID":10,"ENTITY_TYPE":"deal"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.bindings.bind
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
        method: 'crm.timeline.bindings.bind',
        params: {
          fields: {
            OWNER_ID: 1110,
            ENTITY_ID: 10,
            ENTITY_TYPE: 'deal',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Binding created:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@2/dist/umd/index.min.js"></script>
    <script>
      async function createBinding() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.timeline.bindings.bind',
            params: {
              fields: {
                OWNER_ID: 1110,
                ENTITY_ID: 10,
                ENTITY_TYPE: 'deal',
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
          console.info('Binding created:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', createBinding)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.timeline.bindings.bind(
            fields={
                "OWNER_ID": 1110,
                "ENTITY_ID": 10,
                "ENTITY_TYPE": "deal",
            },
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
                'crm.timeline.bindings.bind',
                [
                    'fields' => [
                        'OWNER_ID'    => 1110,
                        'ENTITY_ID'   => 10,
                        'ENTITY_TYPE' => 'deal',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error binding timeline: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.bindings.bind",
        {
            fields: {
                "OWNER_ID": 1110,
                "ENTITY_ID": 10,
                "ENTITY_TYPE": "deal",
            },
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.bindings.bind',
        [
            'fields' => [
                'OWNER_ID' => 1110,
                'ENTITY_ID' => 10,
                'ENTITY_TYPE' => 'deal',
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
    res, err := client.Core().Call(ctx, "crm.timeline.bindings.bind", b24.Params{
    	"fields": b24.Params{
    		"OWNER_ID":    1110,
    		"ENTITY_ID":   10,
    		"ENTITY_TYPE": "deal",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.timeline.bindings.bind: %w", err)
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
    "result": true,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if the binding is created or already existed.

Returns `false` if the user does not have permission to modify the CRM entity from `ENTITY_ID` or the binding could not be saved. The HTTP status of the response remains `200`, and the `error` object is not returned ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "OWNER_ID is not defined or invalid."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | OWNER_ID is not defined or invalid. | The required parameter `OWNER_ID` is not provided, a non-numeric value is passed, or the number is less than one ||
|| `400` | Empty value | ENTITY_ID is not defined or invalid. | The required parameter `ENTITY_ID` is not provided, a non-numeric value is passed, or the number is less than one ||
|| `400` | Empty value | ENTITY_TYPE is not defined or invalid. | The required parameter `ENTITY_TYPE` is not provided, an unsupported type code is passed, or there is no smart process with such `entityTypeId` ||
|| `400` | `ERROR_ARGUMENT` | Wrong params. | The `fields` parameter is not passed as an object ||
|#

If permission is denied, the method returns `false` rather than an error.

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-bindings-list.md)
- [{#T}](./crm-timeline-bindings-unbind.md)
- [{#T}](./crm-timeline-bindings-fields.md)
- [{#T}](./index.md)
