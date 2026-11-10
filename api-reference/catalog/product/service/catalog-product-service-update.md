# Update Service Fields catalog.product.service.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit product" access permission, one of the "View product catalog", "View catalog products", or "Edit selling price" access permissions, and the permission to edit elements of the catalog information block

The `catalog.product.service.update` method updates the service fields.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_service.id`](../../data-types.md#catalog_product_service) | Identifier of the service. You can retrieve the identifier using the [catalog.product.service.list](./catalog-product-service-list.md) method.

The method does not check the element type: if you pass the identifier of a product, the method updates its fields and returns `service: []` ||
|| **fields***
[`object`](../../../data-types.md) | Object with the service fields to update [(detailed description)](#fields).

If you pass an empty object, the method returns the service unchanged ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Service name. An empty value causes the "Name is not specified" error ||
|| **active**
[`string`](../../../data-types.md) | Service activity.

Possible values:

- `Y` — active
- `N` — inactive

Other values are saved as `N` ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../../../data-types.md) | External code ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order ||
|| **createdBy**
[`user.id`](../../../data-types.md) | Identifier of the user who created the service. You can retrieve the identifier using the [user.get](../../../user/user-get.md) method ||
|| **modifiedBy**
[`user.id`](../../../data-types.md) | Identifier of the user who modified the service ||
|| **dateActiveFrom**
[`datetime`](../../../data-types.md) | Activity start date. Currently the date cannot be set: a string in any format causes the "Incorrect starting date format" error, and a number is ignored. `null` is accepted ||
|| **dateActiveTo**
[`datetime`](../../../data-types.md) | Activity end date. Currently the date cannot be set: a string in any format causes the "Incorrect expiration date format" error, and a number is ignored. `null` is accepted ||
|| **dateCreate**
[`datetime`](../../../data-types.md) | Creation date. The field is filled in automatically, so you do not need to pass it: currently a passed date is saved incorrectly, for example `2026-11-01T10:00:00+01:00` is saved as `2001-11-30T00:00:00+01:00` ||
|| **iblockSectionId**
[`catalog_section.id`](../../data-types.md#catalog_section) | Identifier of the main section. You can retrieve the identifier using the [catalog.section.list](../../section/catalog-section-list.md) method.

For a nonexistent section, the method returns the `200040300050` error. To unlink the service from sections, pass `0` ||
|| **iblockSection**
[`catalog_section.id[]`](../../data-types.md#catalog_section) | Array of identifiers of the sections to link the service to, for example `[41, 39]`. Set the main section in `iblockSectionId`.

The array replaces the current section links ||
|| **measure**
[`catalog_measure.id`](../../data-types.md#catalog_measure) | Identifier of the unit of measurement. You can retrieve the identifier using the [catalog.measure.list](../../measure/catalog-measure-list.md) method.

The method does not check that the unit of measurement exists: a nonexistent identifier is saved without an error ||
|| **previewText**
[`string`](../../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Type of the preview description.

Possible values:

- `text` — text
- `html` — HTML ||
|| **previewPicture**
[`object`](../../../data-types.md) | Preview picture. An object in the format `{"fileData": ["photo.png", "base64"]}`: the first element is the file name with the extension, the second is the file content in [Base64](../../../files/how-to-upload-files.md).

To delete the picture, pass `{"remove": "Y"}` ||
|| **detailText**
[`string`](../../../data-types.md) | Detailed description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Type of the detailed description.

Possible values:

- `text` — text
- `html` — HTML ||
|| **detailPicture**
[`object`](../../../data-types.md) | Detailed picture. An object in the format `{"fileData": ["photo.png", "base64"]}`: the first element is the file name with the extension, the second is the file content in [Base64](../../../files/how-to-upload-files.md).

To delete the picture, pass `{"remove": "Y"}` ||
|| **vatId**
[`catalog_vat.id`](../../data-types.md#catalog_vat) | Identifier of the VAT rate. You can retrieve the identifier using the [catalog.vat.list](../../vat/catalog-vat-list.md) method.

The method does not check that the rate exists: a nonexistent identifier is saved without an error ||
|| **vatIncluded**
[`string`](../../../data-types.md) | Whether VAT is included in the price.

Possible values:

- `Y` — yes
- `N` — no ||
|| **propertyN**
[`object`\|`array`](../../../data-types.md) | Value of the service property, where `N` is the property identifier. There can be multiple properties.

The value of a single property is an object `{"valueId": 4159, "value": "SRV-AC-03"}`, the value of a multiple property is an array of such objects. `valueId` is the identifier of the property value, you can retrieve it using the [catalog.product.service.get](./catalog-product-service-get.md) method.

If `valueId` is not specified, the existing value is deleted and replaced with the new one from `value`. For a multiple property, all values for which `valueId` is not passed are deleted. The exception is properties of the "File" type: values not passed in the request are retained, but their `valueId` change ||
|#

{% note warning "Working with Service Prices" %}

To change the service price, use the [catalog.price.*](../../price/index.md) methods.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7697,"fields":{"name":"Air conditioner installation with an on-site visit","previewText":"Split system installation with a technician visit","detailPicture":{"fileData":["photo.png","iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="]},"property301":{"valueId":4159,"value":"SRV-AC-03"},"property305":[{"valueId":4161,"value":"Technician visit"},{"value":"Removal of the old unit"}]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7697,"fields":{"name":"Air conditioner installation with an on-site visit","previewText":"Split system installation with a technician visit","detailPicture":{"fileData":["photo.png","iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="]},"property301":{"valueId":4159,"value":"SRV-AC-03"},"property305":[{"valueId":4161,"value":"Technician visit"},{"value":"Removal of the old unit"}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.service.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ServiceResult = {
      service: {
        id: number,
        iblockId: number,
        iblockSectionId: number | null,
        iblockSection: number[] | null,
        name: string,
        code: string | null,
        active: string,
        xmlId: string,
        dateCreate: ISODate,
        timestampX: ISODate,
        previewPicture: { id: string, url: string, urlMachine: string } | null,
        detailPicture: { id: string, url: string, urlMachine: string } | null,
        property301: { value: string, valueId: string } | null,
        property305: Array<{ value: string, valueId: string }> | null,
        [key: string]: unknown,
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ServiceResult>({
        method: 'catalog.product.service.update',
        params: {
          id: 7697,
          fields: {
            name: 'Air conditioner installation with an on-site visit',
            previewText: 'Split system installation with a technician visit',
            detailPicture: {
              fileData: ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
            },
            property301: {
              valueId: 4159,
              value: 'SRV-AC-03',
            },
            property305: [
              {
                valueId: 4161,
                value: 'Technician visit',
              },
              {
                value: 'Removal of the old unit',
              },
            ],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated service id:', result.service.id, 'name:', result.service.name)
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
      async function updateService() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.service.update',
            params: {
              id: 7697,
              fields: {
                name: 'Air conditioner installation with an on-site visit',
                previewText: 'Split system installation with a technician visit',
                detailPicture: {
                  fileData: ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
                },
                property301: {
                  valueId: 4159,
                  value: 'SRV-AC-03',
                },
                property305: [
                  {
                    valueId: 4161,
                    value: 'Technician visit',
                  },
                  {
                    value: 'Removal of the old unit',
                  },
                ],
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
          console.info('Updated service id:', result.service.id, 'name:', result.service.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateService)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.service.update(
            id=7697,
            fields={
                "name": "Air conditioner installation with an on-site visit",
                "previewText": "Split system installation with a technician visit",
                "detailPicture": {
                    "fileData": ["photo.png", "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="],
                },
                "property301": {
                    "valueId": 4159,
                    "value": "SRV-AC-03",
                },
                "property305": [
                    {
                        "valueId": 4161,
                        "value": "Technician visit",
                    },
                    {
                        "value": "Removal of the old unit",
                    },
                ],
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
                'catalog.product.service.update',
                [
                    'id'     => 7697,
                    'fields' => [
                        'name'          => 'Air conditioner installation with an on-site visit',
                        'previewText'   => 'Split system installation with a technician visit',
                        'detailPicture' => [
                            'fileData' => ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
                        ],
                        'property301'   => [
                            'valueId' => 4159,
                            'value'   => 'SRV-AC-03',
                        ],
                        'property305'   => [
                            [
                                'valueId' => 4161,
                                'value'   => 'Technician visit',
                            ],
                            [
                                'value' => 'Removal of the old unit',
                            ],
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating service: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.product.service.update',
        {
            'id': 7697,
            'fields': {
                'name': 'Air conditioner installation with an on-site visit',
                'previewText': 'Split system installation with a technician visit',
                'detailPicture': {
                    'fileData': ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==']
                },
                'property301': {
                    'valueId': 4159,
                    'value': 'SRV-AC-03'
                },
                'property305': [
                    {
                        'valueId': 4161,
                        'value': 'Technician visit'
                    },
                    {
                        'value': 'Removal of the old unit'
                    }
                ]
            }
        },
        function(result)
        {
            if(result.error())
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
        'catalog.product.service.update',
        [
            'id'     => 7697,
            'fields' => [
                'name'          => 'Air conditioner installation with an on-site visit',
                'previewText'   => 'Split system installation with a technician visit',
                'detailPicture' => [
                    'fileData' => ['photo.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=='],
                ],
                'property301'   => [
                    'valueId' => 4159,
                    'value'   => 'SRV-AC-03',
                ],
                'property305'   => [
                    [
                        'valueId' => 4161,
                        'value'   => 'Technician visit',
                    ],
                    [
                        'value' => 'Removal of the old unit',
                    ],
                ],
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
    res, err := client.Core().Call(ctx, "catalog.product.service.update", b24.Params{
    	"id":     7697,
    	"fields": b24.Params{
    		"name":          "Air conditioner installation with an on-site visit",
    		"previewText":   "Split system installation with a technician visit",
    		"detailPicture": b24.Params{
    			"fileData": []string{"photo.png", "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg=="},
    		},
    		"property301":   b24.Params{
    			"valueId": 4159,
    			"value":   "SRV-AC-03",
    		},
    		"property305":   []b24.Params{
    			b24.Params{
    				"valueId": 4161,
    				"value":   "Technician visit",
    			},
    			b24.Params{
    				"value": "Removal of the old unit",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.service.update: %w", err)
    }

    // The method wraps the response in an object with the "service" key.
    raw, ok := b24.Unwrap(res.Result, "service")
    if !ok {
    	return fmt.Errorf("no service key in the response")
    }

    var item struct {
    	ID              int    `json:"id"`
    	IblockID        int    `json:"iblockId"`
    	IblockSectionID int    `json:"iblockSectionId"`
    	Name            string `json:"name"`
    	Code            string `json:"code"`
    	Active          string `json:"active"`
    	XMLID           string `json:"xmlId"`
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
            "code": null,
            "createdBy": 1269,
            "dateActiveFrom": null,
            "dateActiveTo": null,
            "dateCreate": "2026-11-06T14:21:47+01:00",
            "detailPicture": {
                "id": "40065",
                "url": "/rest/catalog.product.download?fields%5BfieldName%5D=detailPicture&fields%5BfileId%5D=40065&fields%5BproductId%5D=7697",
                "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=detailPicture&fields%5BfileId%5D=40065&fields%5BproductId%5D=7697"
            },
            "detailText": null,
            "detailTextType": "text",
            "iblockId": 25,
            "iblockSection": null,
            "iblockSectionId": null,
            "id": 7697,
            "measure": null,
            "modifiedBy": 1269,
            "name": "Air conditioner installation with an on-site visit",
            "previewPicture": null,
            "previewText": "Split system installation with a technician visit",
            "previewTextType": "text",
            "property301": {
                "value": "SRV-AC-03",
                "valueId": "4159"
            },
            "property303": null,
            "property305": [
                {
                    "value": "Technician visit",
                    "valueId": "4161"
                },
                {
                    "value": "Removal of the old unit",
                    "valueId": "4163"
                }
            ],
            "sort": 500,
            "timestampX": "2026-11-06T14:21:59+01:00",
            "type": 7,
            "vatId": null,
            "vatIncluded": "N",
            "xmlId": "7697"
        }
    },
    "time": {
        "start": 1793964118,
        "finish": 1793964119.417712,
        "duration": 1.4177119731903076,
        "processing": 0,
        "date_start": "2026-11-06T14:21:58+01:00",
        "date_finish": "2026-11-06T14:21:59+01:00",
        "operating_reset_at": 1793964660,
        "operating": 0.6042139530181885
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
[`catalog_product_service`](../../data-types.md#catalog_product_service) | Object with the service fields after the update. Empty fields are returned as `null`.

The response contains all information block properties as `propertyN` keys. The example shows only the `property301`, `property303`, and `property305` properties ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"200040300010",
    "error_description":"Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" access permissions ||
|| `400` | `200040300040` | Access Denied | The user does not have the "Edit product" access permission ||
|| `400` | `200040300043` | Access Denied | The user does not have the permission to edit elements of the catalog information block ||
|| `400` | `200040300050` | Access Denied | The `iblockSectionId` section does not exist, or the user does not have the permission to link elements to it ||
|| `400` | Empty value | service does not exist. | There is no service with the specified `id`. The method returns the same error if `id` contains a string that is not a number ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter is not provided ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `100` | Invalid value {abc} to match with parameter {fields}. Should be value of type array. | `fields` is not an object ||
|| `400` | `0` | productType is not allowed for this catalog | `fields.iblockId` contains another information block. A service cannot be moved to another catalog ||
|| `400` | `0` | Incorrect starting date format.<br> | `dateActiveFrom` contains a string. Currently the date cannot be set ||
|| `400` | `0` | Incorrect expiration date format.<br> | `dateActiveTo` contains a string. Currently the date cannot be set ||
|| `400` | `0` | Name is not specified.<br> | `name` contains an empty string ||
|| `400` | `0` | User createdBy is not exists | The `createdBy` user was not found ||
|| `400` | `0` | User modifiedBy is not exists | The `modifiedBy` user was not found ||
|| `400` | `0` | Wrong format of field `name`. | The field contains an array or an object instead of a value. The text names the field ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-add.md)
- [{#T}](./catalog-product-service-get.md)
- [{#T}](./catalog-product-service-list.md)
- [{#T}](./catalog-product-service-delete.md)
- [{#T}](./catalog-product-service-download.md)
- [{#T}](./catalog-product-service-get-fields-by-filter.md)