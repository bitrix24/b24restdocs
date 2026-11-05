# Add Warehouse catalog.store.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit warehouses" access permission

The method `catalog.store.add` creates a warehouse.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a warehouse ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **address***
[`string`](../../data-types.md) | Warehouse address. An empty string is not accepted ||
|| **title**
[`string`](../../data-types.md) | Warehouse name.

If not provided, the warehouse is created without a name, and `title` is `null` in the response ||
|| **active**
[`string`](../../data-types.md) | Warehouse activity. Possible values:
- `Y` — the warehouse is active
- `N` — the warehouse is inactive

The method saves any other value as `N`.

Default: `Y` ||
|| **description**
[`string`](../../data-types.md) | Warehouse description ||
|| **gpsN**
[`double`](../../data-types.md) | Warehouse latitude, for example, `52.51463`. You can pass a number or a string containing a number ||
|| **gpsS**
[`double`](../../data-types.md) | Warehouse longitude, for example, `13.35011`. You can pass a number or a string containing a number ||
|| **phone**
[`string`](../../data-types.md) | Warehouse phone number ||
|| **schedule**
[`string`](../../data-types.md) | Warehouse working hours, for example, `Mon-Fri 9:00-20:00` ||
|| **email**
[`string`](../../data-types.md) | Warehouse email. The method does not validate the address format ||
|| **issuingCenter**
[`string`](../../data-types.md) | Whether the warehouse is a pickup point. Possible values:
- `Y` — yes
- `N` — no

The method saves any other value as `N`.

Default: `N` ||
|| **sort**
[`integer`](../../data-types.md) | Sort order of the warehouse in lists.

The method saves a non-numeric value as `null` without an error.

Default: `100` ||
|| **code**
[`string`](../../data-types.md) | Warehouse symbolic code. The method does not check that the code is unique ||
|| **xmlId**
[`string`](../../data-types.md) | Warehouse external code. Use it to link the warehouse to a record in an external system ||
|#

The method fills in the `id`, `userId`, `modifiedBy`, `dateCreate`, and `dateModify` fields itself: `userId` and `modifiedBy` get the ID of the current user, and the dates get the creation time. The method ignores values passed in these fields without an error. It also ignores unknown fields in `fields`.

