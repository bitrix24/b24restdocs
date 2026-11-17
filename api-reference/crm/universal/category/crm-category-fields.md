# Get Pipeline Fields crm.category.fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: an administrator or an intranet user if the `intranet` module is installed; without this module, any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.&lt;type&gt;.category.field.list](../../rest-v3/categories/index.md#field-methods). Pipelines of invoices, CRM documents, and B2E documents are not available in REST 3.0 yet, so use this method for them.

{% endnote %}

The `crm.category.fields` method retrieves information about the pipeline fields of a CRM object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) of the CRM object for which to retrieve information about the pipeline fields. The object type must support pipelines: [list of supported objects](./index.md#which-objects-support-pipelines) ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldDescription = {
      type: string
      isRequired: boolean
      isReadOnly: boolean
      isImmutable: boolean
      isMultiple: boolean
      isDynamic: boolean
      title: string
      upperName: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CategoryFieldsResult = {
      fields: Record<string, FieldDescription>
    }

    try {
      const response = await $b24.actions.v2.call.make<CategoryFieldsResult>({
        method: 'crm.category.fields',
        params: {
          entityTypeId: 2,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Available category fields:', Object.keys(result.fields))
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
      async function getCategoryFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.category.fields',
            params: {
              entityTypeId: 2,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Available category fields:', Object.keys(result.fields))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCategoryFields)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.category.fields(
            entity_type_id=2,
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
                'crm.category.fields',
                [
                    'entityTypeId' => 2,
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

    ```js
    BX24.callMethod(
        "crm.category.fields",
        {
            entityTypeId: 2,
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.category.fields',
        [
            'entityTypeId' => 2
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.category.fields", b24.Params{
    	"entityTypeId": 2,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.category.fields: %w", err)
    }

    // The method wraps the response in an object with the "fields" key.
    raw, ok := b24.Unwrap(res.Result, "fields")
    if !ok {
    	return fmt.Errorf("no fields key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

Example response for a deal with `entityTypeId = 2`:

```json
{
    "result": {
        "fields": {
            "id": {
                "type": "integer",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "ID",
                "upperName": "ID"
            },
            "name": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "NAME",
                "upperName": "NAME"
            },
            "sort": {
                "type": "integer",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "SORT",
                "upperName": "SORT"
            },
            "entityTypeId": {
                "type": "integer",
                "isRequired": true,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "ENTITY_TYPE_ID",
                "upperName": "ENTITY_TYPE_ID"
            },
            "isDefault": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "IS_DEFAULT",
                "upperName": "IS_DEFAULT"
            },
            "originId": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "ORIGIN_ID",
                "upperName": "ORIGIN_ID"
            },
            "originatorId": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "ORIGINATOR_ID",
                "upperName": "ORIGINATOR_ID"
            }
        }
    },
    "time": {
        "start": 1718098629.350263,
        "finish": 1718098629.800741,
        "duration": 0.45047783851623535,
        "processing": 0.02919292449951172,
        "date_start": "2024-06-11T11:37:09+02:00",
        "date_finish": "2024-06-11T11:37:09+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root object of the result. Contains the `fields` key ||
|| **fields**
[`object`](../../../data-types.md) | Object where each key is the identifier of a [pipeline field](#fields) and each value is its description in the [crm_rest_field_description](../../data-types.md#crm_rest_field_description) format ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Field Descriptions {#fields}

#| 
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the pipeline ||
|| **name**
[`string`](../../../data-types.md) | Name of the pipeline ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) to which the pipeline belongs ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Indicates whether the pipeline is the default pipeline ||
|| **originId**
[`string`](../../../data-types.md) | Identifier of the item in the data source.

Exists only in deals ||
|| **originatorId**
[`string`](../../../data-types.md) | Identifier of the data source.

Exists only in deals ||
|| **isSystem** 
[`boolean`](../../../data-types.md) | Indicates whether the pipeline is a system pipeline.

Returned in the metadata of contacts, companies, invoices, CRM documents, B2E documents, and SPAs ||
|| **code**
[`string`](../../../data-types.md) | Alias for system pipelines.

Returned in the metadata of contacts, companies, invoices, CRM documents, B2E documents, and SPAs ||
|#

The presence of `isSystem` and `code` in the metadata does not guarantee that they are present in the [crm.category.list](./crm-category-list.md) response: for contacts and companies these fields are returned, while for SPAs they may be missing.

## Error Handling

HTTP status of the example: **400**

```json
{
    "error": "NOT_FOUND", 
    "error_description": "Smart Process Automation was not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {entityTypeId} | The required `entityTypeId` parameter is not provided ||
|| `400` | `NOT_FOUND` | Smart Process Automation was not found | A CRM object type with the specified `entityTypeId` was not found ||
|| `400` | `ENTITY_TYPE_NOT_SUPPORTED` | CRM entity "{entityTypeName}" is not supported | The CRM object does not support pipelines. The name of the object type is returned in place of `{entityTypeName}` ||
|| `403` | `allowed_only_intranet_user` | This action is available only to Intranet users | The user is not an administrator or an intranet user while the `intranet` module is installed ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}


## Continue Learning 

- [{#T}](./crm-category-add.md)
- [{#T}](./crm-category-update.md)
- [{#T}](./crm-category-get.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-delete.md)
