# Add Price Type Binding to Customer Group v 3.0 catalog.pricetype.group.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Manage price types" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `catalog.pricetype.group.add` adds a price type binding to a customer group and returns its fields.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new binding. [Field description](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **catalogGroupId***
[`integer`](../../../data-types.md) | Price type identifier.

The identifier can be retrieved using the [catalog.pricetype.list](../catalog-price-type-list-rest-v3.md) method ||
|| **groupId***
[`integer`](../../../data-types.md) | User group identifier.

When a price type is created, Bitrix24 automatically adds bindings to the administrators group with the identifier `1` with both access types ||
|| **access***
[`string`](../../../data-types.md) | Price access type.

Possible values:

- `Y` — permission to purchase at this price type
- `N` — permission to view this price type

Values in a different case, for example `y`, cause a validation error ||
|#

The combination of `catalogGroupId`, `groupId`, and `access` must be unique. The `id` field is filled in automatically; if you pass it in `fields`, the method returns an error.

{% note warning "" %}

In the method response, the `access` field comes in a different format: `"1"` for `Y` and an empty string for `N`. The value in the `Y` or `N` format is returned by the [catalog.priceTypeGroup.list](./catalog-price-type-group-list.md) method of the previous API version.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.pricetype.group.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":21,"groupId":2,"access":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.pricetype.group.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":21,"groupId":2,"access":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.pricetype.group.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PriceTypeGroupAddResult = {
      item: {
        id: number
        catalogGroupId: number
        groupId: number
        access: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<PriceTypeGroupAddResult>({
        method: 'catalog.pricetype.group.add',
        params: {
          fields: {
            catalogGroupId: 21,
            groupId: 2,
            access: 'Y',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Binding created:', result.item.id)
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
      async function addPriceTypeGroup() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.pricetype.group.add',
            params: {
              fields: {
                catalogGroupId: 21,
                groupId: 2,
                access: 'Y',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Binding created:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPriceTypeGroup)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.pricetype.group.add(
            fields={
                "catalogGroupId": 21,
                "groupId": 2,
                "access": "Y",
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
                'catalog.pricetype.group.add',
                [
                    'fields' => [
                        'catalogGroupId' => 21,
                        'groupId' => 2,
                        'access' => 'Y',
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
        'catalog.pricetype.group.add',
        {
            fields: {
                catalogGroupId: 21,
                groupId: 2,
                access: 'Y',
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
        'catalog.pricetype.group.add',
        [
            'fields' => [
                'catalogGroupId' => 21,
                'groupId' => 2,
                'access' => 'Y',
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
    res, err := client.Core().Call(ctx, "catalog.pricetype.group.add", b24.Params{
    	"fields": b24.Params{
    		"catalogGroupId": 21,
    		"groupId":        2,
    		"access":         "Y",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.pricetype.group.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID             int    `json:"id"`
    	CatalogGroupID int    `json:"catalogGroupId"`
    	GroupID        int    `json:"groupId"`
    	Access         string `json:"access"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(item.ID, item.GroupID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 169,
            "catalogGroupId": 21,
            "groupId": 2,
            "access": "1"
        }
    },
    "time": {
        "start": 1792668012,
        "finish": 1792668012.406017,
        "duration": 0.4060170650482178,
        "processing": 0,
        "date_start": "2026-10-22T14:20:12+02:00",
        "date_finish": "2026-10-22T14:20:12+02:00",
        "operating_reset_at": 1792668600,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the response data ||
|| **result.item**
[`object`](../../../data-types.md) | Fields of the created binding. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Binding identifier. It is required to delete the binding with the [catalog.pricetype.group.delete](./catalog-price-type-group-delete-rest-v3.md) method ||
|| **catalogGroupId**
[`integer`](../../../data-types.md) | Price type identifier ||
|| **groupId**
[`integer`](../../../data-types.md) | User group identifier ||
|| **access**
[`string`](../../../data-types.md) | Access type: `"1"` — purchase, `Y` in the request; an empty string — view, `N` in the request ||
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
                "message": "The specified access type for this group already exists"
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
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `catalogGroupId` | This request requires data type `int` in field `catalogGroupId` | Pass a number ||
|| `access` | This request requires data type `string` in field `access` | Pass the string `Y` or `N` ||
|| `-` | The specified price type does not exist | Check the price type identifier using the [catalog.pricetype.list](../catalog-price-type-list-rest-v3.md) method ||
|| `-` | The specified group does not exist | Pass the identifier of an existing user group ||
|| `-` | Invalid access type provided. The available values are: Y, N | Pass `Y` or `N` in uppercase in `access` ||
|| `-` | The specified access type for this group already exists | This binding already exists. Bindings to group `1` are created together with the price type ||
|#

If the request contains several errors, the method returns all of them in the `validation` array.

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `catalogGroupId` | Required field "catalogGroupId" is empty | Pass `catalogGroupId` in `fields` ||
|| `groupId` | Required field "groupId" is empty | Pass `groupId` in `fields` ||
|| `access` | Required field "access" is empty | Pass `access` in `fields` ||
|| `id` | Field "id" is read-only | Remove `id` from `fields` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `PriceTypeGroupDto` | Pass only `catalogGroupId`, `groupId`, `access` in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "Manage price types" access permission ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-group-delete-rest-v3.md)
- [{#T}](./catalog-price-type-group-field-list.md)
- [{#T}](./catalog-price-type-group-field-get.md)
- [{#T}](../catalog-price-type-add-rest-v3.md)
