# Get a List of CRM Pipelines v 3.0 crm.[type].category.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read items of this CRM object type

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.category.list` method returns the pipelines of a CRM object type. The object type is set by the `<type>` segment of the method name. The list includes only the pipelines whose items the user can read. The method skips other pipelines without an error.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.category.list` ||
|| Contact | `crm.contact.category.list` ||
|| Company | `crm.company.category.list` ||
|| SPA | `crm.smartprocess<entityTypeId>.category.list`, for example, `crm.smartprocess1038.category.list` ||
|#

For an SPA, the method name includes its type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

Deals, contacts, and companies have pipelines, and so does an SPA if pipelines are enabled in its type settings. Leads, estimates, invoices, and my company have none.

The method name is written in lowercase: `crm.smartprocess1038.category.list`, not `crm.smartProcess1038.category.list`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters. [Parameter description](#pagination)

By default, the method returns the first 50 pipelines ||
|#

The method returns pipelines in ascending `sort` order, and pipelines with the same `sort` in ascending `id` order.

{% note warning "" %}

The method has no `filter`, `order`, or `select` parameters. If they are passed, the method skips them without an error: it returns unfiltered pipelines with all fields, in `sort` order.

{% endnote %}

### The pagination Parameter {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Number of pipelines per page, from `1` to `100`. A value greater than `100` causes an error.

Default: `50` ||
|| **page**
[`integer`](../../../data-types.md) | Page number, starting from `1`. The offset is calculated as `(page - 1) × limit`. Zero returns the first page.

If `page` is passed, the `offset` parameter is ignored ||
|| **offset**
[`integer`](../../../data-types.md) | Number of pipelines to skip from the beginning of the list. A negative value causes an error.

Default: `0` ||
|#

Other keys in `pagination` cause an error.

{% note info "" %}

The response does not contain the total number of pipelines. If the page returns fewer pipelines than specified in `limit`, it is the last page.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.category.list`

{% endnote %}

The examples retrieve the first two pipelines of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"pagination":{"limit":2}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.category.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"pagination":{"limit":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.category.list
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CategoryListResult = {
      items: Array<{
        id: number
        name: string
        sort: number
        isDefault: boolean
        isSystem?: boolean
        code?: string
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CategoryListResult>(
        'crm.smartprocess1038.category.list',
        {
          pagination: {
            limit: 2,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const categories = response.getData()!.result.items
        categories.forEach(category => console.info(category.id, category.name))
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listCategories() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.category.list',
            {
              pagination: {
                limit: 2,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const categories = response.getData().result.items
          categories.forEach(category => console.info(category.id, category.name))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listCategories)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.category.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "pagination": {
                    "limit": 2,
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.category.list',
                [
                    'pagination' => [
                        'limit' => 2,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.category.list',
        {
            pagination: {
                limit: 2,
            },
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.category.list',
        [
            'pagination' => [
                'limit' => 2,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.category.list", b24.Params{
    	"pagination": b24.Params{
    		"limit": 2,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.category.list: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("response has no items")
    }

    var categories []struct {
    	ID        int    `json:"id"`
    	Name      string `json:"name"`
    	Sort      int    `json:"sort"`
    	IsDefault bool   `json:"isDefault"`
    }
    if err := json.Unmarshal(raw, &categories); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, category := range categories {
    	fmt.Println(category.ID, category.Name)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 37,
                "name": "General",
                "sort": 500,
                "isDefault": false,
                "isSystem": false,
                "code": ""
            },
            {
                "id": 105,
                "name": "Equipment and furniture procurement",
                "sort": 650,
                "isDefault": false,
                "isSystem": false,
                "code": ""
            }
        ]
    },
    "time": {
        "start": 1793714739,
        "finish": 1793714739.672466,
        "duration": 0.6724660396575928,
        "processing": 0,
        "date_start": "2026-11-03T17:05:39+03:00",
        "date_finish": "2026-11-03T17:05:39+03:00",
        "operating_reset_at": 1793715300,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.items**
[`object[]`](../../../data-types.md) | Pipelines. The pipeline fields are described on the page of the [crm.&lt;type&gt;.category.get](./crm-entity-category-get-rest-v3.md#item) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

The list of deal, contact, and company pipelines includes a pipeline with `id` `0`. Deal pipelines have no `isSystem` or `code` fields. The list for a contact and a company can contain system pipelines with `isSystem: true`, for example, the contact vendor pipeline with `code` `CATALOG_CONTRACTOR_CONTACT`.

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION",
        "message": "Incorrect \"pagination\" parameter value: {\"limit\":101}."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Incorrect "pagination" parameter value: `#PAGINATION#` | Pass `pagination` as an object with the `limit`, `page`, and `offset` keys only. `limit` — from `1` to `100`, `offset` — `0` or greater ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartprocess1038.category.list`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and whether pipelines are enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-category-get-rest-v3.md)
- [{#T}](./crm-entity-category-add-rest-v3.md)
- [{#T}](./crm-entity-category-stage-list-rest-v3.md)
