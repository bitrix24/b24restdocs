### SHIPMENT

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_order_shipment.id`](/api-reference/sale/data-types.html#sale_order_shipment) | Identifier of the shipment.

If the calculation is based on an unsaved shipment, the parameter value will be `null`.

You can obtain shipment identifiers using the [sale.shipment.list](/api-reference/sale/shipment/sale-shipment-list.html) method ||
|| **DELIVERY_SERVICE**
[`object`](/api-reference/data-types.html) | Information about the selected delivery service, its profile, and settings (detailed description provided [below](#delivery_service)) ||
|| **PRICE**
[`double`](/api-reference/data-types.html) | Total cost of goods for the client in the shipment ||
|| **CURRENCY**
[`crm_currency.CURRENCY`](/api-reference/crm/data-types.html) | Currency code of the cost ||
|| **WEIGHT**
[`double`](/api-reference/data-types.html) | Total weight of goods in the shipment (in grams) ||
|| **PROPERTY_VALUES**
[`object[]`](/api-reference/data-types.html) | Array containing the property values of the shipment (detailed description provided [below](#property_values)) ||
|| **ITEMS**
[`object[]`](/api-reference/data-types.html) | Array containing all the goods included in the shipment (detailed description provided [below](#items)) ||
|| **EXTRA_SERVICE_VALUES**
[`object[]`](/api-reference/data-types.html) | Array containing a list of necessary additional services selected for delivery (detailed description provided [below](#extra_service_values)) ||
|| **RESPONSIBLE_CONTACT**
[`object`](/api-reference/data-types.html) | Information about the contact person responsible for delivery from Bitrix24 (detailed description provided [below](#responsible_contact)) ||
|| **RECIPIENT_CONTACT**
[`object`](/api-reference/data-types.html) | Information about the recipient's contact person (detailed description provided [below](#recipient_contact)) ||
|#

### DELIVERY_SERVICE

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_delivery_service.ID`](/api-reference/sale/data-types.html#sale_delivery_service) | Identifier of the delivery service ||
|| **CONFIG**
[`object[]`](/api-reference/data-types.html) | Values of the delivery service settings (detailed description provided [below](#config)) ||
|| **PARENT**
[`object`](/api-reference/data-types.html) | Information about the parent delivery service (detailed description provided [below](#parent)) ||
|#

### PARENT

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_delivery_service.ID`](/api-reference/sale/data-types.html#sale_delivery_service) | Identifier of the parent delivery service ||
|| **CONFIG**
[`object[]`](/api-reference/data-types.html) | Values of the parent delivery service settings (detailed description provided [below](#config)) ||
|#

### CONFIG

#|
|| **Name**
`type` | **Description** ||
|| **CODE**
[`string`](/api-reference/data-types.html) | Symbolic code of the setting ||
|| **VALUE**
[`any`](/api-reference/data-types.html) | Value of the setting ||
|#

### PROPERTY_VALUES

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_shipment_property.id`](/api-reference/sale/data-types.html#sale_shipment_property) | Identifier of the shipment property.

You can obtain the identifier of shipment properties using the [sale.shipmentproperty.list](/api-reference/sale/shipment-property/sale-shipment-property-list.html) method
||
|| **TYPE**
[`string`](/api-reference/data-types.html) | Type of the property. Possible values:

- `STRING` — string
- `Y`/`N` — yes or no
- `NUMBER` — number
- `ENUM` — list
- `FILE` — file
- `DATE` — date
- `LOCATION` — location
- `ADDRESS` — address
 ||
|| **VALUE**
[`string`](/api-reference/data-types.html) \| [`object`](/api-reference/data-types.html) | Value of the property. For the `object` type, detailed description provided [below](#value) ||
|#

### VALUE

#|
|| **Name**
`type` | **Description** ||
|| **LATITUDE**
[`double`](/api-reference/data-types.html) | Geographic latitude ||
|| **LONGITUDE**
[`double`](/api-reference/data-types.html) | Geographic longitude ||
|| **FIELDS**
[`object`](/api-reference/data-types.html) | Detailed information about the delivery address (detailed description provided [below](#fields)) ||
|#

### FIELDS

#|
|| **Name**
`type` | **Description** ||
|| **POSTAL_CODE**
[`string`](/api-reference/data-types.html) | Postal code ||
|| **COUNTRY**
[`string`](/api-reference/data-types.html) | Country ||
|| **ADM_LEVEL_1**
[`string`](/api-reference/data-types.html) | First-level administrative division unit (e.g., state or region) ||
|| **ADM_LEVEL_2**
[`string`](/api-reference/data-types.html) | Second-level administrative division unit (e.g., district) ||
|| **LOCALITY**
[`string`](/api-reference/data-types.html) | Locality ||
|| **STREET**
[`string`](/api-reference/data-types.html) | Street ||
|| **BUILDING**
[`string`](/api-reference/data-types.html) | Building, house number ||
|| **ADDRESS_LINE_1**
[`string`](/api-reference/data-types.html) | Address (street, building, house number) ||
|#

### ITEMS

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](/api-reference/data-types.html) | Name of the product ||
|| **PRICE**
[`double`](/api-reference/data-types.html) | Price of a single item of the product ||
|| **CURRENCY**
[`crm_currency.CURRENCY`](/api-reference/crm/data-types.html) | Currency code of the price ||
|| **WEIGHT**
[`double`](/api-reference/data-types.html) | Weight of a single item of the product ||
|| **QUANTITY**
[`double`](/api-reference/data-types.html) | Quantity of product units ||
|| **DIMENSIONS**
[`object`](/api-reference/data-types.html) | Dimensions of the cargo (detailed description provided [below](#dimensions)) ||
|#

### DIMENSIONS

#|
|| **Name**
`type` | **Description** ||
|| **LENGTH**
[`double`](/api-reference/data-types.html) | Length of the product (mm) ||
|| **WIDTH**
[`double`](/api-reference/data-types.html) | Width of the product (mm) ||
|| **HEIGHT**
[`double`](/api-reference/data-types.html) | Height of the product (mm) ||
|#

### EXTRA_SERVICE_VALUES

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_delivery_extra_service.ID`](/api-reference/sale/data-types.html#sale_delivery_extra_service) | Identifier of the service.

You can obtain the identifiers of delivery service extra services using the [sale.delivery.extra.service.get](/api-reference/sale/delivery/extra-service/sale-delivery-extra-service-get.html) method ||
|| **CODE**
[`string`](/api-reference/data-types.html) | Symbolic code of the additional service ||
|| **VALUE**
[`string` \| `double`](/api-reference/data-types.html) | Value.

Depending on the type ([sale_delivery_extra_service.TYPE](/api-reference/sale/data-types.html#sale_delivery_extra_service)) of the additional service, the value is formed differently:

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
[`string`](/api-reference/data-types.html) | Full name of the contact ||
|| **PHONES**
[`object[]`](/api-reference/data-types.html) | Array containing information about the contact's phone numbers (detailed description provided [below](#phones)) ||
|#

### RECIPIENT_CONTACT

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](/api-reference/data-types.html) | Full name of the contact ||
|| **PHONES**
[`object[]`](/api-reference/data-types.html) | Array containing information about the contact's phone numbers (detailed description provided [below](#phones)) ||
|#

### PHONES

#|
|| **Name**
`type` | **Description** ||
|| **TYPE**
[`string`](/api-reference/data-types.html) | Type of phone. Possible values:

- `WORK` — work
- `MOBILE` — mobile
- `HOME` — home
- `FAX` — fax
- `PAGER` — pager
 ||
|| **VALUE**
[`string`](/api-reference/data-types.html) | Phone number ||
|#
