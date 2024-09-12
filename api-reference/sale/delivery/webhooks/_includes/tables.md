### SHIPMENT

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_order_shipment.id`](../../../data-types.md#sale_order_shipment) | Identifier of the shipment.

If the calculation is based on an unsaved shipment, the parameter value will be `null`.

You can obtain shipment identifiers using the [sale.shipment.list](../../../shipment/sale-shipment-list.md) method ||
|| **DELIVERY_SERVICE**
[`object`](../../../../data-types.md) | Information about the selected delivery service, its profile, and settings (detailed description provided [below](#delivery_service)) ||
|| **PRICE**
[`double`](../../../../data-types.md) | Total cost of goods for the client in the shipment ||
|| **CURRENCY**
[`crm_currency.CURRENCY`](../../../../crm/data-types.md) | Currency code of the cost ||
|| **WEIGHT**
[`double`](../../../../data-types.md) | Total weight of goods in the shipment (in grams) ||
|| **PROPERTY_VALUES**
[`object[]`](../../../../data-types.md) | Array containing the property values of the shipment (detailed description provided [below](#property_values)) ||
|| **ITEMS**
[`object[]`](../../../../data-types.md) | Array containing all the goods included in the shipment (detailed description provided [below](#items)) ||
|| **EXTRA_SERVICE_VALUES**
[`object[]`](../../../../data-types.md) | Array containing a list of necessary additional services selected for delivery (detailed description provided [below](#extra_service_values)) ||
|| **RESPONSIBLE_CONTACT**
[`object`](../../../../data-types.md) | Information about the contact person responsible for delivery from Bitrix24 (detailed description provided [below](#responsible_contact)) ||
|| **RECIPIENT_CONTACT**
[`object`](../../../../data-types.md) | Information about the recipient's contact person (detailed description provided [below](#recipient_contact)) ||
|#

### DELIVERY_SERVICE

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_delivery_service.ID`](../../../data-types.md#sale_delivery_service) | Identifier of the delivery service ||
|| **CONFIG**
[`object[]`](../../../../data-types.md) | Values of the delivery service settings (detailed description provided [below](#config)) ||
|| **PARENT**
[`object`](../../../../data-types.md) | Information about the parent delivery service (detailed description provided [below](#parent)) ||
|#

### PARENT

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_delivery_service.ID`](../../../data-types.md#sale_delivery_service) | Identifier of the parent delivery service ||
|| **CONFIG**
[`object[]`](../../../../data-types.md) | Values of the parent delivery service settings (detailed description provided [below](#config)) ||
|#

### CONFIG

#|
|| **Name**
`type` | **Description** ||
|| **CODE**
[`string`](../../../../data-types.md) | Symbolic code of the setting ||
|| **VALUE**
[`any`](../../../../data-types.md) | Value of the setting ||
|#

### PROPERTY_VALUES

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_shipment_property.id`](../../../data-types.md#sale_shipment_property) | Identifier of the shipment property.

You can obtain the identifier of shipment properties using the [sale.shipmentproperty.list](../../../shipment-property/sale-shipment-property-list.md) method 
||
|| **TYPE**
[`string`](../../../../data-types.md) | Type of the property. Possible values:

- `STRING`
- `Y`/`N`
- `NUMBER`
- `ENUM`
- `FILE`
- `DATE`
- `LOCATION`
- `ADDRESS`
 ||
|| **VALUE**
[`string`](../../../../data-types.md) \| [`object`](../../../../data-types.md) | Value of the property. For the `object` type, detailed description provided [below](#value) ||
|#

### VALUE

#|
|| **Name**
`type` | **Description** ||
|| **LATITUDE**
[`double`](../../../../data-types.md) | Geographic latitude ||
|| **LONGITUDE**
[`double`](../../../../data-types.md) | Geographic longitude ||
|| **FIELDS**
[`object`](../../../../data-types.md) | Detailed information about the delivery address (detailed description provided [below](#fields)) ||
|#

### FIELDS

#|
|| **Name**
`type` | **Description** ||
|| **POSTAL_CODE**
[`string`](../../../../data-types.md) | Postal code ||
|| **COUNTRY**
[`string`](../../../../data-types.md) | Country ||
|| **ADM_LEVEL_1**
[`string`](../../../../data-types.md) | First-level administrative division unit (e.g., state or region) ||
|| **ADM_LEVEL_2**
[`string`](../../../../data-types.md) | Second-level administrative division unit (e.g., district) ||
|| **LOCALITY**
[`string`](../../../../data-types.md) | Locality ||
|| **STREET**
[`string`](../../../../data-types.md) | Street ||
|| **BUILDING**
[`string`](../../../../data-types.md) | Building, house number ||
|| **ADDRESS_LINE_1**
[`string`](../../../../data-types.md) | Address (street, building, house number) ||
|#

### ITEMS

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../../../../data-types.md) | Name of the product ||
|| **PRICE**
[`double`](../../../../data-types.md) | Price of a single item of the product ||
|| **CURRENCY**
[`crm_currency.CURRENCY`](../../../../crm/data-types.md) | Currency code of the price ||
|| **WEIGHT**
[`double`](../../../../data-types.md) | Weight of a single item of the product ||
|| **QUANTITY**
[`double`](../../../../data-types.md) | Quantity of product units ||
|| **DIMENSIONS**
[`object`](../../../../data-types.md) | Dimensions of the cargo (detailed description provided [below](#dimensions)) ||
|#

### DIMENSIONS

#|
|| **Name**
`type` | **Description** ||
|| **LENGTH**
[`double`](../../../../data-types.md) | Length of the product (mm) ||
|| **WIDTH**
[`double`](../../../../data-types.md) | Width of the product (mm) ||
|| **HEIGHT**
[`double`](../../../../data-types.md) | Height of the product (mm) ||
|#

### EXTRA_SERVICE_VALUES

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_delivery_extra_service.ID`](../../../data-types.md#sale_delivery_extra_service) | Identifier of the service.

You can obtain the identifiers of delivery service extra services using the [sale.delivery.extra.service.get](../../extra-service/sale-delivery-extra-service-get.md) method ||
|| **CODE**
[`string`](../../../../data-types.md) | Symbolic code of the additional service ||
|| **VALUE**
[`string` \| `double`](../../../../data-types.md) | Value.

Depending on the type ([sale_delivery_extra_service.TYPE](../../../data-types.md#sale_delivery_extra_service)) of the additional service, the value is formed differently:

- `checkbox` 
  - `Y` — if the service is required
  - `N` — if the service is not required
- `enum` — string containing the symbolic code of the selected value from the service list
- `quantity` — number reflecting the required amount for the additional service ||
|#

### RESPONSIBLE_CONTACT

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../../../../data-types.md) | Full name of the contact ||
|| **PHONES**
[`object[]`](../../../../data-types.md) | Array containing information about the contact's phone numbers (detailed description provided [below](#phones)) ||
|#

### RECIPIENT_CONTACT

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../../../../data-types.md) | Full name of the contact ||
|| **PHONES**
[`object[]`](../../../../data-types.md) | Array containing information about the contact's phone numbers (detailed description provided [below](#phones)) ||
|#

### PHONES

#|
|| **Name**
`type` | **Description** ||
|| **TYPE**
[`string`](../../../../data-types.md) | Type of phone. Possible values:

- `WORK`
- `MOBILE`
- `HOME`
- `FAX`
- `PAGER`
 ||
|| **VALUE**
[`string`](../../../../data-types.md) | Phone number ||
|#