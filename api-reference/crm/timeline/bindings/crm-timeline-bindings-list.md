# Retrieve a List of Timeline Record Bindings with CRM Entities crm.timeline.bindings.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Retrieves a list of bindings of a single timeline record with CRM entities.

The list also includes the binding with the entity in which the record was created: it appears automatically, without calling [crm.timeline.bindings.bind](./crm-timeline-bindings-bind.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`object`](../../../data-types.md) | Object for filtering selected records [(detailed description)](#filter) ||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static: 50 bindings.

To select the second page of results, pass the value `50`. To select the third page of results, pass the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` is the number of the desired page.

The default value is `0` — the first page. If you pass a value greater than the total number of bindings, the method returns the first page rather than an empty result ||
|| **order**
[`object`](../../../data-types.md) | The method accepts this parameter but ignores it: sorting of the result is not supported.

The value must be an object, otherwise the method returns the error `Parameter 'order' must be array.` ||
|#

### Parameter filter {#filter}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **OWNER_ID***
[`integer`](../../../data-types.md) | Identifier of the timeline record whose bindings you want to retrieve. Obtain it from the response of the method [crm.timeline.comment.add](../comments/crm-timeline-comment-add.md) or [crm.timeline.logmessage.add](../logmessage/crm-timeline-logmessage-add.md), or retrieve it from the list using the method [crm.timeline.comment.list](../comments/crm-timeline-comment-list.md) or [crm.timeline.logmessage.list](../logmessage/crm-timeline-logmessage-list.md).

The method takes only this filter field into account. Other fields do not affect the result ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"OWNER_ID":999}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.bindings.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"OWNER_ID":999},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.bindings.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each binding returned in result[]
    type TimelineBindingItem = {
      OWNER_ID: string,
      ENTITY_ID: string,
      ENTITY_TYPE: string,
    }

    try {
      // The list helpers (callList.make(), fetchList.make()) page through an id cursor field,
      // and this method returns no id at all — they would stop after the first page.
      // Walk the pages manually: repeat the call with `start` increased by 50 while `next` is present.
      const response = await $b24.actions.v2.call.make<TimelineBindingItem[]>({
        method: 'crm.timeline.bindings.list',
        params: {
          filter: {
            OWNER_ID: 999,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Bindings:', result.length, result)
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
      async function listTimelineBindings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // The list helpers (callList.make(), fetchList.make()) page through an id cursor field,
          // and this method returns no id at all — they would stop after the first page.
          // Walk the pages manually: repeat the call with `start` increased by 50 while `next` is present.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.timeline.bindings.list',
            params: {
              filter: {
                OWNER_ID: 999,
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
          console.info('Bindings:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTimelineBindings)
    </script>
    ```

- Python

    Example

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.timeline.bindings.list(
            filter={
                "OWNER_ID": 999,
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

    Example `as_list`

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.timeline.bindings.list(
            filter={
                "OWNER_ID": 999,
            },
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
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

    The fast traversal `as_list_fast` is not suitable for this method: it pages through the `ID` field, which is not present in the response.

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.timeline.bindings.list',
                [
                    'filter' => [
                        'OWNER_ID' => 999,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        // Retrieve the next page with the same call using the start parameter
        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching timeline bindings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.bindings.list",
        {
            filter: {
                "OWNER_ID": 999,
            },
        }, result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
                if (result.more()) {
                    result.next();
                }
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.bindings.list',
        [
            'filter' => [
                'OWNER_ID' => 999,
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
    res, err := client.Core().Call(ctx, "crm.timeline.bindings.list", b24.Params{
    	"filter": b24.Params{
    		"OWNER_ID": 999,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.timeline.bindings.list: %w", err)
    }

    var items []struct {
    	OwnerID    b24.ID `json:"OWNER_ID"`
    	EntityID   b24.ID `json:"ENTITY_ID"`
    	EntityType string `json:"ENTITY_TYPE"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.OwnerID, it.EntityID)
    }

    // For a full list traversal, use client.Core().Pages: it pages through start.
    // Scan does not work here — it goes by identifier, which is not present in the response.
    if res.Total != nil {
    	fmt.Println("total:", *res.Total)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "OWNER_ID": "999",
            "ENTITY_ID": "39",
            "ENTITY_TYPE": "deal"
        },
        {
            "OWNER_ID": "999",
            "ENTITY_ID": "92",
            "ENTITY_TYPE": "company"
        },
        {
            "OWNER_ID": "999",
            "ENTITY_ID": "205",
            "ENTITY_TYPE": "lead"
        }
    ],
    "total": 3,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+03:00",
        "date_finish": "2024-05-03T17:19:01+03:00",
        "operating": 0
    }
}
```

In the example below, the record has 60 bindings — one binding out of 50 on the first page is shown:

```json
{
    "result": [
        {
            "OWNER_ID": "999",
            "ENTITY_ID": "39",
            "ENTITY_TYPE": "deal"
        }
    ],
    "next": 50,
    "total": 60,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+03:00",
        "date_finish": "2024-05-03T17:19:01+03:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | Array of objects with the bindings found [(detailed description)](#result) ||
|| **next**
[`integer`](../../../data-types.md) | Value of the `start` parameter for the next page. Returned only if the timeline record has more than 50 bindings ||
|| **total**
[`integer`](../../../data-types.md) | The total number of bindings found ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **OWNER_ID**
[`string`](../../../data-types.md) | Identifier of the timeline record ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Identifier of the CRM entity ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | String code of the CRM object type `entityTypeName`. The method always returns it in lowercase. Possible values:
- `lead` — lead
- `deal` — deal
- `contact` — contact
- `company` — company
- `quote` — quote
- `smart_invoice` — invoice
- `order` — order
- `activity` — activity
- `dynamic_<entityTypeId>` — smart process item, for example `dynamic_128`

The response can also contain other CRM object types, for example `invoice` — an invoice in the old format. How the string type codes are structured is described in the section [CRM Object Type](../../data-types.md#object_type) ||
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
|| `400` | Empty value | Parameter 'filter' must be array. | The `filter` parameter is not passed as an object ||
|| `400` | Empty value | Parameter 'order' must be array. | The `order` parameter is not passed as an object ||
|#

If the timeline record with the given `OWNER_ID` does not exist or has no bindings, the method returns an empty array in `result` and `total` with the value `0`.

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-bindings-bind.md)
- [{#T}](./crm-timeline-bindings-unbind.md)
- [{#T}](./crm-timeline-bindings-fields.md)
- [{#T}](./index.md)
