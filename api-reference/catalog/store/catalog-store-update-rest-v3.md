# Update Warehouse v 3.0 catalog.store.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit warehouses" access permission if the warehouse is available to them through the "View and select warehouses" access permission or they created the warehouse

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.store.update` method updates a warehouse and returns its data after the update.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Warehouse identifier.

The identifier can be retrieved using the [catalog.store.list](./catalog-store-list-rest-v3.md) method or from the [catalog.store.add](./catalog-store-add-rest-v3.md) response ||
|| **fields***
[`object`](../../data-types.md) | Object with the warehouse fields to update. The other fields retain their values. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **address**
[`string`](../../data-types.md) | Warehouse address. An empty string is not accepted ||
|| **title**
[`string`](../../data-types.md) | Warehouse name ||
|| **active**
[`boolean`](../../data-types.md) | Warehouse activity.

Possible values:

- `true` — the warehouse is active
- `false` — the warehouse is inactive

The values `Y`, `N`, and `null` are not accepted ||
|| **description**
[`string`](../../data-types.md) | Warehouse description ||
|| **gpsN**
[`double`](../../data-types.md) | Warehouse latitude, for example, `52.51735`. Pass a number: a string causes an error ||
|| **gpsS**
[`double`](../../data-types.md) | Warehouse longitude, for example, `13.38886`. Pass a number: a string causes an error ||
|| **image**
[`object`](../../data-types.md) | Warehouse image. [Object description](#image)

To delete the image, pass `null`. If the field is not provided, the image does not change ||
|| **phone**
[`string`](../../data-types.md) | Warehouse phone number ||
|| **schedule**
[`string`](../../data-types.md) | Warehouse working hours, for example, `Mon–Fri 9:00–18:00` ||
|| **email**
[`string`](../../data-types.md) | Warehouse email. The method does not validate the address format ||
|| **issuingCenter**
[`boolean`](../../data-types.md) | Pickup point flag.

Possible values:

- `true` — the warehouse operates as a pickup point
- `false` — a regular warehouse

The values `Y`, `N`, and `null` are not accepted ||
|| **sort**
[`integer`](../../data-types.md) | Sort order of the warehouse in lists ||
|| **code**
[`string`](../../data-types.md) | Warehouse symbolic code. The method does not check that the code is unique ||
|| **xmlId**
[`string`](../../data-types.md) | Warehouse external ID. Use it to link the warehouse to a record in an external system ||
|#

The method fills in the `id`, `imageId`, `imageUrl`, `userId`, `modifiedBy`, `dateCreate`, and `dateModify` fields itself. If you pass them in `fields`, the method returns an error.

REST 3.0 methods do not support custom fields of a warehouse.

### Object image {#image}

#|
|| **Name**
`type` | **Description** ||
|| **upload***
[`object`](../../data-types.md) | Data of the file to upload. [Object description](#upload) ||
|#

### Object image.upload {#upload}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | File name with an extension, for example, `store.png`. Only an image can be uploaded: the method determines the file type by its extension ||
|| **data**
[`string`](../../data-types.md) | File content in Base64 format.

Pass either `data` or `url`, not both ||
|| **url**
[`string`](../../data-types.md) | Public link to the file. Bitrix24 downloads the file from the link.

Pass either `data` or `url`, not both ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.store.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"fields":{"title":"Friedrichstrasse 51 Warehouse","phone":"+49 30 1111111","schedule":"Mon–Sat 9:00–20:00"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.store.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"fields":{"title":"Friedrichstrasse 51 Warehouse","phone":"+49 30 1111111","schedule":"Mon–Sat 9:00–20:00"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.store.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StoreUpdateResult = {
      item: {
        id: number
        title: string
        active: boolean
        address: string
        description: string
        gpsN: number | null
        gpsS: number | null
        imageId: number
        imageUrl: string
        dateModify: string
        dateCreate: string | null
        userId: number
        modifiedBy: number
        phone: string
        schedule: string
        xmlId: string
        sort: number
        email: string
        issuingCenter: boolean
        code: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<StoreUpdateResult>({
        method: 'catalog.store.update',
        params: {
          id: 51,
          fields: {
            title: 'Friedrichstrasse 51 Warehouse',
            phone: '+49 30 1111111',
            schedule: 'Mon–Sat 9:00–20:00',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Store updated:', result.item.title)
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
      async function updateStore() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.store.update',
            params: {
              id: 51,
              fields: {
                title: 'Friedrichstrasse 51 Warehouse',
                phone: '+49 30 1111111',
                schedule: 'Mon–Sat 9:00–20:00',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Store updated:', result.item.title)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateStore)
    </script>
    ```

