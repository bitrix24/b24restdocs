# Add Vendor to Inventory Document v 3.0 catalog.document.contractor.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create and edit" and "View" permissions for documents of the "Incoming" type and the "View and select warehouses" and "View product catalog" permissions

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.document.contractor.add` binds a CRM contact or company as a vendor to an incoming document and returns the binding.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object with the fields of the new binding. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **documentId***
[`integer`](../../data-types.md) | Identifier of the incoming document of type `A`. The document must not be processed.

The identifier can be retrieved using the [catalog.document.list](../document/catalog-document-list-rest-v3.md) method or from the [catalog.document.add](../document/catalog-document-add-rest-v3.md) response ||
|| **entityTypeId***
[`integer`](../../data-types.md) | CRM object type.

Possible values:

- `3` — contact
- `4` — company ||
|| **entityId***
[`integer`](../../data-types.md) | Identifier of the contact or company.

The identifier can be retrieved using the [crm.contact.list](../../crm/contacts/crm-contact-list.md) and [crm.company.list](../../crm/companies/crm-company-list.md) methods.

A document can have several contacts and only one company bound to it. The method does not check that the CRM object exists ||
|#

The `id` field is filled in automatically. If you pass it in `fields`, the method returns an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.contractor.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"documentId":325,"entityTypeId":3,"entityId":1}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.contractor.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"documentId":325,"entityTypeId":3,"entityId":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.contractor.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentContractorAddResult = {
      item: {
        id: number
        documentId: number
        entityTypeId: number
        entityId: number
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentContractorAddResult>({
        method: 'catalog.document.contractor.add',
        params: {
          fields: {
            documentId: 325,
            entityTypeId: 3,
            entityId: 1,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Contractor bound:', result.item.id)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function documentContractorAdd() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.contractor.add',
            params: {
              fields: {
                documentId: 325,
                entityTypeId: 3,
                entityId: 1,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Contractor bound:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', documentContractorAdd)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.contractor.add(
            fields={
                "documentId": 325,
                "entityTypeId": 3,
                "entityId": 1,
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
                'catalog.document.contractor.add',
                [
                    'fields' => [
                        'documentId' => 325,
                        'entityTypeId' => 3,
                        'entityId' => 1,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.document.contractor.add',
        {
            fields: {
                documentId: 325,
                entityTypeId: 3,
                entityId: 1,
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
        'catalog.document.contractor.add',
        [
            'fields' => [
                'documentId' => 325,
                'entityTypeId' => 3,
                'entityId' => 1,
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
    res, err := client.Core().Call(ctx, "catalog.document.contractor.add", b24.Params{
    	"fields": b24.Params{
    		"documentId":   325,
    		"entityTypeId": 3,
    		"entityId":     1,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.contractor.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID           int `json:"id"`
    	DocumentID   int `json:"documentId"`
    	EntityTypeID int `json:"entityTypeId"`
    	EntityID     int `json:"entityId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.EntityID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 29,
            "documentId": 325,
            "entityTypeId": 3,
            "entityId": 1
        }
    },
    "time": {
        "start": 1792665375,
        "finish": 1792665375.827902,
        "duration": 0.82790207862854,
        "processing": 0,
        "date_start": "2026-10-22T13:36:15+02:00",
        "date_finish": "2026-10-22T13:36:15+02:00",
        "operating_reset_at": 1792665960,
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
|| **result.item**
[`object`](../../data-types.md) | Fields of the created binding. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Binding identifier ||
|| **documentId**
[`integer`](../../data-types.md) | Inventory document identifier ||
|| **entityTypeId**
[`integer`](../../data-types.md) | CRM object type: `3` — contact, `4` — company ||
|| **entityId**
[`integer`](../../data-types.md) | Identifier of the contact or company ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "This document already has a Company contractor"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `documentId` | Required field `documentId` is empty | Pass `documentId` in `fields` ||
|| `entityTypeId` | Required field `entityTypeId` is empty | Pass `entityTypeId` in `fields` ||
|| `entityId` | Required field `entityId` is empty | Pass `entityId` in `fields` ||
|| `id` | Field `id` is read-only | Remove `id` from `fields` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

Several field validation errors can arrive in one response in the `validation` array.

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `documentId` | This request requires data type `int` in field `documentId` | Pass an integer ||
|| `-` | Store document was not found | There is no document with this identifier, or it is not available to the user. Check the identifier using the [catalog.document.list](../document/catalog-document-list-rest-v3.md) method ||
|| `-` | Type of store document is wrong | A vendor can be bound only to an incoming document of type `A` ||
|| `-` | Unable to edit conducted document | The document is processed. Cancel processing using the [catalog.document.cancel](../document/catalog-document-cancel-rest-v3.md) method ||
|| `-` | Wrong entity type id | Pass `3` or `4` in `entityTypeId` ||
|| `-` | Wrong entity id | Pass a positive number in `entityId` ||
|| `-` | This contractor has been already bound to this document | This contact or company is already bound to the document ||
|| `-` | This document already has a Company contractor | A company is already bound to the document. Delete the previous binding using the [catalog.document.contractor.delete](./catalog-documentcontractor-delete-rest-v3.md) method ||
|| `-` | Contractors should be provided by CRM | Document vendors are not taken from CRM. The methods work only if inventory document vendors are managed in CRM ||
|| `-` | CRM module is required for document contractors | The methods work only if the CRM module is installed ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `DocumentContractorDto` | Pass only `documentId`, `entityTypeId`, `entityId` in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for incoming documents, warehouses, and the catalog ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-documentcontractor-list-rest-v3.md)
- [{#T}](./catalog-documentcontractor-delete-rest-v3.md)
- [{#T}](./catalog-documentcontractor-field-list.md)
- [{#T}](./index.md)
