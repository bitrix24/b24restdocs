# Copy a CRM Item v 3.0 crm.[type].copy

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the source item and to add items of this CRM object type. For my company, a CRM administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.copy` method creates a copy of a CRM item and returns the ID of the copy. The object type is set by the `<type>` segment of the method name.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.copy` | [Deal fields](./fields-deal.md) ||
|| Lead | `crm.lead.copy` | [Lead fields](./fields-lead.md) ||
|| Contact | `crm.contact.copy` | [Contact fields](./fields-contact.md) ||
|| Company | `crm.company.copy` | [Company fields](./fields-company.md) ||
|| Estimate | `crm.quote.copy` | [Estimate fields](./fields-quote.md) ||
|| Invoice | `crm.smartinvoice.copy` | [Invoice fields](./fields-smart-invoice.md) ||
|| My company | `crm.mycompany.copy` | [My company fields](./fields-my-company.md) ||
|| SPA | `crm.smartprocess<entityTypeId>.copy`, for example, `crm.smartprocess1038.copy` | [SPA fields](./fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs `entityTypeId` are returned by the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method.

The method name is written in lowercase: `crm.smartinvoice.copy`, not `crm.smartInvoice.copy` or `crm.smart_invoice.copy`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | ID of the item to copy.

The ID can be retrieved with the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) method ||
|| **fields**
[`object`](../../../data-types.md) | Object with fields whose values in the copy must differ from the source item, in the `{"field": "value"}` format. [Parameter description](#fields).

Default: the copy receives the values of the source item ||
|#

The method does not accept other parameters: for an extra key in the request body, it returns a validation error.

### What Is Copied

The copy receives the field values of the source item, including the stage, amount, comment, the multiple fields `phone`, `email`, `web`, `im`, and product rows. The name does not change: the copy has the same `title` as the source item.

The following is not copied:

- fields that Bitrix24 fills in automatically: `id`, `createdTime`, `createdById`, `updatedTime`, `updatedById`. The user who called the method becomes the creator of the copy
- activities of the source item

Files in the `photo` and `logo` fields and in custom fields of the "File" type are received by the copy as new files.

### Parameter fields {#fields}

Pass in `fields` only the fields you need to change. The value format and validation rules are the same as for the [crm.&lt;type&gt;.update](./crm-entity-update-rest-v3.md) method:

- field names are written in camelCase: `title`, `assignedById`, `stageId`
- read-only fields, such as `id` or `contactId`, cause an error
- `stageId` must be a stage of the copy's pipeline, and `categoryId` must be an existing pipeline of this object type
- if you pass only `categoryId`, the copy goes to the first stage of the new pipeline

The fields of each object type are described on the fields page of the type; the links are in the table above. An empty `fields` object is the same as a call without `fields`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.copy`

{% endnote %}

The examples copy an SPA item with the type ID `1038` and set a new name and amount for the copy. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":41,"fields":{"title":"Copy of the purchase request","opportunity":3000}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.copy
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":41,"fields":{"title":"Copy of the purchase request","opportunity":3000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.copy
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CopyResult = {
      id: number
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<CopyResult>(
        'crm.smartprocess1038.copy',
        {
          id: 41,
          fields: {
            title: 'Copy of the purchase request',
            opportunity: 3000,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Copy ID:', result.id)
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
      async function copyItem() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.copy',
            {
              id: 41,
              fields: {
                title: 'Copy of the purchase request',
                opportunity: 3000,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Copy ID:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', copyItem)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.copy"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 41,
                "fields": {
                    "title": "Copy of the purchase request",
                    "opportunity": 3000,
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["id"])
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
                'crm.smartprocess1038.copy',
                [
                    'id' => 41,
                    'fields' => [
                        'title' => 'Copy of the purchase request',
                        'opportunity' => 3000,
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
        'crm.smartprocess1038.copy',
        {
            id: 41,
            fields: {
                title: 'Copy of the purchase request',
                opportunity: 3000,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.copy',
        [
            'id' => 41,
            'fields' => [
                'title' => 'Copy of the purchase request',
                'opportunity' => 3000,
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
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.copy", b24.Params{
    	"id": 41,
    	"fields": b24.Params{
    		"title":       "Copy of the purchase request",
    		"opportunity": 3000,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.copy: %w", err)
    }

    var created struct {
    	ID int `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &created); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(created.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 65
    },
    "time": {
        "start": 1795431045,
        "finish": 1795431045.31671,
        "duration": 0.3167099952697754,
        "processing": 0,
        "date_start": "2026-11-23T13:50:45+01:00",
        "date_finish": "2026-11-23T13:50:45+01:00",
        "operating_reset_at": 1795431600,
        "operating": 0.349790096282959
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.id**
[`integer`](../../../data-types.md) | ID of the copy. The fields of the copy can be retrieved with the [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Field \"id\" is read-only.",
                "field": "id"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the `id` of the source item ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|| `id` | Field value must be 1 or greater | Pass a positive ID ||
|| `fields` | This request requires data type `object` in field `fields` | Pass `fields` as an object `{"field": "value"}`, not as a string or an array ||
|| `foo` | Error validating request object | An extra key was passed in the request body; `field` contains its name. Keep only `id` and `fields` ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Field `id` is read-only | Remove `id` from `fields`: the ID of the copy is assigned automatically ||
|| `contactId` | Field `contactId` is read-only | Remove the read-only field from `fields`. Pass contacts in `contactsId` ||
|| `stageId` | Field value is not one of the possible values: NEW, PREPARATION, PREPAYMENT_INVOICE, EXECUTING, FINAL_INVOICE, WON, LOSE, APOLOGY | Pass a stage from the copy's pipeline. The error message lists the allowed stages ||
|| `categoryId` | Field value is not one of the possible values | Pass the ID of an existing pipeline of this object type ||
|| `assignedById` | Field value is not one of the possible values | Pass the ID of an existing user ||
|#

#### Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `999999999` was not found | The source item does not exist for this object type, or you have no permission to view it. Check `id` and the method name: `crm.company.copy` does not find my companies, and `crm.mycompany.copy` does not find client companies ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `foo` found in entity `EntityTypeSmartProcess1038Dto` | Check the field name in `fields` against the fields page of the object type. Field names are written in camelCase: `title`, not `TITLE` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | You don't have permission to view this item #ENTITY_DESCRIPTION# | You have no permission to add items of this object type. Ask the CRM administrator to grant the permission ||
|| `-` | Access denied | For `crm.lead.copy`: leads are disabled on Bitrix24. For `crm.mycompany.copy`: you are not a CRM administrator ||
|#

#### Copy Not Fully Created

Error Code: `CRM_COPY_INCOMPLETE`, HTTP status: **500**

```json
{
    "error": {
        "code": "CRM_COPY_INCOMPLETE",
        "message": "Generic error",
        "persistence": "created",
        "id": 65
    }
}
```

The error occurred after the copy might have been saved. The response contains additional keys:

- `persistence` — state of the copy: `created` — the copy was created, `unknown` — it is unknown whether the copy was created
- `id` — ID of the copy, if known

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Generic error | Do not repeat the call right away: this creates a second copy. If the response contains `id`, check the copy with the [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) method. If there is no `id`, find the copy with the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) method, for example, by `title` and `createdTime` ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `crm.smart_invoice.copy` was not found | Check the method name: it is written in lowercase, for example, `crm.smartinvoice.copy`. For an SPA, check `entityTypeId` with the [crm.smartprocess.list](../smart-process-types/crm-smart-process-list-rest-v3.md) method. CRM documents and B2E documents have no copy method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-add-rest-v3.md)
- [{#T}](./crm-entity-update-rest-v3.md)
- [{#T}](./crm-entity-get-rest-v3.md)
- [{#T}](./crm-entity-list-rest-v3.md)
