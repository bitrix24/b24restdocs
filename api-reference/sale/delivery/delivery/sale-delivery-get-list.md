# Get a list of delivery services sale.delivery.getlist

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

The method `sale.delivery.getlist` retrieves a list of delivery services.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SELECT**
[`array`](../../../data-types.md) | An array of fields to return. Available fields:
- `ID` — service identifier
- `PARENT_ID` — parent service identifier
- `NAME` — name
- `ACTIVE` — activity
- `DESCRIPTION` — description
- `SORT` — sorting
- `CURRENCY` — currency code
- `LOGOTYPE` — logo file identifier

Pass field names in uppercase; any other field causes the `ERROR_CHECK_FAILURE` error.

If the parameter is not passed, all fields are returned. If an empty array is passed, the list elements are returned empty: `[[]]` ||
|| **FILTER**
[`object`](../../../data-types.md) | An object for filtering in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`. The available fields are the same as in `SELECT`. To filter by logo, use the name `LOGOTIP`: the `LOGOTYPE` field in the filter causes the `100` error.

You can add a prefix to the field name:
- no prefix or `=` — equals. An array value works as "one of"
- `@` — one of the array values
- `!` or `!=` — not equal
- `>`, `>=`, `<`, `<=` — greater than, greater than or equal to, less than, less than or equal to
- `%` — contains a substring, case-insensitive. The `%` character is not needed in the value
- `!%` — does not contain a substring
- `=%` or `%=` — LIKE by pattern. Pass the `%` character in the value: `"Courier%"` — starts with "Courier"
- `!=%` or `!%=` — NOT LIKE by pattern

The `><` prefix is not supported ||
|| **ORDER**
[`object`](../../../data-types.md) | An object for sorting in the format `{"field_1": "direction_1", ..., "field_N": "direction_N"}`. The available fields are the same as in `SELECT`.

Possible direction values:
- `asc` — ascending
- `desc` — descending

The direction is case-insensitive. Any other value causes the `ERROR_ARGUMENT` error ||
|#

{% note info "" %}

The method returns all delivery services without pagination, including services not created via REST, and profiles. Pass the `SELECT`, `FILTER`, and `ORDER` parameter names in uppercase: the method does not recognize lowercase parameters and returns the entire list.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PARENT_ID","NAME","ACTIVE","DESCRIPTION","SORT","CURRENCY"],"FILTER":{"@ID":[196,197,198]},"ORDER":{"SORT":"ASC","ID":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.getlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"SELECT":["ID","PARENT_ID","NAME","ACTIVE","DESCRIPTION","SORT","CURRENCY"],"FILTER":{"@ID":[196,197,198]},"ORDER":{"SORT":"ASC","ID":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.getlist
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each DeliveryService returned in result[]
    type DeliveryService = {
      ID: number
      PARENT_ID: number | null
      NAME: string
      ACTIVE: string
      DESCRIPTION: string
      SORT: number
      CURRENCY: string
    }

    try {
      // sale.delivery.getlist returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<DeliveryService[]>({
        method: 'sale.delivery.getlist',
        params: {
          SELECT: ['ID', 'PARENT_ID', 'NAME', 'ACTIVE', 'DESCRIPTION', 'SORT', 'CURRENCY'],
          FILTER: {
            '@ID': [196, 197, 198],
          },
          ORDER: {
            SORT: 'ASC',
            ID: 'DESC',
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
        console.info('Delivery services (%d):', result.length, result)
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
      async function getDeliveryList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.delivery.getlist returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.delivery.getlist',
            params: {
              SELECT: ['ID', 'PARENT_ID', 'NAME', 'ACTIVE', 'DESCRIPTION', 'SORT', 'CURRENCY'],
              FILTER: {
                '@ID': [196, 197, 198],
              },
              ORDER: {
                SORT: 'ASC',
                ID: 'DESC',
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
          console.info('Delivery services (%d):', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDeliveryList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.sale.delivery.getlist(
            select=[
                "ID",
                "PARENT_ID",
                "NAME",
                "ACTIVE",
                "DESCRIPTION",
                "SORT",
                "CURRENCY",
            ],
            filter={
                "@ID": [
                    196,
                    197,
                    198,
                ],
            },
            order={
                "SORT": "ASC",
                "ID": "DESC",
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
                'sale.delivery.getlist',
                [
                    'SELECT' => [
                        "ID",
                        "PARENT_ID",
                        "NAME",
                        "ACTIVE",
                        "DESCRIPTION",
                        "SORT",
                        "CURRENCY",
                    ],
                    'FILTER' => [
                        '@ID' => [196, 197, 198],
                    ],
                    'ORDER' => [
                        'SORT' => "ASC",
                        'ID'   => "DESC",
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting delivery list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.getlist', {
            SELECT: [
                "ID",
                "PARENT_ID",
                "NAME",
                "ACTIVE",
                "DESCRIPTION",
                "SORT",
                "CURRENCY",
            ],
            FILTER: {
                "@ID": [196, 197, 198],
            },
            ORDER: {
                SORT: "ASC",
                ID: "DESC",
            },
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
        'sale.delivery.getlist',
        [
            'SELECT' => [
                "ID",
                "PARENT_ID",
                "NAME",
                "ACTIVE",
                "DESCRIPTION",
                "SORT",
                "CURRENCY",
            ],
            'FILTER' => [
                "@ID" => [196, 197, 198],
            ],
            'ORDER' => [
                "SORT" => "ASC",
                "ID" => "DESC",
            ]
        ]
    );
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.delivery.getlist", b24.Params{
    	"SELECT": []string{"ID", "PARENT_ID", "NAME", "ACTIVE", "DESCRIPTION", "SORT", "CURRENCY"},
    	"FILTER": b24.Params{
    		"@ID": []int{196, 197, 198},
    	},
    	"ORDER": b24.Params{
    		"SORT": "ASC",
    		"ID":   "DESC",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sale.delivery.getlist: %w", err)
    }

    var items []struct {
    	Name        string `json:"NAME"`
    	Active      string `json:"ACTIVE"`
    	Description string `json:"DESCRIPTION"`
    	Currency    string `json:"CURRENCY"`
    	ID          b24.ID `json:"ID"`
    	ParentID    b24.ID `json:"PARENT_ID"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.Name, it.Active)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result":[
        {
            "NAME":"Cargo",
            "ACTIVE":"Y",
            "DESCRIPTION":"Cargo Delivery",
            "CURRENCY":"USD",
            "ID":198,
            "PARENT_ID":196,
            "SORT":500
        },
        {
            "NAME":"Taxi",
            "ACTIVE":"Y",
            "DESCRIPTION":"Taxi Delivery",
            "CURRENCY":"USD",
            "ID":197,
            "PARENT_ID":196,
            "SORT":500
        },
        {
            "NAME":"Uber Taxi",
            "ACTIVE":"Y",
            "DESCRIPTION":"Uber Taxi Description",
            "CURRENCY":"USD",
            "ID":196,
            "PARENT_ID":null,
            "SORT":500
        }
    ],
    "time":{
        "start":1714141661.751638,
        "finish":1714141661.945714,
        "duration":0.1940760612487793,
        "processing":0.013057947158813477,
        "date_start":"2024-04-26T17:27:41+02:00",
        "date_finish":"2024-04-26T17:27:41+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | An array of delivery services. If nothing is found, an empty array is returned [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Element {#result}

The element contains the fields from `SELECT`. Without `SELECT`, all fields are returned.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Delivery service identifier ||
|| **PARENT_ID**
[`integer`](../../../data-types.md) | Identifier of the parent service. For a profile — the service identifier, for a top-level service — `null` or `0` ||
|| **NAME**
[`string`](../../../data-types.md) | Name ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Activity: `Y` — active, `N` — inactive ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description. Can be `null` ||
|| **CURRENCY**
[`string`](../../../data-types.md) | Symbolic code of the currency ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **LOGOTYPE**
[`integer`](../../../data-types.md) | Logo file identifier or `null` ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "ERROR_CHECK_FAILURE",
    "error_description": "FOO not allowed for select"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CHECK_FAILURE` | FOO not allowed for select | `SELECT` contains an unavailable field or a field not in uppercase ||
|| `400` | `ERROR_CHECK_FAILURE` | FOO not allowed for filter | `FILTER` contains an unavailable field or an unsupported prefix ||
|| `400` | `ERROR_CHECK_FAILURE` | FOO not allowed for order | `ORDER` contains an unavailable field ||
|| `400` | `ERROR_ARGUMENT` | Invalid order "BAD" | `ORDER` contains a direction other than `asc` or `desc` ||
|| `400` | `100` | Unknown field definition `LOGOTYPE` (LOGOTYPE) for \Bitrix\Sale\Delivery\Services\ Entity. | `FILTER` contains the `LOGOTYPE` field. To filter by logo, use `LOGOTIP` ||
|| `403` | `ACCESS_DENIED` | Access denied! | The user does not have CRM administrator permissions ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-add.md)
- [{#T}](./sale-delivery-update.md)
- [{#T}](./sale-delivery-delete.md)
- [{#T}](./sale-delivery-config-update.md)
- [{#T}](./sale-delivery-config-get.md)

