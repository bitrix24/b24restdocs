# Add Product Variation catalog.product.offer.add

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a product variation to the trade catalog.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values for adding the product variation ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.id`](../../data-types.md#catalog_catalog) | Identifier of the information block of the trade catalog for variations.

To obtain existing identifiers of information blocks of trade catalogs, use [catalog.catalog.list](../../catalog/catalog-catalog-list.md). The variation info block has the `productIblockId` field filled.
||
|| **name***
[`string`](../../../data-types.md) | Name of the product variation ||
|| **active**
[`string`](../../../data-types.md) | Activity status. Possible values:
- `Y` — yes
- `N` — no

Defaults to `Y`
||
|| **code**
[`string`](../../../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../../../data-types.md) | External code ||
|| **barcodeMulti**
[`string`](../../../data-types.md) | Unique barcodes for each instance. Possible values:
- `Y` — yes
- `N` — no 

Defaults to `N`
||
|| **canBuyZero**
[`string`](../../../data-types.md) | Is the purchase of the product variation allowed when it is out of stock? Possible values:
- `Y` — yes
- `N` — no 

Defaults to `N`
||
|| **createdBy**
[`user.id`](../../../data-types.md) | Created by ||
|| **modifiedBy**
[`user.id`](../../../data-types.md) | Modified by ||
|| **dateActiveFrom**
[`datetime`](../../../data-types.md) | Start date of activity ||
|| **dateActiveTo**
[`datetime`](../../../data-types.md) | End date of activity ||
|| **dateCreate**
[`datetime`](../../../data-types.md) | Creation date ||
|| **iblockSectionId**
[`catalog_section.id`](../../data-types.md#catalog_section) | Identifier of the main section of the information block ||
|| **IblockSection**
[`array`](../../../data-types.md)| Array with all sections to which the product variation is linked ||
|| **measure**
[`catalog_measure.id`](../../data-types.md#catalog_measure) | Unit of measurement ||
|| **previewText**
[`string`](../../../data-types.md) | Description for the announcement ||
|| **detailText**
[`string`](../../../data-types.md) | Detailed description ||
|| **previewPicture**
[`object`](../../../data-types.md) | Picture for the announcement. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the picture file with extension, `value2` — picture in base64 format.

To delete the picture, use the object in the format `{remove: 'Y'}` ||
|| **detailPicture**
[`object`](../../../data-types.md) | Detailed picture. Object in the format `{fileData: [value1, value2]}`, where `value1` — name of the picture file with extension, `value2` — picture in base64 format.

To delete the picture, use the object in the format `{remove: 'Y'}` ||
|| **previewTextType**
[`string`](../../../data-types.md) | Type of description for the announcement. Possible values:
- `text` — text
- `html` — HTML

Defaults to `text`
||
|| **detailTextType**
[`string`](../../../data-types.md) | Type of detailed description. Possible values:
- `text` — text
- `html` — HTML

Defaults to `text`
||
|| **sort**
[`integer`](../../../data-types.md) | Sorting ||
|| **subscribe**
[`string`](../../../data-types.md) | Subscription permission for the product variation. Possible values:
- `Y` — yes
- `N` — no
- `D` — default

Defaults to `D` 
||
|| **vatId**
[`catalog_vat.id`](../../data-types.md#catalog_vat) | VAT identifier ||
|| **vatIncluded**
[`string`](../../../data-types.md) | Is VAT included in the price? Possible values:
- `Y` — yes
- `N` — no

Defaults to `N` 
||
|| **height**
[`double`](../../../data-types.md) | Height of the product variation ||
|| **length**
[`double`](../../../data-types.md) | Length of the product variation ||
|| **weight**
[`double`](../../../data-types.md) | Weight of the product variation ||
|| **width**
[`double`](../../../data-types.md) | Width of the product variation ||
|| **quantityTrace**
[`string`](../../../data-types.md) | Quantity accounting mode. Possible values:
- `Y` — enabled
- `N` — disabled
- `D` — default

Defaults to `D`
||
|| **purchasingCurrency**
[`string`](../../../data-types.md) | Currency of the purchasing price.

The list of currencies can be obtained using the method [crm.currency.list](../../../crm/currency/crm-currency-list.md).

Not editable when inventory management is enabled 
||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchasing price.

Not editable when inventory management is enabled 
||
|| **quantity**
[`double`](../../../data-types.md) | Quantity.

Not editable when inventory management is enabled 
||
|| **quantityReserved**
[`double`](../../../data-types.md) | Reserved quantity.

Not editable when inventory management is enabled
||
|| **recurSchemeLength**
[`integer`](../../../data-types.md) | Length of the payment period.

Defaults to `0`.

Used only in on-premise version for content sales ||
|| **recurSchemeType**
[`string`](../../../data-types.md) | Time unit of the payment period. Possible values:
- `H` — hour
- `D` — day
- `W` — week
- `M` — month
- `Q` — quarter
- `S` — half-year
- `Y` — year

Defaults to `D`.

Used only in on-premise version for content sales
||
|| **trialPriceId**
[`integer`](../../../data-types.md) | Product for trial payment.

Used only in on-premise version for content sales ||
|| **withoutOrder**
[`string`](../../../data-types.md) | Renewal without placing an order. Possible values:
- `Y` — yes
- `N` — no

Defaults to `N`.

Used only in on-premise version for content sales
||
|| **parentId**
[`integer`](../../../data-types.md) | Identifier of the parent product.

To obtain identifiers of parent products, use [catalog.product.sku.list](../sku/catalog-product-sku-list.md) 
||
|| **propertyN**
[`any`](../../../data-types.md) | Value of the property of the product variation, where `N` — property identifier. There can be multiple properties.

When adding a product variation, the property value can be specified as a string, number, or as an object `{value: value}`. If the property is multiple, an array of values or objects of the form `{value: value}` should be specified ||
|#

{% note warning "Working with Product Price" %}

To specify the price for the variation, use the methods [catalog.price.*](../../price/index.md).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":24,"name":"Product Variation","active":"Y","barcodeMulti":"Y","canBuyZero":"Y","code":"Tovar","createdBy":1,"dateActiveFrom":"2024-05-28T10:00:00","dateActiveTo":"2024-05-29T10:00:00","dateCreate":"2024-05-27T10:00:00","detailPicture":{"fileData":["detailPicture.png","iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElF­TkSuQmCCiVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BM­VEX37ff////58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGVKw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCoc­SfQFGKP3+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA­/q2TwrXZib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt­3qSQtwdJSsku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+2­8tICq4rTqXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQ­EFhV3CCNTph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKr­ihqje7Y9iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guv­ayybW1i3Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWt­JSyP21r+FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0h­Ptw86hMX99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC"]},"detailText":"","detailTextType":"text","height":100,"iblockSectionId":47,"length":100,"measure":5,"modifiedBy":1,"previewPicture":{"fileData":["previewPicture.png","iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElF­TkSuQmCC"]},"previewText":"","previewTextType":"text","purchasingCurrency":"USD","purchasingPrice":1000,"quantity":10,"quantityReserved":1,"quantityTrace":"Y","recurSchemeLength":1,"recurSchemeType":"D","sort":100,"subscribe":"Y","trialPriceId":175,"vatId":1,"vatIncluded":"Y","weight":100,"width":100,"withoutOrder":"Y","xmlId":"","parentId":1275,"property258":"test","property259":["test1","test2"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.offer.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":24,"name":"Product Variation","active":"Y","barcodeMulti":"Y","canBuyZero":"Y","code":"Tovar","createdBy":1,"dateActiveFrom":"2024-05-28T10:00:00","dateActiveTo":"2024-05-29T10:00:00","dateCreate":"2024-05-27T10:00:00","detailPicture":{"fileData":["detailPicture.png","iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElF­TkSuQmCCiVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BM­VEX37ff////58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGVKw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCoc­SfQFGKP3+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA­/q2TwrXZib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt­3qSQtwdJSsku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+2­8tICq4rTqXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQ­EFhV3CCNTph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKr­ihqje7Y9iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guv­ayybW1i3Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWt­JSyP21r+FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0h­Ptw86hMX99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC"]},"detailText":"","detailTextType":"text","height":100,"iblockSectionId":47,"length":100,"measure":5,"modifiedBy":1,"previewPicture":{"fileData":["previewPicture.png","iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC"]},"previewText":"","previewTextType":"text","purchasingCurrency":"USD","purchasingPrice":1000,"quantity":10,"quantityReserved":1,"quantityTrace":"Y","recurSchemeLength":1,"recurSchemeType":"D","sort":100,"subscribe":"Y","trialPriceId":175,"vatId":1,"vatIncluded":"Y","weight":100,"width":100,"withoutOrder":"Y","xmlId":"","parentId":1275,"property258":"test","property259":["test1","test2"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.offer.add
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'catalog.product.offer.add',
            {
                fields: {
                    iblockId: 24,
                    name: 'Product Variation',
                    active: 'Y',
                    barcodeMulti: 'Y',
                    canBuyZero: 'Y',
                    code: 'Tovar',
                    createdBy: 1,
                    dateActiveFrom: '2024-05-28T10:00:00',
                    dateActiveTo: '2024-05-29T10:00:00',
                    dateCreate: '2024-05-27T10:00:00',
                    detailPicture: {
                        fileData: ['detailPicture.png', 'iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElF­TkSuQmCCiVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BM­VEX37ff////58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGVKw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCoc­SfQFGKP3+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA­/q2TwrXZib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt­3qSQtwdJSsku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+2­8tICq4rTqXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQ­EFhV3CCNTph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKr­ihqje7Y9iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guv­ayybW1i3Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWt­JSyP21r+FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0h­Ptw86hMX99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC']
                    },
                    detailText: '',
                    detailTextType: 'text',
                    height: 100,
                    iblockSectionId: 47,
                    length: 100,
                    measure: 5,
                    modifiedBy: 1,
                    previewPicture: {
                        fileData: ['previewPicture.png', 'iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElF­TkSuQmCC']},
                    previewText: '',
                    previewTextType: 'text',
                    purchasingCurrency: 'USD',
                    purchasingPrice: 1000,
                    quantity: 10,
                    quantityReserved: 1,
                    quantityTrace: 'Y',
                    recurSchemeLength: 1,
                    recurSchemeType: 'D',
                    sort: 100,
                    subscribe: 'Y',
                    trialPriceId: 175,
                    vatId: 1,
                    vatIncluded: 'Y',
                    weight: 100,
                    width: 100,
                    withoutOrder: 'Y',
                    xmlId: '',
                    parentId: 1275,
                    property258: 'test',
                    property259: ['test1', 'test2'],
                }
            }
        );
    
        const result = response.getData().result;
        console.log('Created element with ID:', result);
        processResult(result);
    } catch (error) {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.offer.add',
                [
                    'fields' => [
                        'iblockId'            => 24,
                        'name'                => 'Product Variation',
                        'active'              => 'Y',
                        'barcodeMulti'        => 'Y',
                        'canBuyZero'          => 'Y',
                        'code'                => 'Tovar',
                        'createdBy'           => 1,
                        'dateActiveFrom'      => '2024-05-28T10:00:00',
                        'dateActiveTo'        => '2024-05-29T10:00:00',
                        'dateCreate'          => '2024-05-27T10:00:00',
                        'detailPicture'       => [
                            'fileData' => ['detailPicture.png', 'iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElF­TkSuQmCCiVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BM­VEX37ff////58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGVKw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCoc­SfQFGKP3+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA­/q2TwrXZib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt­3qSQtwdJSsku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+2­8tICq4rTqXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQ­EFhV3CCNTph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKr­ihqje7Y9iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guv­ayybW1i3Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWt­JSyP21r+FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0h­Ptw86hMX99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC']},
                        'detailText'          => '',
                        'detailTextType'      => 'text',
                        'height'              => 100,
                        'iblockSectionId'     => 47,
                        'length'              => 100,
                        'measure'             => 5,
                        'modifiedBy'          => 1,
                        'previewPicture'      => [
                            'fileData' => ['previewPicture.png', 'iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElFTkSuQmCC']},
                        'previewText'         => '',
                        'previewTextType'     => 'text',
                        'purchasingCurrency'  => 'USD',
                        'purchasingPrice'     => 1000,
                        'quantity'            => 10,
                        'quantityReserved'    => 1,
                        'quantityTrace'       => 'Y',
                        'recurSchemeLength'   => 1,
                        'recurSchemeType'     => 'D',
                        'sort'                => 100,
                        'subscribe'           => 'Y',
                        'trialPriceId'        => 175,
                        'vatId'               => 1,
                        'vatIncluded'         => 'Y',
                        'weight'              => 100,
                        'width'               => 100,
                        'withoutOrder'        => 'Y',
                        'xmlId'               => '',
                        'parentId'            => 1275,
                        'property258'         => 'test',
                        'property259'         => ['test1', 'test2'],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding product offer: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'catalog.product.offer.add', 
    {
    'fields':{
                iblockId: 24,
                name: 'Product Variation',
                active: 'Y',
                barcodeMulti: 'Y',
                canBuyZero: 'Y',
                code: 'Tovar',
                createdBy: 1,
                dateActiveFrom: '2024-05-28T10:00:00',
                dateActiveTo: '2024-05-29T10:00:00',
                dateCreate: '2024-05-27T10:00:00',
                detailPicture: {
                    'fileData':  ['detailPicture.png','iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElF­TkSuQmCCiVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BM­VEX37ff////58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGVKw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCoc­SfQFGKP3+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA­/q2TwrXZib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt­3qSQtwdJSsku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+2­8tICq4rTqXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQ­EFhV3CCNTph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKr­ihqje7Y9iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guv­ayybW1i3Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWt­JSyP21r+FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0h­Ptw86hMX99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC']},
                detailText: '',
                detailTextType: 'text',
                height: 100,
                iblockSectionId: 47,
                length: 100,
                measure: 5,
                modifiedBy: 1,
                previewPicture: {
                    'fileData':['previewPicture.png', 'iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElFTkSuQmCC']},
                previewText: '',
                previewTextType: 'text',
                purchasingCurrency: 'USD',
                purchasingPrice: 1000,
                quantity: 10,
                quantityReserved: 1,
                quantityTrace: 'Y',
                recurSchemeLength: 1,
                recurSchemeType: 'D',
                sort: 100,
                subscribe: 'Y',
                trialPriceId: 175,
                vatId: 1,
                vatIncluded: 'Y',
                weight: 100,
                width: 100,
                withoutOrder: 'Y',
                xmlId: '',
                parentId: 1275,
                property258: 'test',
                property259: ['test1', 'test2'],
    },
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
        'catalog.product.offer.add',
        [
            'fields' => [
                'iblockId' => 24,
                'name' => 'Product Variation',
                'active' => 'Y',
                'barcodeMulti' => 'Y',
                'canBuyZero' => 'Y',
                'code' => 'Tovar',
                'createdBy' => 1,
                'dateActiveFrom' => '2024-05-28T10:00:00',
                'dateActiveTo' => '2024-05-29T10:00:00',
                'dateCreate' => '2024-05-27T10:00:00',
                'detailPicture' => [
                    'fileData' => ['detailPicture.png', 'iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElF­TkSuQmCCiVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BM­VEX37ff////58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGVKw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCoc­SfQFGKP3+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA­/q2TwrXZib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt­3qSQtwdJSsku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+2­8tICq4rTqXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQ­EFhV3CCNTph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKr­ihqje7Y9iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guv­ayybW1i3Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWt­JSyP21r+FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0h­Ptw86hMX99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC']
                ],
                'detailText' => '',
                'detailTextType' => 'text',
                'height' => 100,
                'iblockSectionId' => 47,
                'length' => 100,
                'measure' => 5,
                'modifiedBy' => 1,
                'previewPicture' => [
                    'fileData' => ['previewPicture.png', 'iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGVKw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCoc­SfQFGKP3+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guv­ayybW1i3Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWt­JSyP21r+FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0h­Ptw86hMX99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC']
                ],
                'previewText' => '',
                'previewTextType' => 'text',
                'purchasingCurrency' => 'USD',
                'purchasingPrice' => 1000,
                'quantity' => 10,
                'quantityReserved' => 1,
                'quantityTrace' => 'Y',
                'recurSchemeLength' => 1,
                'recurSchemeType' => 'D',
                'sort' => 100,
                'subscribe' => 'Y',
                'trialPriceId' => 175,
                'vatId' => 1,
                'vatIncluded' => 'Y',
                'weight' => 100,
                'width' => 100,
                'withoutOrder' => 'Y',
                'xmlId' => '',
                'parentId' => 1275,
                'property258' => 'test',
                'property259' => ['test1', 'test2'],
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "offer": {
            "active": "Y",
            "available": "Y",
            "bundle": "N",
            "canBuyZero": "Y",
            "code": "Tovar",
            "createdBy": 1,
            "dateActiveFrom": "2024-05-28T10:00:00+02:00",
            "dateActiveTo": "2024-05-29T10:00:00+02:00",
            "dateCreate": "2024-05-27T10:00:00+02:00",
            "detailPicture": {
                "id": "6536",
                "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6536\u0026fields%5BproductId%5D=1285",
                "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=detailPicture\u0026fields%5BfileId%5D=6536\u0026fields%5BproductId%5D=1285"
            },
            "detailText": null,
            "detailTextType": "text",
            "height": 100,
            "iblockId": 24,
            "iblockSectionId": null,
            "id": 1285,
            "length": 100,
            "measure": 5,
            "modifiedBy": 1,
            "name": "Product Variation",
            "parentId": {
                "value": "1275",
                "valueId": "9863"
            },
            "previewPicture": {
                "id": "6535",
                "url": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6535\u0026fields%5BproductId%5D=1285",
                "urlMachine": "\/rest\/catalog.product.download?fields%5BfieldName%5D=previewPicture\u0026fields%5BfileId%5D=6535\u0026fields%5BproductId%5D=1285"
            },
            "previewText": null,
            "previewTextType": "text",
            "property261": {
                "value": "test",
                "valueId": "9864"
            },
            "property262": [
                {
                    "value": "test1",
                    "valueId": "9865"
                },
                {
                    "value": "test2",
                    "valueId": "9866"
                }
            ],
            "purchasingCurrency": "USD",
            "purchasingPrice": "1000.000000",
            "quantity": 10,
            "quantityReserved": 1,
            "quantityTrace": "Y",
            "sort": 100,
            "subscribe": "Y",
            "timestampX": "2024-06-17T12:25:48+02:00",
            "type": 4,
            "vatId": 1,
            "vatIncluded": "Y",
            "weight": 100,
            "width": 100,
            "xmlId": "1285"
        }
    },
    "time": {
        "start": 1718623548.268628,
        "finish": 1718623549.235369,
        "duration": 0.9667410850524902,
        "processing": 0.559999942779541,
        "date_start": "2024-06-17T14:25:48+02:00",
        "date_finish": "2024-06-17T14:25:49+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **offer**
[`catalog_product_offer`](../../data-types.md#catalog_product_offer) | Object with information about the added product variation ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300050` | Insufficient rights to bind the information block element to the section
|| 
|| `200040300040` | Insufficient rights to create a product variation
|| 
|| `200040300000` | Information block with the specified identifier does not exist
|| 
|| `200040300043` | Insufficient rights to edit the information block element
|| 
|| `200040300010` | Insufficient rights to read the trade catalog
|| 
|| `100` | Parameter `fields` is not specified or is empty
|| 
|| `0` | Section with the specified identifier does not exist
|| 
|| `0` | VAT rate with the specified identifier does not exist
|| 
|| `0` | The specified purchasing price currency does not exist
|| 
|| `0` | User with the specified identifier who created the product variation does not exist
|| 
|| `0` | User with the specified identifier who modified the product variation does not exist
|| 
|| `0` | Required fields are not provided
|| 
|| `0` | Product variation with the specified symbolic code already exists
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-offer-update.md)
- [{#T}](./catalog-product-offer-get.md)
- [{#T}](./catalog-product-offer-list.md)
- [{#T}](./catalog-product-offer-download.md)
- [{#T}](./catalog-product-offer-delete.md)
- [{#T}](./catalog-product-offer-get-fields-by-filter.md)