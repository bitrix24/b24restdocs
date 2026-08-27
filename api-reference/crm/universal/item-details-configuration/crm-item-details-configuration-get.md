# Get Parameters of CRM Item Detail Configuration crm.item.details.configuration.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: access rights during method execution depend on the provided data:
>   - Any user has the right to retrieve their own and shared settings
>   - A user can access another user's settings only if they are an administrator

The method `crm.item.details.configuration.get` returns the settings of the detail form for a specific CRM object. It can work with both personal settings of the specified user and shared settings defined for all users.

{% include [Extras Notice](./_includes/extras_notice.md) %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description**                                                                                                                    ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [system](../../index.md) or [custom type](../user-defined-object-types/index.md) of CRM entities ||
|| **userId**
[`user`](../../../data-types.md) | Identifier of the user whose configuration you want to retrieve.

If this parameter is not provided, the `userId` of the user calling this method will be used.

Required only when requesting personal settings
||
|| **scope**
[`string`](../../../data-types.md) | Scope of the settings. Acceptable values:
- `'P'` — personal settings
- `'C'` — shared settings

By default, the value is `'P'`

||
|| **extras**
[`object`](../../../data-types.md) | Additional parameters. Possible values and their structure are described [below](#parameter-extras) ||
|#

### Parameter extras

The parameter in `extras` depends on the CRM object.

#|
|| **CRM Object** | **Name** | **Description** ||
|| **SPA** | `categoryId` | Identifier of the SPA funnel. Can be obtained using [`crm.category.list`](./../category/crm-category-list.md).

If not specified, the default funnel identifier for this SPA is used ||
|| **Deal** | `dealCategoryId` | Identifier of the deal funnel. Can be obtained using [`crm.category.list`](./../category/crm-category-list.md).

If not specified, the default funnel identifier for deals is used ||
|| **Lead** | `leadCustomerType` | Type of leads. 

Possible values:
- `1` — simple leads
- `2` — repeat leads
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

1. Retrieve the general configuration of item details for deals in the pipeline with `id = 9`, for the user with `id = 1`

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"userId":1,"scope":"C","extras":{"dealCategoryId":9}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.details.configuration.get
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"userId":1,"scope":"C","extras":{"dealCategoryId":9},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.details.configuration.get
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type SectionElement = {
          name: string
          optionFlags: string
          options?: Record<string, string>
        }

        type Section = {
          name: string
          title: string
          type: string
          elements: SectionElement[]
        }

        try {
          const response = await $b24.actions.v2.call.make<Section[] | null>({
            method: 'crm.item.details.configuration.get',
            params: {
              entityTypeId: 2,
              userId: 1,
              scope: 'C',
              extras: {
                dealCategoryId: 9,
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('Sections count:', result?.length ?? 0, 'first section:', result?.[0]?.name)
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
          async function getDetailsConfiguration() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.item.details.configuration.get',
                params: {
                  entityTypeId: 2,
                  userId: 1,
                  scope: 'C',
                  extras: {
                    dealCategoryId: 9,
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
              console.info('Sections count:', result?.length ?? 0, 'first section:', result?.[0]?.name)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', getDetailsConfiguration)
        </script>
        ```

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.item.details.configuration.get(
                entity_type_id=2,
                user_id=1,
                scope="C",
                extras={
                    "dealCategoryId": 9,
                },
            ).response
            result = bitrix_response.result
            print(result)
        except BitrixAPIError as error:
            print(
                "Bitrix API Error",
                f"error: {error.error}",
                f"error_description: {error.error_description}",
                sep="\n",
            )
        except BitrixSDKException as error:
            print(f"Bitrix SDK Error: {error.message}")
        except Exception as error:
            print(f"Unexpected error: {error}")
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.details.configuration.get',
            [
                'entityTypeId' => 2,
                'userId' => 1,
                'scope' => "C",
                'extras' => [
                    'dealCategoryId' => 9,
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
        res, err := client.Core().Call(ctx, "crm.item.details.configuration.get", b24.Params{
        	"entityTypeId": 2,
        	"userId":       1,
        	"scope":        "C",
        	"extras": b24.Params{
        		"dealCategoryId": 9,
        	},
        }, b24.WithIdempotent())
        if err != nil {
        	return fmt.Errorf("crm.item.details.configuration.get: %w", err)
        }

        var items []struct {
        	Name  string `json:"name"`
        	Title string `json:"title"`
        	Type  string `json:"type"`
        }
        if err := json.Unmarshal(res.Result, &items); err != nil {
        	return fmt.Errorf("parse response: %w", err)
        }
        for _, it := range items {
        	fmt.Println(it.Name, it.Title)
        }
        ```

    {% endlist %}

2. Retrieve the personal configuration of item details for the SPA with `entityTypeId = 1032` in the pipeline with `id = 5`

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":1032,"extras":{"categoryId":5}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.details.configuration.get
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":1032,"extras":{"categoryId":5},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.details.configuration.get
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type SectionElement = {
          name: string
          optionFlags: string
          options?: Record<string, string>
        }

        type Section = {
          name: string
          title: string
          type: string
          elements: SectionElement[]
        }

        try {
          const response = await $b24.actions.v2.call.make<Section[] | null>({
            method: 'crm.item.details.configuration.get',
            params: {
              entityTypeId: 1032,
              extras: {
                categoryId: 5,
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('Sections count:', result?.length ?? 0, 'first section:', result?.[0]?.name)
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
          async function getDetailsConfiguration() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.item.details.configuration.get',
                params: {
                  entityTypeId: 1032,
                  extras: {
                    categoryId: 5,
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
              console.info('Sections count:', result?.length ?? 0, 'first section:', result?.[0]?.name)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', getDetailsConfiguration)
        </script>
        ```

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.item.details.configuration.get(
                entity_type_id=1032,
                extras={
                    "categoryId": 5,
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
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.details.configuration.get',
            [
                'entityTypeId' => 1032,
                'extras' => [
                    'categoryId' => 5,
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
        res, err := client.Core().Call(ctx, "crm.item.details.configuration.get", b24.Params{
        	"entityTypeId": 1032,
        	"extras": b24.Params{
        		"categoryId": 5,
        	},
        }, b24.WithIdempotent())
        if err != nil {
        	return fmt.Errorf("crm.item.details.configuration.get: %w", err)
        }

        var items []struct {
        	Name  string `json:"name"`
        	Title string `json:"title"`
        	Type  string `json:"type"`
        }
        if err := json.Unmarshal(res.Result, &items); err != nil {
        	return fmt.Errorf("parse response: %w", err)
        }
        for _, it := range items {
        	fmt.Println(it.Name, it.Title)
        }
        ```

    {% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "name": "main",
            "title": "About the deal",
            "type": "section",
            "elements": [
                {
                    "name": "TITLE",
                    "optionFlags": "0"
                },
                {
                    "name": "STAGE_ID",
                    "optionFlags": "0"
                },
                {
                    "name": "OPPORTUNITY_WITH_CURRENCY",
                    "optionFlags": "0"
                },
                {
                    "name": "CLOSEDATE",
                    "optionFlags": "0"
                },
                {
                    "name": "CLIENT",
                    "optionFlags": "1",
                    "options": {
                        "defaultCountry": "RU"
                    }
                },
                {
                    "name": "UF_CRM_1686898039656",
                    "optionFlags": "1"
                }
            ]
        },
        {
            "name": "additional",
            "title": "Additional",
            "type": "section",
            "elements": [
                {
                    "name": "TYPE_ID",
                    "optionFlags": "0"
                },
                {
                    "name": "SOURCE_ID",
                    "optionFlags": "0"
                },
                {
                    "name": "SOURCE_DESCRIPTION",
                    "optionFlags": "0"
                },
                {
                    "name": "BEGINDATE",
                    "optionFlags": "0"
                },
                {
                    "name": "OPENED",
                    "optionFlags": "0"
                },
                {
                    "name": "ASSIGNED_BY_ID",
                    "optionFlags": "0"
                },
                {
                    "name": "OBSERVER",
                    "optionFlags": "0"
                },
                {
                    "name": "COMMENTS",
                    "optionFlags": "0"
                },
                {
                    "name": "UTM",
                    "optionFlags": "0"
                }
            ]
        },
        {
            "name": "products",
            "title": "Products",
            "type": "section",
            "elements": [
                {
                    "name": "PRODUCT_ROW_SUMMARY",
                    "optionFlags": "0"
                }
            ]
        },
        {
            "name": "recurring",
            "title": "Recurring deal",
            "type": "section",
            "elements": [
                {
                    "name": "RECURRING",
                    "optionFlags": "0"
                }
            ]
        }
    ],
    "time": {
        "start": 1720624891.017344,
        "finish": 1720624891.405621,
        "duration": 0.3882770538330078,
        "processing": 0.02097320556640625,
        "date_start": "2024-07-10T17:21:31+02:00",
        "date_finish": "2024-07-10T17:21:31+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`section[]`](#section)\|`null` | Root element of the response. Contains the configuration of the sections of the detail form. Returns `null` if there is no configuration ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

#### Section

Describes a specific section with fields inside the element card

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
[`section_element[]`](#section_element) | List of fields displayed in the entity card with additional settings ||
|#

#### Section_Element

Configuration of a specific field within the section

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Field identifier ||
|| **optionFlags**
[`string`](../../../data-types.md) | Values:
- `"1"` — always show
- `"0"` — not always show ||
|| **options**
[`object`](../../../data-types.md) | Additional field options ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'entityTypeId' is not defined"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty value | Parameter 'entityTypeId' is not defined | Required parameter `entityTypeId` not provided ||
|| Empty value | The entity type '`entityTypeName`' is not supported in current context. | The method does not support this entity type || 
|| Empty value | Access denied. | The user does not have administrative rights ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-details-configuration-set.md)
- [{#T}](./crm-item-details-configuration-reset.md)
- [{#T}](./crm-item-details-configuration-forceCommonScopeForAll.md)
