# Add Service catalog.product.service.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create product" access permission, one of the "View product catalog", "View catalog products", or "Edit selling price" access permissions, and the permission to edit elements of the catalog information block

The `catalog.product.service.add` method adds a service to the trade catalog.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values for creating the service ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.id`](../../data-types.md#catalog_catalog) | Identifier of the trade catalog information block.

You can retrieve the identifier using the [catalog.catalog.list](../../catalog/catalog-catalog-list.md) method. The variations information block is not suitable: the method returns the `productType is not allowed for this catalog` error ||
|| **name***
[`string`](../../../data-types.md) | Service name ||
|| **active**
[`string`](../../../data-types.md) | Service activity. Possible values:
- `Y` — active
- `N` — inactive

The method saves any other value as `N`.

Default is `Y` ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code. The method does not check that the code is unique ||
|| **xmlId**
[`string`](../../../data-types.md) | External code. Use it to link the service to a record in an external system.

If it is not passed, the method writes the service identifier to the field ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order.

Default is `500` ||
|| **createdBy**
[`user.id`](../../../data-types.md) | Identifier of the user who created the service. The user must exist.

If it is not passed, the method writes the identifier of the current user ||
|| **modifiedBy**
[`user.id`](../../../data-types.md) | Identifier of the user who last modified the service. The user must exist.

If it is not passed, the method writes the identifier of the current user ||
|| **dateActiveFrom**
[`datetime`](../../../data-types.md) | Activity start date. Currently the date cannot be set: a string in any format causes the "Incorrect starting date format" error, and a number is ignored. `null` is accepted ||
|| **dateActiveTo**
[`datetime`](../../../data-types.md) | Activity end date. Currently the date cannot be set: a string in any format causes the "Incorrect expiration date format" error, and a number is ignored. `null` is accepted ||
|| **iblockSectionId**
[`catalog_section.id`](../../data-types.md#catalog_section) | Identifier of the main catalog section.

You can retrieve the identifier using the [catalog.section.list](../../section/catalog-section-list.md) method. For a nonexistent section, the method returns the `200040300050` access error ||
|| **measure**
[`catalog_measure.id`](../../data-types.md#catalog_measure) | Identifier of the unit of measurement.

You can retrieve the identifier using the [catalog.measure.list](../../measure/catalog-measure-list.md) method. If it is not passed, `measure` is `null` in the response. The method does not check that the unit of measurement exists: a nonexistent identifier is saved without an error ||
|| **previewText**
[`string`](../../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Type of the preview description. Possible values:
- `text` — text
- `html` — HTML

Default is `text` ||
|| **detailText**
[`string`](../../../data-types.md) | Detailed description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Type of the detailed description. Possible values:
- `text` — text
- `html` — HTML

Default is `text` ||
|| **previewPicture**
[`object`](../../../data-types.md) | Preview picture. An object in the `{"fileData": ["file_name.png", "base64 content"]}` format ||
|| **detailPicture**
[`object`](../../../data-types.md) | Detailed picture. An object in the `{"fileData": ["file_name.png", "base64 content"]}` format ||
|| **vatId**
[`catalog_vat.id`](../../data-types.md#catalog_vat) | Identifier of the VAT rate. The rate must exist.

You can retrieve the identifier using the [catalog.vat.list](../../vat/catalog-vat-list.md) method ||
|| **vatIncluded**
[`string`](../../../data-types.md) | Whether VAT is included in the price. Possible values:
- `Y` — yes
- `N` — no

Default is `N` ||
|| **propertyN**
[`any`](../../../data-types.md) | Value of the service property, where `N` is the property identifier, for example `property301`.

You can pass the value as a string, a number, or an object `{"value": "value"}`. For a multiple property, pass an array of values or `{"value": "value"}` objects. An array for a single property causes the `Wrong format value of field` error.

You can retrieve property identifiers using the [catalog.product.service.getFieldsByFilter](./catalog-product-service-get-fields-by-filter.md) method ||
|#

The method fills in the `id`, `type`, `available`, `bundle`, `dateCreate`, and `timestampX` fields itself: `type` is always `7` for a service, and the dates are the creation time. The method skips passed values of these fields and unknown fields without an error. The exception is `dateCreate`: currently a passed date is saved incorrectly, for example `2026-11-01T10:00:00+01:00` is saved as `2001-11-30T00:00:00+01:00`, so do not pass this field.

{% note warning "Service Price" %}

To set the service price, use the [catalog.price.*](../../price/index.md) methods.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Air conditioner installation","code":"ac_installation","xmlId":"srv-ac-01","sort":200,"vatId":9,"vatIncluded":"Y","measure":9,"previewText":"Installation of a split system up to 3.5 kW","detailText":"<p>Installation of the indoor and outdoor units</p>","detailTextType":"html","previewPicture":{"fileData":["ac.png","iVBORw0KGgoAAAANSUhEUgAAAAgAAAAICAIAAABLbSncAAAAEUlEQVR4nGPQqDiBFTEMLQkAAtVaAcSRe60AAAAASUVORK5CYII="]},"property301":"SRV-AC-01","property303":"Klima Service GmbH"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Air conditioner installation","code":"ac_installation","xmlId":"srv-ac-01","sort":200,"vatId":9,"vatIncluded":"Y","measure":9,"previewText":"Installation of a split system up to 3.5 kW","detailText":"<p>Installation of the indoor and outdoor units</p>","detailTextType":"html","previewPicture":{"fileData":["ac.png","iVBORw0KGgoAAAANSUhEUgAAAAgAAAAICAIAAABLbSncAAAAEUlEQVR4nGPQqDiBFTEMLQkAAtVaAcSRe60AAAAASUVORK5CYII="]},"property301":"SRV-AC-01","property303":"Klima Service GmbH"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.service.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PictureInfo = { id: string; url: string; urlMachine: string }
    type PropertyValue = { value: string; valueId: string }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ServiceAddResult = {
      service: {
        id: number
        iblockId: number
        name: string
        active: string
        code: string | null
        xmlId: string
        sort: number
        type: number
        measure: number | null
        vatId: number | null
        vatIncluded: string
        previewText: string | null
        previewPicture: PictureInfo | null
        detailPicture: PictureInfo | null
        dateCreate: ISODate
        timestampX: ISODate
        // Properties come as propertyN: null, a string, an object or an array of objects
        [key: `property${number}`]: PropertyValue | PropertyValue[] | string | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ServiceAddResult>({
        method: 'catalog.product.service.add',
        params: {
          fields: {
            iblockId: 25,
            name: 'Air conditioner installation',
            code: 'ac_installation',
            xmlId: 'srv-ac-01',
            sort: 200,
            vatId: 9,
            vatIncluded: 'Y',
            measure: 9,
            previewText: 'Installation of a split system up to 3.5 kW',
            detailText: '<p>Installation of the indoor and outdoor units</p>',
            detailTextType: 'html',
            previewPicture: {
              fileData: ['ac.png', 'iVBORw0KGgoAAAANSUhEUgAAAAgAAAAICAIAAABLbSncAAAAEUlEQVR4nGPQqDiBFTEMLQkAAtVaAcSRe60AAAAASUVORK5CYII='],
            },
            property301: 'SRV-AC-01',
            property303: 'Klima Service GmbH',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added service:', result.service.id, result.service.name)
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
      async function addService() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.service.add',
            params: {
              fields: {
                iblockId: 25,
                name: 'Air conditioner installation',
                code: 'ac_installation',
                xmlId: 'srv-ac-01',
                sort: 200,
                vatId: 9,
                vatIncluded: 'Y',
                measure: 9,
                previewText: 'Installation of a split system up to 3.5 kW',
                detailText: '<p>Installation of the indoor and outdoor units</p>',
                detailTextType: 'html',
                previewPicture: {
                  fileData: ['ac.png', 'iVBORw0KGgoAAAANSUhEUgAAAAgAAAAICAIAAABLbSncAAAAEUlEQVR4nGPQqDiBFTEMLQkAAtVaAcSRe60AAAAASUVORK5CYII='],
                },
                property301: 'SRV-AC-01',
                property303: 'Klima Service GmbH',
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
          console.info('Added service:', result.service.id, result.service.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addService)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.service.add(
            fields={
                "iblockId": 25,
                "name": "Air conditioner installation",
                "code": "ac_installation",
                "xmlId": "srv-ac-01",
                "sort": 200,
                "vatId": 9,
                "vatIncluded": "Y",
                "measure": 9,
                "previewText": "Installation of a split system up to 3.5 kW",
                "detailText": "<p>Installation of the indoor and outdoor units</p>",
                "detailTextType": "html",
                "previewPicture": {
                    "fileData": ["ac.png", "iVBORw0KGgoAAAANSUhEUgAAAAgAAAAICAIAAABLbSncAAAAEUlEQVR4nGPQqDiBFTEMLQkAAtVaAcSRe60AAAAASUVORK5CYII="],
                },
                "property301": "SRV-AC-01",
                "property303": "Klima Service GmbH",
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
                'catalog.product.service.add',
                [
                    'fields' => [
                        'iblockId'        => 25,
                        'name'            => 'Air conditioner installation',
                        'code'            => 'ac_installation',
                        'xmlId'           => 'srv-ac-01',
                        'sort'            => 200,
                        'vatId'           => 9,
                        'vatIncluded'     => 'Y',
                        'measure'         => 9,
                        'previewText'     => 'Installation of a split system up to 3.5 kW',
                        'detailText'      => '<p>Installation of the indoor and outdoor units</p>',
                        'detailTextType'  => 'html',
                        'previewPicture'  => [
                            'fileData' => ['ac.png', 'iVBORw0KGgoAAAANSUhEUgAAAAgAAAAICAIAAABLbSncAAAAEUlEQVR4nGPQqDiBFTEMLQkAAtVaAcSRe60AAAAASUVORK5CYII='],
                        ],
                        'property301'     => 'SRV-AC-01',
                        'property303'     => 'Klima Service GmbH',
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
        'catalog.product.service.add',
        {
            fields: {
                iblockId: 25,
                name: 'Air conditioner installation',
                code: 'ac_installation',
                xmlId: 'srv-ac-01',
                sort: 200,
                vatId: 9,
                vatIncluded: 'Y',
                measure: 9,
                previewText: 'Installation of a split system up to 3.5 kW',
                detailText: '<p>Installation of the indoor and outdoor units</p>',
                detailTextType: 'html',
                previewPicture: {
                    fileData: ['ac.png', 'iVBORw0KGgoAAAANSUhEUgAAAAgAAAAICAIAAABLbSncAAAAEUlEQVR4nGPQqDiBFTEMLQkAAtVaAcSRe60AAAAASUVORK5CYII='],
                },
                property301: 'SRV-AC-01',
                property303: 'Klima Service GmbH',
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
        'catalog.product.service.add',
        [
            'fields' => [
                'iblockId' => 25,
                'name' => 'Air conditioner installation',
                'code' => 'ac_installation',
                'xmlId' => 'srv-ac-01',
                'sort' => 200,
                'vatId' => 9,
                'vatIncluded' => 'Y',
                'measure' => 9,
                'previewText' => 'Installation of a split system up to 3.5 kW',
                'detailText' => '<p>Installation of the indoor and outdoor units</p>',
                'detailTextType' => 'html',
                'previewPicture' => [
                    'fileData' => ['ac.png', 'iVBORw0KGgoAAAANSUhEUgAAAAgAAAAICAIAAABLbSncAAAAEUlEQVR4nGPQqDiBFTEMLQkAAtVaAcSRe60AAAAASUVORK5CYII='],
                ],
                'property301' => 'SRV-AC-01',
                'property303' => 'Klima Service GmbH',
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
    res, err := client.Core().Call(ctx, "catalog.product.service.add", b24.Params{
    	"fields": b24.Params{
    		"iblockId":       25,
    		"name":           "Air conditioner installation",
    		"code":           "ac_installation",
    		"xmlId":          "srv-ac-01",
    		"sort":           200,
    		"vatId":          9,
    		"vatIncluded":    "Y",
    		"measure":        9,
    		"previewText":    "Installation of a split system up to 3.5 kW",
    		"detailText":     "<p>Installation of the indoor and outdoor units</p>",
    		"detailTextType": "html",
    		"previewPicture": b24.Params{
    			"fileData": []string{"ac.png", "iVBORw0KGgoAAAANSUhEUgAAAAgAAAAICAIAAABLbSncAAAAEUlEQVR4nGPQqDiBFTEMLQkAAtVaAcSRe60AAAAASUVORK5CYII="},
    		},
    		"property301": "SRV-AC-01",
    		"property303": "Klima Service GmbH",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.service.add: %w", err)
    }

    // The method wraps the response in an object with the "service" key.
    raw, ok := b24.Unwrap(res.Result, "service")
    if !ok {
    	return fmt.Errorf("no service key in the response")
    }

    // Optional fields can be null, so pointers are used for them.
    var item struct {
    	ID          b24.ID  `json:"id"`
    	IblockID    int     `json:"iblockId"`
    	Name        string  `json:"name"`
    	Code        *string `json:"code"`
    	XMLID       string  `json:"xmlId"`
    	Sort        int     `json:"sort"`
    	Type        int     `json:"type"`
    	Measure     *int    `json:"measure"`
    	VatID       *int    `json:"vatId"`
    	VatIncluded string  `json:"vatIncluded"`
    	PreviewPicture *struct {
    		ID  string `json:"id"`
    		URL string `json:"url"`
    	} `json:"previewPicture"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "service": {
            "active": "Y",
            "available": "N",
            "bundle": "N",
            "code": "ac_installation",
            "createdBy": 1269,
            "dateActiveFrom": null,
            "dateActiveTo": null,
            "dateCreate": "2026-11-05T18:19:49+01:00",
            "detailPicture": null,
            "detailText": "<p>Installation of the indoor and outdoor units</p>",
            "detailTextType": "html",
            "iblockId": 25,
            "iblockSection": null,
            "iblockSectionId": null,
            "id": 7623,
            "measure": 9,
            "modifiedBy": 1269,
            "name": "Air conditioner installation",
            "previewPicture": {
                "id": "40035",
                "url": "/rest/catalog.product.download?fields%5BfieldName%5D=previewPicture&fields%5BfileId%5D=40035&fields%5BproductId%5D=7623",
                "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=previewPicture&fields%5BfileId%5D=40035&fields%5BproductId%5D=7623"
            },
            "previewText": "Installation of a split system up to 3.5 kW",
            "previewTextType": "text",
            "property293": null,
            "property295": "N",
            "property297": "N",
            "property299": "N",
            "property301": {
                "value": "SRV-AC-01",
                "valueId": "4059"
            },
            "property303": {
                "value": "Klima Service GmbH",
                "valueId": "4061"
            },
            "property305": null,
            "property307": null,
            "property309": null,
            "property311": null,
            "property313": null,
            "property315": null,
            "property329": null,
            "property343": null,
            "property359": null,
            "property431": null,
            "property433": null,
            "property435": null,
            "property893": null,
            "property897": null,
            "property901": null,
            "property917": null,
            "property921": null,
            "property931": null,
            "property933": null,
            "property935": null,
            "property1013": null,
            "property1015": null,
            "property1019": null,
            "property1021": null,
            "property1023": null,
            "property1077": null,
            "property1081": null,
            "property1083": null,
            "property1085": null,
            "sort": 200,
            "timestampX": "2026-11-05T18:19:49+01:00",
            "type": 7,
            "vatId": 9,
            "vatIncluded": "Y",
            "xmlId": "srv-ac-01"
        }
    },
    "time": {
        "start": 1793891989,
        "finish": 1793891989.83412,
        "duration": 0.8341200351715088,
        "processing": 0,
        "date_start": "2026-11-05T18:19:49+01:00",
        "date_finish": "2026-11-05T18:19:49+01:00",
        "operating_reset_at": 1793892589,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **result.service**
[`catalog_product_service`](../../data-types.md#catalog_product_service) | Object with the fields of the created service. The method returns all fields, including empty ones with the `null` value, and all catalog properties in the `propertyN` format.

The value of a filled property is an object `{"value": "...", "valueId": "..."}`, a multiple property is an array of such objects. A picture is an object with the file identifier `id` and the `url` and `urlMachine` download links for the [catalog.product.service.download](./catalog-product-service-download.md) method ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: name"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" access permissions ||
|| `400` | `200040300040` | Access Denied | The user does not have the "Create product" access permission ||
|| `400` | `200040300043` | Access Denied | The user does not have the permission to edit elements of the catalog information block ||
|| `400` | `200040300050` | Access Denied | The `iblockSectionId` section does not exist, or the user does not have the permission to link elements to it ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `0` | Incorrect fields format | `fields` is not an object ||
|| `400` | `0` | Required fields: name | A required field is not provided. The text lists the missing fields ||
|| `400` | `0` | iblock is not catalog | `iblockId` is not provided, is not a number, or the information block is not a trade catalog ||
|| `400` | `0` | productType is not allowed for this catalog | `iblockId` contains a variations information block ||
|| `400` | `0` | VAT vatId is not exists | The VAT rate with the specified `vatId` was not found ||
|| `400` | `0` | User createdBy is not exists | The `createdBy` user was not found ||
|| `400` | `0` | User modifiedBy is not exists | The `modifiedBy` user was not found ||
|| `400` | `0` | Incorrect starting date format.<br> | `dateActiveFrom` contains a string. Currently the date cannot be set ||
|| `400` | `0` | Incorrect expiration date format.<br> | `dateActiveTo` contains a string. Currently the date cannot be set ||
|| `400` | `0` | Wrong format value of field `property301`. | A single property contains an array. The text names the property ||
|| `400` | `0` | Wrong format of field `name`. | The field contains an array or an object instead of a value. The text names the field ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-update.md)
- [{#T}](./catalog-product-service-get.md)
- [{#T}](./catalog-product-service-list.md)
- [{#T}](./catalog-product-service-delete.md)
- [{#T}](./catalog-product-service-download.md)
- [{#T}](./catalog-product-service-get-fields-by-filter.md)