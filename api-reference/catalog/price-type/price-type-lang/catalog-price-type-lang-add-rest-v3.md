# Add Price Type Name Translation v 3.0 catalog.pricetype.lang.add

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

The method `catalog.pricetype.lang.add` adds a price type name translation and returns its fields.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new translation. [Field description](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **catalogGroupId***
[`integer`](../../../data-types.md) | Price type identifier.

The identifier can be retrieved using the [catalog.pricetype.list](../catalog-price-type-list-rest-v3.md) method ||
|| **lang***
[`string`](../../../data-types.md) | Language code, for example `en` or `de`.

Language codes are returned by the [catalog.pricetype.lang.getlanguages](./catalog-price-type-lang-get-languages-rest-v3.md) method. The method also accepts codes of inactive languages.

Only one translation per language can be added for a price type. To change a translation, use [catalog.pricetype.lang.update](./catalog-price-type-lang-update-rest-v3.md) ||
|| **name***
[`string`](../../../data-types.md) | Price type name in the selected language. An empty string is allowed ||
|#

The `id` field is filled in automatically; if you pass it in `fields`, the method returns an error.

{% note warning "" %}

If the price type already has a translation into this language, the method returns HTTP status **500** with the `INTERNAL_SERVER_ERROR` error. Check the existing translations using the [catalog.priceTypeLang.list](./catalog-price-type-lang-list.md) method of the previous API version.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.pricetype.lang.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":21,"lang":"en","name":"Wholesale price"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.pricetype.lang.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":21,"lang":"en","name":"Wholesale price"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.pricetype.lang.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PriceTypeLangAddResult = {
      item: {
        id: number
        catalogGroupId: number
        lang: string
        name: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<PriceTypeLangAddResult>({
        method: 'catalog.pricetype.lang.add',
        params: {
          fields: {
            catalogGroupId: 21,
            lang: 'en',
            name: 'Wholesale price',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Translation created:', result.item.id)
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
      async function addPriceTypeLang() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.pricetype.lang.add',
            params: {
              fields: {
                catalogGroupId: 21,
                lang: 'en',
                name: 'Wholesale price',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Translation created:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPriceTypeLang)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.pricetype.lang.add(
            fields={
                "catalogGroupId": 21,
                "lang": "en",
                "name": "Wholesale price",
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
                'catalog.pricetype.lang.add',
                [
                    'fields' => [
                        'catalogGroupId' => 21,
                        'lang' => 'en',
                        'name' => 'Wholesale price',
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
        'catalog.pricetype.lang.add',
        {
            fields: {
                catalogGroupId: 21,
                lang: 'en',
                name: 'Wholesale price',
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
        'catalog.pricetype.lang.add',
        [
            'fields' => [
                'catalogGroupId' => 21,
                'lang' => 'en',
                'name' => 'Wholesale price',
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
    res, err := client.Core().Call(ctx, "catalog.pricetype.lang.add", b24.Params{
    	"fields": b24.Params{
    		"catalogGroupId": 21,
    		"lang":           "en",
    		"name":           "Wholesale price",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.pricetype.lang.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID             int    `json:"id"`
    	CatalogGroupID int    `json:"catalogGroupId"`
    	Lang           string `json:"lang"`
    	Name           string `json:"name"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(item.ID, item.Lang, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 309,
            "catalogGroupId": 21,
            "lang": "en",
            "name": "Wholesale price"
        }
    },
    "time": {
        "start": 1792668051,
        "finish": 1792668051.073813,
        "duration": 0.07381296157836914,
        "processing": 0,
        "date_start": "2026-10-22T14:20:51+02:00",
        "date_finish": "2026-10-22T14:20:51+02:00",
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
[`object`](../../../data-types.md) | Fields of the created translation. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Translation identifier. It is required to update and delete the translation ||
|| **catalogGroupId**
[`integer`](../../../data-types.md) | Price type identifier ||
|| **lang**
[`string`](../../../data-types.md) | Language code ||
|| **name**
[`string`](../../../data-types.md) | Price type name in this language ||
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
                "message": "The specified price type does not exist",
                "field": "201000000000"
            },
            {
                "message": "The specified language does not exist",
                "field": "201200000010"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

In the price type and language check errors, the `field` field contains a numeric error code instead of the parameter name.

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `201000000000` | The specified price type does not exist | Check the price type identifier using the [catalog.pricetype.list](../catalog-price-type-list-rest-v3.md) method ||
|| `201200000010` | The specified language does not exist | Pass a language code from the response of the [catalog.pricetype.lang.getlanguages](./catalog-price-type-lang-get-languages-rest-v3.md) method ||
|| `name` | This request requires data type `string` in field `name` | Pass a string ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `catalogGroupId` | Required field "catalogGroupId" is empty | Pass `catalogGroupId` in `fields` ||
|| `lang` | Required field "lang" is empty | Pass `lang` in `fields` ||
|| `name` | Required field "name" is empty | Pass `name` in `fields` ||
|| `id` | Field "id" is read-only | Remove `id` from `fields` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `PriceTypeLangDto` | Pass only `catalogGroupId`, `lang`, `name` in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "Manage price types" access permission ||
|#

#### Internal Error

Error Code: `INTERNAL_SERVER_ERROR`

HTTP status: **500**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Internal server error | The price type already has a translation into this language. Change the existing translation using the [catalog.pricetype.lang.update](./catalog-price-type-lang-update-rest-v3.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-lang-update-rest-v3.md)
- [{#T}](./catalog-price-type-lang-delete-rest-v3.md)
- [{#T}](./catalog-price-type-lang-get-languages-rest-v3.md)
- [{#T}](./catalog-price-type-lang-field-list.md)