You cannot set the warehouse image `imageId` with this method: the method rejects an object in `imageId`, for example, `{"fileData": [...]}`, with the `Wrong format of field` error, and ignores a string without an error and without saving it.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"address":"Tiergartenstraße 17, 10785 Berlin, Germany","title":"Tiergarten Warehouse","active":"Y","description":"Main retail warehouse","gpsN":52.51463,"gpsS":13.35011,"phone":"+49 30 2128 2850","schedule":"Mon-Fri 9:00-20:00, Sat-Sun 11:00-18:00","email":"store-kgd@example.com","issuingCenter":"Y","sort":200,"code":"store_kgd","xmlId":"store-kgd-01"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.store.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"address":"Tiergartenstraße 17, 10785 Berlin, Germany","title":"Tiergarten Warehouse","active":"Y","description":"Main retail warehouse","gpsN":52.51463,"gpsS":13.35011,"phone":"+49 30 2128 2850","schedule":"Mon-Fri 9:00-20:00, Sat-Sun 11:00-18:00","email":"store-kgd@example.com","issuingCenter":"Y","sort":200,"code":"store_kgd","xmlId":"store-kgd-01"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.store.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type StoreAddResult = {
      store: {
        active: string
        address: string
        code: string | null
        dateCreate: ISODate
        dateModify: ISODate
        description: string | null
        email: string | null
        gpsN: number | null
        gpsS: number | null
        id: number
        imageId: { id: number; url: string } | null
        issuingCenter: string
        modifiedBy: number
        phone: string | null
        schedule: string | null
        sort: number | null
        title: string | null
        userId: number
        xmlId: string | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<StoreAddResult>({
        method: 'catalog.store.add',
        params: {
          fields: {
            address: 'Tiergartenstraße 17, 10785 Berlin, Germany',
            title: 'Tiergarten Warehouse',
            active: 'Y',
            description: 'Main retail warehouse',
            gpsN: 52.51463,
            gpsS: 13.35011,
            phone: '+49 30 2128 2850',
            schedule: 'Mon-Fri 9:00-20:00, Sat-Sun 11:00-18:00',
            email: 'store-kgd@example.com',
            issuingCenter: 'Y',
            sort: 200,
            code: 'store_kgd',
            xmlId: 'store-kgd-01',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added store:', result.store.id, result.store.title)
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
      async function addStore() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.store.add',
            params: {
              fields: {
                address: 'Tiergartenstraße 17, 10785 Berlin, Germany',
                title: 'Tiergarten Warehouse',
                active: 'Y',
                description: 'Main retail warehouse',
                gpsN: 52.51463,
                gpsS: 13.35011,
                phone: '+49 30 2128 2850',
                schedule: 'Mon-Fri 9:00-20:00, Sat-Sun 11:00-18:00',
                email: 'store-kgd@example.com',
                issuingCenter: 'Y',
                sort: 200,
                code: 'store_kgd',
                xmlId: 'store-kgd-01',
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
          console.info('Added store:', result.store.id, result.store.title)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addStore)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.store.add(
            fields={
                "address": "Tiergartenstraße 17, 10785 Berlin, Germany",
                "title": "Tiergarten Warehouse",
                "active": "Y",
                "description": "Main retail warehouse",
                "gpsN": 52.51463,
                "gpsS": 13.35011,
                "phone": "+49 30 2128 2850",
                "schedule": "Mon-Fri 9:00-20:00, Sat-Sun 11:00-18:00",
                "email": "store-kgd@example.com",
                "issuingCenter": "Y",
                "sort": 200,
                "code": "store_kgd",
                "xmlId": "store-kgd-01",
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.store.add',
                [
                    'fields' => [
                        'address'       => 'Tiergartenstraße 17, 10785 Berlin, Germany',
                        'title'         => 'Tiergarten Warehouse',
                        'active'        => 'Y',
                        'description'   => 'Main retail warehouse',
                        'gpsN'          => 52.51463,
                        'gpsS'          => 13.35011,
                        'phone'         => '+49 30 2128 2850',
                        'schedule'      => 'Mon-Fri 9:00-20:00, Sat-Sun 11:00-18:00',
                        'email'         => 'store-kgd@example.com',
                        'issuingCenter' => 'Y',
                        'sort'          => 200,
                        'code'          => 'store_kgd',
                        'xmlId'         => 'store-kgd-01',
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

    ```js
    BX24.callMethod(
        'catalog.store.add',
        {
            fields: {
                address: 'Tiergartenstraße 17, 10785 Berlin, Germany',
                title: 'Tiergarten Warehouse',
                active: 'Y',
                description: 'Main retail warehouse',
                gpsN: 52.51463,
                gpsS: 13.35011,
                phone: '+49 30 2128 2850',
                schedule: 'Mon-Fri 9:00-20:00, Sat-Sun 11:00-18:00',
                email: 'store-kgd@example.com',
                issuingCenter: 'Y',
                sort: 200,
                code: 'store_kgd',
                xmlId: 'store-kgd-01',
            },
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.store.add',
        [
            'fields' => [
                'address' => 'Tiergartenstraße 17, 10785 Berlin, Germany',
                'title' => 'Tiergarten Warehouse',
                'active' => 'Y',
                'description' => 'Main retail warehouse',
                'gpsN' => 52.51463,
                'gpsS' => 13.35011,
                'phone' => '+49 30 2128 2850',
                'schedule' => 'Mon-Fri 9:00-20:00, Sat-Sun 11:00-18:00',
                'email' => 'store-kgd@example.com',
                'issuingCenter' => 'Y',
                'sort' => 200,
                'code' => 'store_kgd',
                'xmlId' => 'store-kgd-01',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.store.add", b24.Params{
    	"fields": b24.Params{
    		"address":       "Tiergartenstraße 17, 10785 Berlin, Germany",
    		"title":         "Tiergarten Warehouse",
    		"active":        "Y",
    		"description":   "Main retail warehouse",
    		"gpsN":          52.51463,
    		"gpsS":          13.35011,
    		"phone":         "+49 30 2128 2850",
    		"schedule":      "Mon-Fri 9:00-20:00, Sat-Sun 11:00-18:00",
    		"email":         "store-kgd@example.com",
    		"issuingCenter": "Y",
    		"sort":          200,
    		"code":          "store_kgd",
    		"xmlId":         "store-kgd-01",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.store.add: %w", err)
    }

    // The method wraps the response in an object with the "store" key.
    raw, ok := b24.Unwrap(res.Result, "store")
    if !ok {
    	return fmt.Errorf("the response has no store key")
    }

    // Optional fields can be null, so they use pointers.
    var item struct {
    	ID            b24.ID   `json:"id"`
    	Title         *string  `json:"title"`
    	Address       string   `json:"address"`
    	Active        string   `json:"active"`
    	IssuingCenter string   `json:"issuingCenter"`
    	GpsN          *float64 `json:"gpsN"`
    	GpsS          *float64 `json:"gpsS"`
    	Sort          *int     `json:"sort"`
    	Code          *string  `json:"code"`
    	XMLID         *string  `json:"xmlId"`
    	UserID        int      `json:"userId"`
    	DateCreate    string   `json:"dateCreate"`
    	ImageID       *struct {
    		ID  int    `json:"id"`
    		URL string `json:"url"`
    	} `json:"imageId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(item.ID, item.Address)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "store": {
            "active": "Y",
            "address": "Tiergartenstraße 17, 10785 Berlin, Germany",
            "code": "store_kgd",
            "dateCreate": "2026-11-05T13:19:57+03:00",
            "dateModify": "2026-11-05T13:19:57+03:00",
            "description": "Main retail warehouse",
            "email": "store-kgd@example.com",
            "gpsN": 52.51463,
            "gpsS": 13.35011,
            "id": 37,
            "imageId": null,
            "issuingCenter": "Y",
            "modifiedBy": 1269,
            "phone": "+49 30 2128 2850",
            "schedule": "Mon-Fri 9:00-20:00, Sat-Sun 11:00-18:00",
            "sort": 200,
            "title": "Tiergarten Warehouse",
            "userId": 1269,
            "xmlId": "store-kgd-01"
        }
    },
    "time": {
        "start": 1793873997,
        "finish": 1793873997.178257,
        "duration": 0.17825698852539062,
        "processing": 0,
        "date_start": "2026-11-05T13:19:57+03:00",
        "date_finish": "2026-11-05T13:19:57+03:00",
        "operating_reset_at": 1793874540,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.store**
[`catalog_store`](../data-types.md#catalog_store) | An object with the fields of the created warehouse. The method returns all warehouse fields, including empty ones with the `null` value ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: address"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300020` | Access Denied | The user does not have the "Edit warehouses" access permission ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `0` | Required fields: address | The `address` field is missing in `fields`, including when `fields` is empty ||
|| `400` | `0` | The warehouse address is not specified | An empty string is passed in `address` ||
|| `400` | `0` | Wrong format of field `imageId`. | An object or an array is passed in the field instead of a value. The text contains the field name, for example, Wrong format of field `title` for `title`. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-store-update.md)
- [{#T}](./catalog-store-get.md)
- [{#T}](./catalog-store-list.md)
- [{#T}](./catalog-store-delete.md)
- [{#T}](./catalog-store-get-fields.md)
