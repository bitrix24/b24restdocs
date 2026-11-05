# Get a CRM Pipeline by ID v 3.0 crm.[type].category.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the items of this pipeline

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.category.get` method returns the fields of a single pipeline by its ID. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.category.get` ||
|| Contact | `crm.contact.category.get` ||
|| Company | `crm.company.category.get` ||
|| SPA | `crm.smartprocess<entityTypeId>.category.get`, for example, `crm.smartprocess1038.category.get` ||
|#

For an SPA, the method name includes its type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

Deals, contacts, and companies have pipelines, and so does an SPA if pipelines are enabled in its type settings. Leads, estimates, invoices, and my company have none.

The method name is written in lowercase: `crm.smartprocess1038.category.get`, not `crm.smartProcess1038.category.get`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Pipeline ID. For the default pipeline of a deal, a contact, and a company, it is `0`.

The ID of a new pipeline is returned by the [crm.&lt;type&gt;.category.add](./crm-entity-category-add-rest-v3.md) method. IDs of existing pipelines can be retrieved with the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method ||
|#

{% note warning "" %}

The method returns all pipeline fields. It ignores the `select` parameter and does not return an error for it.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.category.get`

{% endnote %}

The examples retrieve pipeline `105` of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":105}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.category.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":105,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.category.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CategoryGetResult = {
      item: {
        id: number
        name: string
        sort: number
        isDefault: boolean
        isSystem?: boolean
        code?: string
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CategoryGetResult>(
        'crm.smartprocess1038.category.get',
        {
          id: 105,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const category = response.getData()!.result.item
        console.info(category.name, category.isDefault)
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
      async function getCategory() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.category.get',
            {
              id: 105,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const category = response.getData().result.item
          console.info(category.name, category.isDefault)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCategory)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.category.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 105,
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
                'crm.smartprocess1038.category.get',
                [
                    'id' => 105,
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
        'crm.smartprocess1038.category.get',
        {
            id: 105,
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
        'crm.smartprocess1038.category.get',
        [
            'id' => 105,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.category.get", b24.Params{
    	"id": 105,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.category.get: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("response has no item")
    }

    var category struct {
    	ID        int    `json:"id"`
    	Name      string `json:"name"`
    	Sort      int    `json:"sort"`
    	IsDefault bool   `json:"isDefault"`
    }
    if err := json.Unmarshal(raw, &category); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(category.Name, category.IsDefault)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 105,
            "name": "Equipment and furniture procurement",
            "sort": 650,
            "isDefault": false,
            "isSystem": false,
            "code": ""
        }
    },
    "time": {
        "start": 1793714730,
        "finish": 1793714730.530146,
        "duration": 0.5301458835601807,
        "processing": 0,
        "date_start": "2026-11-03T17:05:30+03:00",
        "date_finish": "2026-11-03T17:05:30+03:00",
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
|| **result.item**
[`object`](../../../data-types.md) | Pipeline fields. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Pipeline Fields {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Pipeline ID ||
|| **name**
[`string`](../../../data-types.md) | Pipeline name ||
|| **sort**
[`integer`](../../../data-types.md) | Pipeline order: the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method returns pipelines in ascending `sort` order ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Default pipeline flag.

Possible values:

- `true` — the default pipeline
- `false` — a regular pipeline ||
|| **isSystem**
[`boolean`](../../../data-types.md) | System pipeline flag, for example, for the vendor pipeline. A deal has no such field.

Possible values:

- `true` — a system pipeline
- `false` — a regular pipeline ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code of a system pipeline, for example, `CATALOG_CONTRACTOR_CONTACT`. For other pipelines, it is an empty string. A deal has no such field ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 99999 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Pipeline Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | Check the pipeline ID with the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method. This object type has no pipeline with this `id`, or the user has no permission to read its items ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the pipeline `id` ||
|| `id` | Field value must be 0 or greater | Pass a non-negative pipeline ID ||
|| `id` | This request requires data type `int` in field `id` | Pass `id` as a number ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartprocess1038.category.get`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and whether pipelines are enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-category-add-rest-v3.md)
- [{#T}](./crm-entity-category-update-rest-v3.md)
- [{#T}](./crm-entity-category-list-rest-v3.md)
- [{#T}](./crm-entity-category-delete-rest-v3.md)
- [{#T}](./crm-entity-category-stage-list-rest-v3.md)
