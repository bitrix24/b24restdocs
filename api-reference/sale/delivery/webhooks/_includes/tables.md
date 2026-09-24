### SHIPMENT

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`sale_order_shipment.id`](/api-reference/sale/data-types.html#sale_order_shipment) | Identifier of the shipment.

If the calculation is based on an unsaved shipment, the parameter value will be `null`.

You can obtain shipment identifiers using the [sale.shipment.list](/api-reference/sale/shipment/sale-shipment-list.html) method ||
|| **DELIVERY_SERVICE**
[`object`](/api-reference/data-types.html) | Information about the selected delivery service, its profile, and settings (detailed description provided [below](#delivery_service)). May be `null` if the delivery service is not found ||
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
|| **EXTRA_SERVICES_VALUES**
[`object[]`](/api-reference/data-types.html) | Array containing a list of necessary additional services selected for delivery (detailed description provided [below](#extra_service_values)) ||
|| **RESPONSIBLE_CONTACT**
[`object`](/api-reference/data-types.html) | Information about the employee responsible for delivery on the Bitrix24 side (detailed description provided [below](#responsible_contact)). May be `null` if the responsible person is not specified or not found ||
|| **RECIPIENT_CONTACT**
[`object`](/api-reference/data-types.html) | Information about the shipment recipient (detailed description provided [below](#recipient_contact)). May be `null` if the recipient contact is unavailable ||
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
[`object`](/api-reference/data-types.html) | Information about the parent delivery service (detailed description provided [below](#parent)). The field is absent if no parent service is specified ||
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
- `ADDRESS` — address
 ||
|| **VALUE**
[`string`](/api-reference/data-types.html) \| [`object`](/api-reference/data-types.html) | Value of the property. For the `object` type, detailed description provided [below](#value). May be `null` if the address value is absent ||
|#

### VALUE

#|
|| **Name**
`type` | **Description** ||
|| **LATITUDE**
[`double`](/api-reference/data-types.html) | Geographic latitude. May be `null` ||
|| **LONGITUDE**
[`double`](/api-reference/data-types.html) | Geographic longitude. May be `null` ||
|| **FIELDS**
[`object`](/api-reference/data-types.html) | Detailed information about the delivery address (detailed description provided [below](#fields)) ||
|#

### FIELDS

The object structure depends on which address components are filled in. Bitrix24 passes the available fields from the following list.

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
|| **ADM_LEVEL_3**
[`string`](/api-reference/data-types.html) | Third-level administrative division unit ||
|| **ADM_LEVEL_4**
[`string`](/api-reference/data-types.html) | Fourth-level administrative division unit ||
|| **LOCALITY**
[`string`](/api-reference/data-types.html) | Locality ||
|| **SUB_LOCALITY**
[`string`](/api-reference/data-types.html) | District or part of a locality ||
|| **SUB_LOCALITY_LEVEL_1**
[`string`](/api-reference/data-types.html) | First-level subdivision of a locality ||
|| **SUB_LOCALITY_LEVEL_2**
[`string`](/api-reference/data-types.html) | Second-level subdivision of a locality ||
|| **STREET**
[`string`](/api-reference/data-types.html) | Street ||
|| **BUILDING**
[`string`](/api-reference/data-types.html) | Building, house number ||
|| **ADDRESS_LINE_1**
[`string`](/api-reference/data-types.html) | Address (street, building, house number) ||
|| **ADDRESS_LINE_2**
[`string`](/api-reference/data-types.html) | Additional address line ||
|| **FLOOR**
[`string`](/api-reference/data-types.html) | Floor ||
|| **ROOM**
[`string`](/api-reference/data-types.html) | Room ||
|| **RECIPIENT_COMPANY**
[`string`](/api-reference/data-types.html) | Recipient company name ||
|| **RECIPIENT**
[`string`](/api-reference/data-types.html) | Recipient name ||
|| **PO_BOX**
[`string`](/api-reference/data-types.html) | Post office box number ||
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
[`double`](/api-reference/data-types.html) | Weight of a single item of the product. May be `null` if the weight is not specified ||
|| **QUANTITY**
[`double`](/api-reference/data-types.html) | Quantity of product units ||
|| **DIMENSIONS**
[`object`](/api-reference/data-types.html) | Dimensions of the cargo (detailed description provided [below](#dimensions)). May be `null` if the dimensions are not fully specified ||
|#

### DIMENSIONS

#|
|| **Name**
`type` | **Description** ||
|| **LENGTH**
[`double`](/api-reference/data-types.html) | Product length in millimeters ||
|| **WIDTH**
[`double`](/api-reference/data-types.html) | Product width in millimeters ||
|| **HEIGHT**
[`double`](/api-reference/data-types.html) | Product height in millimeters ||
|#

### EXTRA_SERVICES_VALUES {#extra_service_values}

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
[`object[]`](/api-reference/data-types.html) | Array of the contact's phone numbers (detailed description provided [below](#phones)) ||
|#

### RECIPIENT_CONTACT

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](/api-reference/data-types.html) | Full name of the contact ||
|| **PHONES**
[`object[]`](/api-reference/data-types.html) | Array of the contact's phone numbers (detailed description provided [below](#phones)). The field is absent if no phone numbers are specified ||
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
