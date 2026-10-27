# Get a List of CRM Forms crm.form.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the permission to view CRM forms

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `crm.form.list` returns a list of CRM forms with filtering, sorting, and pagination.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of fields to return.

You can specify an entire section, for example `crm`, or a section field using dot notation, for example `crm.entityScheme`. All fields are described in [CRM Form Structure](./data-types.md#crm_form).

By default, the simple fields are returned: `id`, `code`, `name`, `active`, `system`, `languageId`, `templateId`, `createdTime`, `updatedTime`, `activeChangedById`, `activeChangedTime`.

The `appearance`, `crm`, `fields`, `dependencies`, `presetFields`, `responsible`, `agreements`, `submission`, `payment`, `callback`, `booking`, `analytics`, `spamProtection`, `integrations` sections are returned only if specified in `select` ||
|| **filter**
[`array`](../../data-types.md) | Selection conditions in the REST 3.0 format: an array of conditions `[field, operator, value]`. The format is described in the [REST 3.0 overview](../../rest-v3.md).

Fields available for filtering:

- `id` — form identifier
- `code` — form symbolic code
- `name` — form name. Only exact match search is supported; partial match operators are not supported
- `active` — form activity: `true` or `false`
- `createdTime` — creation date
- `updatedTime` — modification date
- `crm.entityScheme` — CRM scheme, that is, the objects the form creates. [crm.form.configuration.get](./setup/crm-form-configuration-get.md) returns the scheme identifiers
- `responsible.userIds` — responsible person. Pass a user identifier or an array of identifiers, positive numbers only

For dates, use the `>`, `>=`, `<`, `<=` comparisons: listing several dates with `in` is not allowed.

Filter limits: up to 50 conditions and groups, nesting up to 5 levels, up to 1000 values across all `in` operators ||
|| **order**
[`object`](../../data-types.md) | Sorting in the format `{"field": "ASC"}` or `{"field": "DESC"}`.

Fields available for sorting: `id`, `name`, `active`, `createdTime`, `updatedTime`, `crm.entityScheme`.

The order of the keys sets the sorting order. The result is always additionally sorted by `id`, so forms are neither lost nor repeated on adjacent pages ||
|| **pagination**
[`object`](../../data-types.md) | Pagination:

- `limit` — how many forms to return, from 1 to 1000
- `offset` — how many forms to skip from the beginning of the selection

By default, 50 forms are returned ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.list`

{% endnote %}

The example retrieves active forms that create a lead and were created since the beginning of 2026.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","active","crm.entityScheme","responsible.userIds","updatedTime"],"filter":[["active","=",true],["crm.entityScheme","=",1],["createdTime",">=","2026-01-01T00:00:00+02:00"]],"order":{"createdTime":"DESC"},"pagination":{"limit":50,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","active","crm.entityScheme","responsible.userIds","updatedTime"],"filter":[["active","=",true],["crm.entityScheme","=",1],["createdTime",">=","2026-01-01T00:00:00+02:00"]],"order":{"createdTime":"DESC"},"pagination":{"limit":50,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormListResult = {
      items: Array<{
        id: number
        name: string
        active: boolean
        updatedTime: string
        crm: {
          entityScheme: number
        }
        responsible: {
          userIds: number[]
        }
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<FormListResult>({
        method: 'crm.form.list',
        params: {
          select: [
            'id',
            'name',
            'active',
            'crm.entityScheme',
            'responsible.userIds',
            'updatedTime',
          ],
          filter: [
            ['active', '=', true],
            ['crm.entityScheme', '=', 1],
            ['createdTime', '>=', '2026-01-01T00:00:00+02:00'],
          ],
          order: {
            createdTime: 'DESC',
          },
          pagination: {
            limit: 50,
            offset: 0,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.items)
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
      async function listForms() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.list',
            params: {
              select: [
                'id',
                'name',
                'active',
                'crm.entityScheme',
                'responsible.userIds',
                'updatedTime',
              ],
              filter: [
                ['active', '=', true],
                ['crm.entityScheme', '=', 1],
                ['createdTime', '>=', '2026-01-01T00:00:00+02:00'],
              ],
              order: {
                createdTime: 'DESC',
              },
              pagination: {
                limit: 50,
                offset: 0,
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
          console.info(result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listForms)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # client = Client(token, prefer_version=3)

    try:
        bitrix_response = client.crm.form.list(
            select=[
                "id",
                "name",
                "active",
                "crm.entityScheme",
                "responsible.userIds",
                "updatedTime",
            ],
            filter=[
                ["active", "=", True],
                ["crm.entityScheme", "=", 1],
                ["createdTime", ">=", "2026-01-01T00:00:00+02:00"],
            ],
            order={
                "createdTime": "DESC",
            },
            pagination={
                "limit": 50,
                "offset": 0,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.form.list',
                [
                    'select' => [
                        'id',
                        'name',
                        'active',
                        'crm.entityScheme',
                        'responsible.userIds',
                        'updatedTime',
                    ],
                    'filter' => [
                        ['active', '=', true],
                        ['crm.entityScheme', '=', 1],
                        ['createdTime', '>=', '2026-01-01T00:00:00+02:00'],
                    ],
                    'order' => [
                        'createdTime' => 'DESC',
                    ],
                    'pagination' => [
                        'limit' => 50,
                        'offset' => 0,
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
        'crm.form.list',
        {
          select: [
            'id',
            'name',
            'active',
            'crm.entityScheme',
            'responsible.userIds',
            'updatedTime',
          ],
          filter: [
            ['active', '=', true],
            ['crm.entityScheme', '=', 1],
            ['createdTime', '>=', '2026-01-01T00:00:00+02:00'],
          ],
          order: {
            createdTime: 'DESC',
          },
          pagination: {
            limit: 50,
            offset: 0,
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
        'crm.form.list',
        [
            'select' => [
                'id',
                'name',
                'active',
                'crm.entityScheme',
                'responsible.userIds',
                'updatedTime',
            ],
            'filter' => [
                ['active', '=', true],
                ['crm.entityScheme', '=', 1],
                ['createdTime', '>=', '2026-01-01T00:00:00+02:00'],
            ],
            'order' => [
                'createdTime' => 'DESC',
            ],
            'pagination' => [
                'limit' => 50,
                'offset' => 0,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "crm.form.list", b24.Params{
    	"select": []string{"id", "name", "active", "crm.entityScheme", "responsible.userIds", "updatedTime"},
    	"filter": []any{
    		[]any{
    			"active",
    			"=",
    			true,
    		},
    		[]any{
    			"crm.entityScheme",
    			"=",
    			1,
    		},
    		[]string{"createdTime", ">=", "2026-01-01T00:00:00+02:00"},
    	},
    	"order": map[string]any{
    		"createdTime": "DESC",
    	},
    	"pagination": map[string]any{
    		"limit": 50,
    		"offset": 0,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.form.list: %w", err)
    }

    // The method wraps the response in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("the response has no items key")
    }

    var forms []struct {
    	ID     int    `json:"id"`
    	Name   string `json:"name"`
    	Active bool   `json:"active"`
    }
    if err := json.Unmarshal(raw, &forms); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(len(forms))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 45,
                "name": "Contact information",
                "active": true,
                "updatedTime": "2026-03-11T16:47:17+02:00",
                "crm": {
                    "entityScheme": 1
                },
                "responsible": {
                    "userIds": []
                }
            }
        ]
    },
    "time": {
        "start": 1792593038,
        "finish": 1792593038.734321,
        "duration": 0.734321117401123,
        "processing": 0,
        "date_start": "2026-10-21T17:30:38+02:00",
        "date_finish": "2026-10-21T17:30:38+02:00",
        "operating_reset_at": 1792593600,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **items**
[`array`](../../data-types.md) | Array of forms. The set of fields of each form depends on `select`. [CRM Form Structure](./data-types.md#crm_form) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION",
        "message": "Unknown filter operator \"%\"."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `system` | DTO `FormDto` in field `system` requires attribute `Filterable` to perform this request | Filter only by the fields from the `filter` parameter description ||
|| `code` | DTO `FormDto` in field `code` requires attribute `Sortable` to perform this request | Sort only by the fields from the `order` parameter description ||
|| `id` | This request requires data type `int` in field `id` | Check the value type in the filter condition ||
|| `responsible.userIds` | This request requires data type `positive int` in field `responsible.userIds` | Pass a positive user identifier ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `%` | Use REST 3.0 operators. Search by name for an exact match with the `=` operator ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Filter node limit exceeded.` | Shorten the filter: up to 50 conditions and groups, nesting up to 5 levels, up to 1000 values across all `in` operators ||
|#

#### Sorting and Pagination Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `UP` | Pass an object like `{"createdTime":"DESC"}` with the `ASC` or `DESC` direction ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":1001}` | Pass `limit` from 1 to 1000 and a non-negative `offset` ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `foo` found in entity `CrmSettingsDto` | Pass fields from [CRM Form Structure](./data-types.md#crm_form) ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the permission to view CRM forms ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-get.md)
- [{#T}](./data-types.md)
- [{#T}](./crm-form-field-list.md)
- [{#T}](./index.md)
