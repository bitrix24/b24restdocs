# Get a CRM Item by ID v 3.0 crm.[type].get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view items of this CRM object type. For my company, a CRM administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.get` method returns the fields of a CRM item by ID. The object type is set by the `<type>` segment of the method name, and the set of fields in the response is set by the `select` parameter.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.get` | [Deal fields](./fields-deal.md) ||
|| Lead | `crm.lead.get` | [Lead fields](./fields-lead.md) ||
|| Contact | `crm.contact.get` | [Contact fields](./fields-contact.md) ||
|| Company | `crm.company.get` | [Company fields](./fields-company.md) ||
|| Estimate | `crm.quote.get` | [Estimate fields](./fields-quote.md) ||
|| Invoice | `crm.smartinvoice.get` | [Invoice fields](./fields-smart-invoice.md) ||
|| My company | `crm.mycompany.get` | [My company fields](./fields-my-company.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.get`, for example, `crm.smartprocess1038.get` | [SPA fields](./fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

The method name is written in lowercase: `crm.smartinvoice.get`, not `crm.smartInvoice.get`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Item ID.

The ID is returned by the [crm.&lt;type&gt;.add](./crm-entity-add-rest-v3.md) method when an item is created. IDs of existing items can be retrieved with the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of fields to return in the response. [Parameter description](#select)

If the parameter is not passed, the response contains only `id` ||
|#

### The select Parameter {#select}

Field names are written in camelCase: `title`, `stageId`, `assignedById`. Each object type has its own set of fields, which are described on these pages:

- [deal fields](./fields-deal.md)
- [lead fields](./fields-lead.md)
- [contact fields](./fields-contact.md)
- [company fields](./fields-company.md)
- [estimate fields](./fields-quote.md)
- [invoice fields](./fields-smart-invoice.md)
- [my company fields](./fields-my-company.md)
- [SPA fields](./fields-smart-process.md)

The list of fields is returned by the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) method.

The response contains only the fields from `select`. The `id` field is not added automatically — include it in the list if you need it in the response. The asterisk `*` is not supported: the method returns an unknown field error.

Linked objects are requested by the field name without the `Id` suffix: `assignedBy`, `createdBy`, `stage`, `category`. There are two ways to do this:

- the whole linked object name, for example, `assignedBy`, — the response contains all fields of the object. For linked CRM items, such as `contact`, `company`, and others, only `id` is returned
- a dot-separated path, for example, `assignedBy.firstName` or `stage.name`, — the response contains only the selected fields of the object

If a link is not set, the linked object is returned as `null`, and a multiple one, for example, `observers`, is returned as an empty array.

Custom fields are passed by name, for example, `UF_CRM_1687529546`. Their list is returned by the [crm.&lt;type&gt;.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.get`

{% endnote %}

The examples retrieve an SPA item with the type ID `1038`: the name, the stage with its name, the responsible person with their first and last name, the amount, and the creation date. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"select":["id","title","stageId","stage.name","assignedById","assignedBy.firstName","assignedBy.lastName","opportunity","currencyId","createdTime"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":15,"select":["id","title","stageId","stage.name","assignedById","assignedBy.firstName","assignedBy.lastName","opportunity","currencyId","createdTime"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ItemGetResult = {
      item: {
        id: number
        title: string
        stageId: string
        assignedById: number
        opportunity: number
        currencyId: string
        createdTime: string
        stage: {
          name: string
        } | null
        assignedBy: {
          firstName: string
          lastName: string
        } | null
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ItemGetResult>(
        'crm.smartprocess1038.get',
        {
          id: 15,
          select: [
            'id',
            'title',
            'stageId',
            'stage.name',
            'assignedById',
            'assignedBy.firstName',
            'assignedBy.lastName',
            'opportunity',
            'currencyId',
            'createdTime',
          ],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const item = response.getData()!.result.item
        console.info(item.title, item.stage?.name, item.assignedBy?.firstName)
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
      async function getItem() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.get',
            {
              id: 15,
              select: [
                'id',
                'title',
                'stageId',
                'stage.name',
                'assignedById',
                'assignedBy.firstName',
                'assignedBy.lastName',
                'opportunity',
                'currencyId',
                'createdTime',
              ],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const item = response.getData().result.item
          console.info(item.title, item.stage?.name, item.assignedBy?.firstName)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getItem)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 15,
                "select": [
                    "id",
                    "title",
                    "stageId",
                    "stage.name",
                    "assignedById",
                    "assignedBy.firstName",
                    "assignedBy.lastName",
                    "opportunity",
                    "currencyId",
                    "createdTime",
                ],
            },
            prefer_version=3,
        )
        item = response["result"]["item"]
        print(item)
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.get',
                [
                    'id' => 15,
                    'select' => [
                        'id',
                        'title',
                        'stageId',
                        'stage.name',
                        'assignedById',
                        'assignedBy.firstName',
                        'assignedBy.lastName',
                        'opportunity',
                        'currencyId',
                        'createdTime',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.get',
        {
            id: 15,
            select: [
                'id',
                'title',
                'stageId',
                'stage.name',
                'assignedById',
                'assignedBy.firstName',
                'assignedBy.lastName',
                'opportunity',
                'currencyId',
                'createdTime',
            ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.get',
        [
            'id' => 15,
            'select' => [
                'id',
                'title',
                'stageId',
                'stage.name',
                'assignedById',
                'assignedBy.firstName',
                'assignedBy.lastName',
                'opportunity',
                'currencyId',
                'createdTime',
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.get", b24.Params{
    	"id": 15,
    	"select": []string{
    		"id",
    		"title",
    		"stageId",
    		"stage.name",
    		"assignedById",
    		"assignedBy.firstName",
    		"assignedBy.lastName",
    		"opportunity",
    		"currencyId",
    		"createdTime",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.get: %w", err)
    }

    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item in response")
    }

    var item struct {
    	ID          int     `json:"id"`
    	Title       string  `json:"title"`
    	StageID     string  `json:"stageId"`
    	Opportunity float64 `json:"opportunity"`
    	CurrencyID  string  `json:"currencyId"`
    	Stage       struct {
    		Name string `json:"name"`
    	} `json:"stage"`
    	AssignedBy struct {
    		FirstName string `json:"firstName"`
    		LastName  string `json:"lastName"`
    	} `json:"assignedBy"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Title, item.Stage.Name, item.AssignedBy.FirstName)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 15,
            "title": "Equipment purchase request",
            "stageId": "DT1038_69:PREPARATION",
            "assignedById": 1,
            "opportunity": 180000,
            "currencyId": "EUR",
            "createdTime": "2026-11-03T14:39:32+03:00",
            "stage": {
                "name": "Preparation"
            },
            "assignedBy": {
                "firstName": "Anna",
                "lastName": "Schmidt"
            }
        }
    },
    "time": {
        "start": 1793706172,
        "finish": 1793706172.777702,
        "duration": 0.7777020931243896,
        "processing": 0,
        "date_start": "2026-11-03T14:42:52+03:00",
        "date_finish": "2026-11-03T14:42:52+03:00",
        "operating_reset_at": 1793706720,
        "operating": 0
    }
}
```

Without the `select` parameter, the method returns only the ID:

```json
{
    "result": {
        "item": {
            "id": 15
        }
    },
    "time": {
        "start": 1793705990,
        "finish": 1793705990.170455,
        "duration": 0.1704549789428711,
        "processing": 0,
        "date_start": "2026-11-03T14:39:50+03:00",
        "date_finish": "2026-11-03T14:39:50+03:00",
        "operating_reset_at": 1793706540,
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
[`object`](../../../data-types.md) | Values of the item fields from `select`. Linked objects are nested under their names, for example, `stage` and `assignedBy`. The fields are described on the fields page of the object type, see the links in the description of the [select parameter](#select) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field \"name\" found in entity \"EmployeeDto\"."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID #ID# was not found | Check the item ID. This object type has no item with this `id`, or the item has been deleted ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the item `id` ||
|| `id` | Field value must be 1 or greater | Pass a positive ID ||
|| `id` | This request requires data type `int` in field `id` | Pass `id` as a number ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `#DTO#` | Check the field name in `select` on the fields page of the object type. If the error names a linked object, for example, `EmployeeDto`, the field name after the dot is wrong: for the responsible person, these are `firstName` and `lastName`, not `name` ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Invalid field in `select`: #VALUE# | Pass `select` as an array of strings, for example, `["id","title"]` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.get`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-add-rest-v3.md)
- [{#T}](./crm-entity-update-rest-v3.md)
- [{#T}](./crm-entity-list-rest-v3.md)
- [{#T}](./crm-entity-delete-rest-v3.md)
- [{#T}](../fields/crm-entity-field-list-rest-v3.md)
