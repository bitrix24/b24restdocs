# Get CRM Object Fields for a Form crm.form.entityfield.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view CRM forms

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.form.entityfield.list` method returns the CRM object fields that can be used to build a CRM form.

{% note warning "" %}

Fields with choice options cannot be added to a form via REST: lists, checkboxes, and radio buttons. The method returns such fields, for example with the `list` type, but [crm.form.validate](../crm-form-validate.md) and [crm.form.add](../crm-form-add.md) do not accept them.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../../../data-types.md) | Filter by the CRM objects whose fields need to be retrieved. The filter is required and contains exactly one condition on the `entityName` field:

- `["entityName", "=", "LEAD"]` — fields of one object
- `["entityName", "in", ["CONTACT", "DEAL"]]` — fields of several objects, no more than 10 values

Object codes are returned by the [crm.form.configuration.get](./crm-form-configuration-get.md) method in the `entitySchemes[].entityNames` field, for example `LEAD`, `CONTACT`, `COMPANY`, `DEAL`, `QUOTE`, `DYNAMIC_174`. The code is case-insensitive.

Other fields, operators other than `=` and `in`, `or` logic, and negation are not supported in the filter. If you pass an empty list in `in`, the method returns an empty `items` array ||
|#

Sorting, pagination, and `select` are not supported: the method returns all fields of the selected objects.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.entityfield.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["entityName","in",["CONTACT","DEAL"]]]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.entityfield.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["entityName","in",["CONTACT","DEAL"]]],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.entityfield.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormEntityFieldListResult = {
      items: Array<{
        name: string
        label: string
        type: string
        required: boolean
        multiple: boolean
        entityName: string
        entityLabel: string
        items: Array<{ id: string, label: string }>
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<FormEntityFieldListResult>({
        method: 'crm.form.entityfield.list',
        params: {
          filter: [
            ['entityName', 'in', ['CONTACT', 'DEAL']],
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.items.map(field => `${field.name}: ${field.label}`))
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
      async function getFormEntityFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.entityfield.list',
            params: {
              filter: [
                ['entityName', 'in', ['CONTACT', 'DEAL']],
              ],
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

      document.addEventListener('DOMContentLoaded', getFormEntityFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.form.entityfield.list(
            filter=[
                ["entityName", "in", ["CONTACT", "DEAL"]],
            ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.form.entityfield.list',
                [
                    'filter' => [
                        ['entityName', 'in', ['CONTACT', 'DEAL']],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.form.entityfield.list',
        {
            filter: [
                ['entityName', 'in', ['CONTACT', 'DEAL']],
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
        'crm.form.entityfield.list',
        [
            'filter' => [
                ['entityName', 'in', ['CONTACT', 'DEAL']],
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
    res, err := client.Core().Call(ctx, "crm.form.entityfield.list", b24.Params{
    	"filter": []any{
    		[]any{"entityName", "in", []string{"CONTACT", "DEAL"}},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.form.entityfield.list: %w", err)
    }

    // The method wraps the list in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("no items key in the response")
    }

    var items []struct {
    	Name       string `json:"name"`
    	Label      string `json:"label"`
    	Type       string `json:"type"`
    	Required   bool   `json:"required"`
    	EntityName string `json:"entityName"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, field := range items {
    	fmt.Println(field.EntityName, field.Name, field.Type)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The example is shortened: three of the 98 fields are left in the list.

```json
{
    "result": {
        "items": [
            {
                "name": "CONTACT_HONORIFIC",
                "label": "Salutation",
                "type": "list",
                "required": false,
                "multiple": false,
                "entityName": "CONTACT",
                "entityLabel": "Contact",
                "items": [
                    {
                        "id": "HNR_RU_1",
                        "label": "Mr."
                    },
                    {
                        "id": "HNR_RU_2",
                        "label": "Mrs."
                    }
                ]
            },
            {
                "name": "CONTACT_NAME",
                "label": "Name",
                "type": "string",
                "required": true,
                "multiple": false,
                "entityName": "CONTACT",
                "entityLabel": "Contact",
                "items": []
            },
            {
                "name": "CONTACT_PHONE",
                "label": "Phone",
                "type": "typed_string",
                "required": false,
                "multiple": true,
                "entityName": "CONTACT",
                "entityLabel": "Contact",
                "items": []
            }
        ]
    },
    "time": {
        "start": 1792594067,
        "finish": 1792594068.0578,
        "duration": 1.057800054550171,
        "processing": 1,
        "date_start": "2026-10-21T17:47:47+02:00",
        "date_finish": "2026-10-21T17:47:48+02:00",
        "operating_reset_at": 1792594620,
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
|| **items**
[`object[]`](../../../data-types.md) | Array of CRM object fields [(detailed description)](#items) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Field code, for example `CONTACT_NAME`. Passed in `fields[].name` when creating or updating a form ||
|| **label**
[`string`](../../../data-types.md) | Field title ||
|| **type**
[`string`](../../../data-types.md) | Field type, for example `string`, `typed_string`, `list`, `date`, `file`, `address`. Fields of the `list` and `checkbox` types cannot be added to a form via REST ||
|| **required**
[`boolean`](../../../data-types.md) | Flag indicating that the field is required in CRM ||
|| **multiple**
[`boolean`](../../../data-types.md) | Multiple field flag ||
|| **entityName**
[`string`](../../../data-types.md) | Code of the CRM object the field belongs to ||
|| **entityLabel**
[`string`](../../../data-types.md) | CRM object name ||
|| **items**
[`object[]`](../../../data-types.md) | Value options of a list field: elements with the fields `id` — the value saved in CRM, and `label` — the option title. For other fields, the array is empty ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_CRM_V2_INFRASTRUCTURE_REST_EXCEPTION_FORM_UNKNOWNFORMENTITYEXCEPTION",
        "message": "Unknown CRM item types: FOO"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Field value must not be empty | Pass a filter with a condition on `entityName` ||
|| `name` | DTO `EntityFieldDto` in field `name` requires attribute `Filterable` to perform this request | Filter only by `entityName` ||
|#

#### Filter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `#FILTER#` | Pass one condition on `entityName` with the `=` or `in` operator. Specify no more than 10 values in `in`, and do not use `or` logic or negation ||
|#

#### Unknown CRM Object Error

Error Code: `BITRIX_CRM_V2_INFRASTRUCTURE_REST_EXCEPTION_FORM_UNKNOWNFORMENTITYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown CRM item types: `#ENTITY_NAMES#` | Specify object codes from `entitySchemes[].entityNames` of the [crm.form.configuration.get](./crm-form-configuration-get.md) method ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Grant the user permission to view CRM forms ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-configuration-get.md)
- [{#T}](../crm-form-add.md)
- [{#T}](../crm-form-validate.md)
- [{#T}](./crm-form-entityfield-field-list.md)
- [{#T}](./index.md)