- Python

    The `catalog.store.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.store.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 51,
                "fields": {
                    "title": "Friedrichstrasse 51 Warehouse",
                    "phone": "+49 30 1111111",
                    "schedule": "Mon–Sat 9:00–20:00",
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
                'catalog.store.update',
                [
                    'id' => 51,
                    'fields' => [
                        'title' => 'Friedrichstrasse 51 Warehouse',
                        'phone' => '+49 30 1111111',
                        'schedule' => 'Mon–Sat 9:00–20:00',
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
        'catalog.store.update',
        {
            id: 51,
            fields: {
                title: 'Friedrichstrasse 51 Warehouse',
                phone: '+49 30 1111111',
                schedule: 'Mon–Sat 9:00–20:00',
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
        'catalog.store.update',
        [
            'id' => 51,
            'fields' => [
                'title' => 'Friedrichstrasse 51 Warehouse',
                'phone' => '+49 30 1111111',
                'schedule' => 'Mon–Sat 9:00–20:00',
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
    res, err := client.Core().Call(ctx, "catalog.store.update", b24.Params{
    	"id": 51,
    	"fields": b24.Params{
    		"title":    "Friedrichstrasse 51 Warehouse",
    		"phone":    "+49 30 1111111",
    		"schedule": "Mon–Sat 9:00–20:00",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.store.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID            int    `json:"id"`
    	Title         string `json:"title"`
    	Active        bool   `json:"active"`
    	Address       string `json:"address"`
    	IssuingCenter bool   `json:"issuingCenter"`
    	ImageURL      string `json:"imageUrl"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Title, item.Address)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 51,
            "title": "Friedrichstrasse 51 Warehouse",
            "active": true,
            "address": "Friedrichstraße 51, 10117 Berlin, Germany",
            "description": "Pickup warehouse",
            "gpsN": 52.51735,
            "gpsS": 13.38886,
            "imageId": 40757,
            "imageUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/fee/fee4295e698466f04b617988b6f415ff/508cb2bfea465e513a642ec8849f1023.png",
            "dateModify": "2026-11-17T16:37:13+02:00",
            "dateCreate": "2026-11-17T16:37:04+02:00",
            "userId": 1295,
            "modifiedBy": 1295,
            "phone": "+49 30 1111111",
            "schedule": "Mon–Sat 9:00–20:00",
            "xmlId": "store-berlin-51",
            "sort": 200,
            "email": "store@example.com",
            "issuingCenter": true,
            "code": "berlin_51"
        }
    },
    "time": {
        "start": 1794922633,
        "finish": 1794922633.225447,
        "duration": 0.2254469394683838,
        "processing": 0,
        "date_start": "2026-11-17T16:37:13+02:00",
        "date_finish": "2026-11-17T16:37:13+02:00",
        "operating_reset_at": 1794922860,
        "operating": 0.1375751495361328
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
[`object`](../../data-types.md) | The warehouse after the update. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Warehouse identifier ||
|| **title**
[`string`](../../data-types.md) | Warehouse name. An empty string if the name is not set ||
|| **active**
[`boolean`](../../data-types.md) | Warehouse activity: `true` — the warehouse is active, `false` — inactive ||
|| **address**
[`string`](../../data-types.md) | Warehouse address ||
|| **description**
[`string`](../../data-types.md) | Warehouse description. An empty string if the description is not set ||
|| **gpsN**
[`double`](../../data-types.md) | Warehouse latitude. If the coordinate is not set, `0` is returned. If Bitrix24 retains a value that is not a number, for example, an empty string, `null` is returned ||
|| **gpsS**
[`double`](../../data-types.md) | Warehouse longitude. If the coordinate is not set, `0` is returned. If Bitrix24 retains a value that is not a number, for example, an empty string, `null` is returned ||
|| **imageId**
[`integer`](../../data-types.md) | Identifier of the warehouse image file. `0` if there is no image ||
|| **imageUrl**
[`string`](../../data-types.md) | Link to the warehouse image. An empty string if there is no image ||
|| **dateModify**
[`datetime`](../../data-types.md) | Date the warehouse was last modified ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date the warehouse was created. Can be `null` for warehouses that Bitrix24 created itself ||
|| **userId**
[`integer`](../../data-types.md) | Identifier of the user who created the warehouse. `0` for warehouses that Bitrix24 created itself ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the warehouse ||
|| **phone**
[`string`](../../data-types.md) | Warehouse phone number. An empty string if the phone number is not set ||
|| **schedule**
[`string`](../../data-types.md) | Warehouse working hours. An empty string if the working hours are not set ||
|| **xmlId**
[`string`](../../data-types.md) | Warehouse external ID. An empty string if the ID is not set ||
|| **sort**
[`integer`](../../data-types.md) | Sort order of the warehouse in lists ||
|| **email**
[`string`](../../data-types.md) | Warehouse email. An empty string if the address is not set ||
|| **issuingCenter**
[`boolean`](../../data-types.md) | Pickup point flag: `true` — the warehouse operates as a pickup point, `false` — a regular warehouse ||
|| **code**
[`string`](../../data-types.md) | Warehouse symbolic code. An empty string if the code is not set ||
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
                "message": "At least one field to update must be provided",
                "field": "fields"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the warehouse `id` ||
|| `-` | Only one of the following values must be provided: id, filter | Pass only `id`: the method does not accept `filter` ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object with the fields to update ||
|| `fields` | At least one field to update must be provided | Pass at least one field in `fields` ||
|| `-` | The warehouse address is not specified | Do not pass an empty string in `address` ||
|| `active`, `issuingCenter` | This request requires data type `bool` in field `#FIELD#` | Pass `true` or `false`, not `Y`, `N`, or `null` ||
|| `gpsN`, `gpsS` | This request requires data type `float` in field `#FIELD#` | Pass the coordinate as a number ||
|| `sort` | This request requires data type `int` in field `sort` | Pass the sort order as an integer ||
|#

#### Field Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id`, `imageId`, `imageUrl`, `userId`, `modifiedBy`, `dateCreate`, `dateModify` | Field "#FIELD#" is read-only | Remove the field from `fields`: the value is filled in automatically. Pass the image in the `image` field ||
|| `image.upload` | Only one of the following values must be provided: data, url | Pass either `data` or `url` in `image.upload` ||
|#

#### File Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_INVALIDFILEEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `FILE_TYPE_NOT_ALLOWED` | File of invalid type. | Upload an image and specify an image extension in `name`, for example, `store.png` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Unknown field `#FIELD#` found in entity `StoreDto` | Pass only the fields from the [fields parameter table](#fields) in `fields` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | A warehouse with this `id` does not exist or has been deleted. Check the identifier using the [catalog.store.list](./catalog-store-list-rest-v3.md) method ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Possible reasons:

- the user does not have the "Edit warehouses" access permission
- the user does not have the "View and select warehouses" access permission for this warehouse, and the warehouse was created by another user ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-store-add-rest-v3.md)
- [{#T}](./catalog-store-get-rest-v3.md)
- [{#T}](./catalog-store-list-rest-v3.md)
- [{#T}](./catalog-store-delete-rest-v3.md)
- [{#T}](./catalog-store-field-list.md)
