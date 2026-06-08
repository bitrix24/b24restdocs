# Get Parameters of the Deal Card crm.deal.details.configuration.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
> - a user can retrieve their own and shared settings
> - personal settings of another user can be accessed if the user has edit rights for that user's personal view

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md).

{% endnote %}

The method `crm.deal.details.configuration.get` retrieves the settings of the deal card. It reads the personal settings of the specified user or the shared settings defined for all users.

{% note info %}

The settings of deal cards may vary across different Sales Funnels. To select a funnel, use the `extras.dealCategoryId` parameter.

{% endnote %}

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings.

Possible values:
- `P` — personal settings
- `C` — shared settings

Default is `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier. Required only when requesting personal settings of another user.

If not specified, the current user's ID is used.
||
|| **extras**
[`object`](../../../data-types.md) | Additional parameters [(detailed description)](#parameter-extras) ||
|#

### Extras Parameter {#parameter-extras}

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **dealCategoryId**
[`integer`](../../../data-types.md) | Identifier of the deal funnel. Can be obtained using [crm.category.list](../../universal/category/crm-category-list.md)

If not specified, the default funnel for deals is used.
||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Example of retrieving the general configuration of the deal card for the funnel with `id = 32`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"scope":"C","extras":{"dealCategoryId":32}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.details.configuration.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"scope":"C","extras":{"dealCategoryId":32},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.details.configuration.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each section returned in result[]
    type CrmDealDetailsSection = {
      name: string
      title: string
      type: string
      elements: CrmDealDetailsSectionElement[]
    }

    type CrmDealDetailsSectionElement = {
      name: string
      optionFlags?: string
      options?: Record<string, unknown>
    }

    try {
      const response = await $b24.actions.v2.call.make<CrmDealDetailsSection[] | null>({
        method: 'crm.deal.details.configuration.get',
        params: {
          scope: 'C',
          extras: {
            dealCategoryId: 32,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Card sections:', result?.length ?? 0, result)
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
      async function getDealCardConfiguration() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.deal.details.configuration.get',
            params: {
              scope: 'C',
              extras: {
                dealCategoryId: 32,
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
          console.info('Card sections:', result?.length ?? 0, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDealCardConfiguration)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.deal.details.configuration.get',
                [
                    'scope' => 'C',
                    'extras' => [
                        'dealCategoryId' => 32,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting deal details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.details.configuration.get',
        {
            scope: "C",
            extras: {
                dealCategoryId: 32,
            },
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.details.configuration.get',
        [
            'scope' => 'C',
            'extras' => [
                'dealCategoryId' => 32,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.deal.details.configuration.get(
            scope="P",
            user_id=1,
            extras={"dealCategoryId": 0},
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
{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "name": "main",
            "title": "About the Deal",
            "type": "section",
            "elements": [
                {
                    "name": "TITLE"
                },
                {
                    "name": "OPPORTUNITY_WITH_CURRENCY"
                },
                {
                    "name": "STAGE_ID"
                },
                {
                    "name": "CLOSEDATE"
                },
                {
                    "name": "CLIENT"
                }
            ]
        },
        {
            "name": "additional",
            "title": "Additional",
            "type": "section",
            "elements": [
                {
                    "name": "TYPE_ID"
                },
                {
                    "name": "SOURCE_ID",
                    "optionFlags": "1"
                },
                {
                    "name": "SOURCE_DESCRIPTION"
                },
                {
                    "name": "BEGINDATE"
                },
                {
                    "name": "OPENED"
                },
                {
                    "name": "ASSIGNED_BY_ID"
                },
                {
                    "name": "OBSERVER"
                },
                {
                    "name": "COMMENTS"
                },
                {
                    "name": "UTM"
                }
            ]
        },
        {
            "name": "products",
            "title": "Products",
            "type": "section",
            "elements": [
                {
                    "name": "PRODUCT_ROW_SUMMARY"
                }
            ]
        },
        {
            "name": "recurring",
            "title": "Recurring Deal",
            "type": "section",
            "elements": [
                {
                    "name": "RECURRING"
                }
            ]
        }
    ],
    "time": {
        "start": 1773240673,
        "finish": 1773240673.91616,
        "duration": 0.9161601066589355,
        "processing": 0,
        "date_start": "2026-03-11T17:51:13+01:00",
        "date_finish": "2026-03-11T17:51:13+01:00",
        "operating_reset_at": 1773241273,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`section[]`](#section)\|`null` | The root element of the response. Contains the configuration of the sections of the deal detail card. Returns `null` if there is no configuration ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

### Section Parameter {#section}

Describes an individual section with fields within the deal card

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Unique name of the section used for identification ||
|| **title**
[`string`](../../../data-types.md) | Title of the section ||
|| **type**
[`string`](../../../data-types.md) | Type of the section ||
|| **elements**
[`section_element[]`](#section_element) | List of fields displayed in the card with additional settings ||
|#

### Section Element Parameter {#section_element}

Configuration of an individual field within the section

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Field identifier ||
|| **optionFlags**
[`string`](../../../data-types.md) | Values:
- `"1"` — always show
- `"0"` — not always show
||
|| **options**
[`object`](../../../data-types.md) | Additional field options ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty Value | Access denied | No rights to retrieve deal card settings ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-deal-details-configuration-set.md)
- [{#T}](./crm-deal-details-configuration-reset.md)
- [{#T}](./crm-deal-details-configuration-force-common-scope-for-all.md)