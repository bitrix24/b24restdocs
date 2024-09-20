# Data Types and Object Structure in the Online Store REST API

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- table sale_order, in the description **clients** needs a link for the method crm.ordercontactcompany.list (the method has not been described yet, only a task has been set for developers to obtain information)

{% endnote %}

{% endif %}

Basic data types are listed in a separate [article](../data-types.md).

In this article, we will discuss the data types and object structure specific to the online store.

## Data Types

#|
|| **Type** | **Descriptions and Values** ||
|| [`sale_order`](#sale_order) | Integer identifier of the order (for example, `1`). Order identifiers can be obtained using the method [sale.order.list](./order/sale-order-list.md) ||
|| [`sale_basket_item`](#sale_basket_item) | Integer identifier of the basket (for example, `1`). Basket identifiers can be obtained using the method [sale.basketItem.list](./basket-item/sale-basket-item-list.md) ||
|| [`sale_order_shipment`](#sale_order_shipment) | Integer identifier of the shipment (for example, `1`). Shipment identifiers can be obtained using the method [sale.shipment.list](./shipment/sale-shipment-list.md) ||
|| [`sale_order_shipment_item`](#sale_order_shipment_item) | Integer identifier of the shipment line item (for example, `1`). Identifiers of shipment line items can be obtained using the method [sale.shipmentitem.list](./shipment-item/sale-shipment-item-list.md) ||
|| [`sale_payment_item_shipment`](#sale_payment_item_shipment) | Integer identifier of the payment binding to the shipment (for example, `1`). Identifiers of payment bindings to shipments can be obtained using the method [sale.paymentitemshipment.list](./payment-item-shipment/sale-payment-item-shipment-list.md) ||
|| [`sale_payment_item_basket`](#sale_payment_item_basket) | Integer identifier of the basket item binding to the payment (for example, `1`). Identifiers of payment bindings to baskets can be obtained using the method [sale.paymentitembasket.list](./payment-item-basket/sale-payment-item-basket-list.md) ||
|| [`sale_status`](#sale_status) | Symbolic identifier of the status (for example, `DN`). Status identifiers can be obtained using the method [sale.status.list](./status/sale-status-list.md) ||
|| [`sale_status_lang`](#sale_status_lang) | Object containing information about the localization of the status. A list of status localization objects can be obtained using the method [sale.statuslang.list](./status-lang/sale-status-lang-list.md) ||
|| [`sale_lang`](#sale_lang) | Symbolic identifier of the language (for example, `de`). Language identifiers can be obtained using the method [sale.statuslang.getlistlangs](./status-lang/sale-status-lang-get-list-langs.md) ||
|| [`sale_person_type`](#sale_person_type) | Integer identifier of the payer type (for example, `1`). Payer type identifiers can be obtained using the method [sale.persontype.list](./person-type/sale-person-type-list.md) ||
|| [`sale_order_property`](#sale_order_property) | Integer identifier of the order property (for example, `1`). Order property identifiers can be obtained using the method [sale.property.list](./property/sale-property-list.md) ||
|| [`sale_shipment_property`](#sale_shipment_property) | Integer identifier of the shipment property (for example, `1`). Shipment property identifiers can be obtained using the method [sale.shipmentproperty.list](./shipment-property/sale-shipment-property-list.md) ||
|| [`sale_shipment_property_value`](#sale_shipment_property_value) | Integer identifier of the shipment property value (for example, `1`). Shipment property value identifiers can be obtained using the method [sale.shipmentpropertyvalue.list](./shipment-property-value/sale-shipment-property-value-list.md) ||
|| [`sale_order_property_group`](#sale_order_property_group) | Integer identifier of the property group (for example, `1`). Property group type identifiers can be obtained using the method [sale.propertygroup.list](./property-group/sale-property-group-list.md) ||
|| [`sale_order_property_value`](#sale_order_property_value) | Integer identifier of the order property value (for example, `1`). Order property value identifiers can be obtained using the method [sale.propertyvalue.list](./property-value/sale-property-value-list.md) ||
|| [`sale_order_property_variant`](#sale_order_property_variant) | Integer identifier of the property value variant (for example, `1`). Property group type identifiers can be obtained using the method [sale.propertyvariant.list](./property-variant/sale-property-variant-list.md) ||
|| [`sale_order_property_relation`](#sale_order_property_relation) | Object containing information about the property binding. A list of property binding objects can be obtained using the method [sale.propertyRelation.list](./property-relation/sale-property-relation-list.md) ||
|| [`sale_order_trade_platform`](#sale_order_trade_platform) | Integer identifier of the trading platform (for example, `1`). Trading platform identifiers can be obtained using the method [sale.tradePlatform.list](./trade-platform/sale-trade-platform-list.md) ||
|| [`sale_order_trade_binding`](#sale_order_trade_binding) | Integer identifier of the binding to the order from an external source (for example, `1`). Identifiers of bindings to orders can be obtained using the method [sale.tradeBinding.list](./trade-binding/sale-trade-binding-list.md) ||
|| [`sale_order_payment`](#sale_order_payment) | Integer identifier of the payment (for example, `1`). Payment identifiers can be obtained using the method [sale.payment.list](./payment/sale-payment-list.md) ||
|| [`sale_business_value_person_domain`](#sale_business_value_person_domain) | Object containing information about the correspondence between the payer type and the individual or legal entity. A list of correspondence objects can be obtained using the method [sale.businessValuePersonDomain.list](./business-value-person-domain/sale-business-value-person-domain-list.md) ||
|| [`sale_delivery_handler`](#sale_delivery_handler) | Delivery service handler object. The delivery service handler is a template from which specific delivery services are created. Identifiers of delivery service handlers can be obtained using the method [sale.delivery.handler.list](./delivery/handler/sale-delivery-handler-list.md) ||
|| [`sale_delivery_service`](#sale_delivery_service) | Delivery service object. Identifiers of delivery services can be obtained using the method [sale.delivery.getlist](./delivery/delivery/sale-delivery-get-list.md) ||
|| [`sale_delivery_extra_service`](#sale_delivery_extra_service) | Additional service object of the delivery service. Identifiers of delivery service extra services can be obtained using the method [sale.delivery.extra.service.get](./delivery/extra-service/sale-delivery-extra-service-get.md) ||
|| [`sale_paysystem_handler`](#sale_paysystem_handler) | Payment system handler object. Identifiers of payment system handlers can be obtained using the method [sale.paysystem.handler.list](../pay-system/sale-pay-system-handler-list.md) ||
|| [`sale_paysystem`](#sale_paysystem) | Payment system object. Identifiers of payment systems can be obtained using the method [sale.paysystem.list](../pay-system/sale-pay-system-list.md) ||
|| [`sale_cashbox_handler`](#sale_cashbox_handler) | Cash register handler object. Identifiers of cash register handlers can be obtained using the method [sale.cashbox.handler.list](./cashbox/sale-cashbox-handler-list.md) ||
|| [`sale_cashbox`](#sale_cashbox) | Cash register object. Identifiers of cash registers can be obtained using the method [sale.cashbox.list](./cashbox/sale-cashbox-list.md) ||
|#

## Object Structure

### sale_order_shipment_item

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Identifier of the shipment line item ||
|| **orderDeliveryId**
[`sale_order_shipment.id`](#sale_order_shipment) | Identifier of the shipment ||
|| **basketId**
[`sale_basket_item.id`](#sale_basket_item) | Identifier of the basket ||
|| **quantity**
[`float`](../data-types.md) | Quantity of the product ||
|| **reservedQuantity**
[`float`](../data-types.md) | Reserved quantity of the product ||
|| **xmlId**
[`string`](../data-types.md) | External identifier.

Can be used to synchronize the current product position of the delivery with a similar position in an external system ||
|| **dateInsert**
[`datetime`](../data-types.md) | Date of adding the shipment line item ||
|#

### sale_payment_item_shipment

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the shipment line item ||
|| **shipmentId**
[`sale_order_shipment.id`](#sale_order_shipment) | Identifier of the shipment ||
|| **paymentId**
[`sale_order_payment.id`](#sale_order_payment) | Identifier of the payment ||
|| **xmlId**
[`string`](../data-types.md) | External identifier of the record ||
|| **dateInsert**
[`datetime`](../data-types.md) | Date of adding the payment binding to the shipment ||
|#

### sale_payment_item_basket

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier linking the basket item to the payment ||
|| **paymentId**
[`sale_order_payment.id`](#sale_order_payment) | Payment identifier ||
|| **basketId**
[`sale_basket_item.id`](#sale_basket_item) | Basket item identifier ||
|| **quantity**
[`double`](../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../data-types.md) | External identifier of the record ||
|| **dateInsert**
[`datetime`](../data-types.md) | Date the basket item was linked to the payment ||
|#

### sale_order_shipment

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Shipment identifier ||
|| **dateInsert**
[`datetime`](../data-types.md) | Date the shipment was created ||
|| **orderId**
[`sale_order.id`](#sale_order) | Order identifier ||
|| **accountNumber**
[`string`](../data-types.md) | System number of the shipment ||
|| **allowDelivery**
[`string`](../data-types.md) | Indicator of delivery permission.

Possible values:
- `Y` — yes (delivery allowed)
- `N` — no (delivery not allowed) ||
|| **dateAllowDelivery**
[`datetime`](../data-types.md) | Date the delivery permission flag was changed ||
|| **empAllowDeliveryId**
[`user.id`](../data-types.md) | User who changed the delivery permission flag value ||
|| **deducted**
 [`string`](../data-types.md) | Indicator of whether the shipment has been shipped.

Possible values:
- `Y` — yes (shipped)
- `N` — no (not shipped) ||
|| **dateDeducted**
[`datetime`](../data-types.md) | Date the shipped flag of the shipment was changed ||
|| **empDeductedId**
[`user.id`](../data-types.md) | User who changed the shipped flag value ||
|| **reasonUndoDeducted** 
[`string`](../data-types.md) | Deprecated property ||
|| **system**
[`string`](../data-types.md) | Indicator of whether the shipment is system-generated.

Value is always `N`. System shipments are not visible via REST and are not intended for direct interaction.

Possible values:
- `Y` — yes
- `N` — no ||
|| **deliveryId**
[`sale_delivery_service.id`](#sale_delivery_service) | Delivery service identifier ||
|| **deliveryName**
[`string`](../data-types.md) | Name of the delivery service ||
|| **deliveryXmlId**
[`string`](../data-types.md) | External identifier of the delivery service ||
|| **statusId** 
[`sale_status.id`](#sale_status) | Delivery status identifier ||
|| **statusXmlId** 
[`string`](../data-types.md) | External identifier of the delivery status ||
|| **canceled** 
[`string`](../data-types.md) | Deprecated. Use the shipment status.

Indicator of whether the shipment is canceled.

Possible values:
- `Y` — yes
- `N` — no ||
|| **dateCanceled**
[`datetime`](../data-types.md) | Deprecated.

Date and time of shipment cancellation ||
|| **empCanceledId** 
[`user.id`](../data-types.md) | Deprecated.

User who changed the canceled flag value of the shipment (`canceled`) ||
|| **marked** 
[`string`](../data-types.md) | Flag for marking. Indicator of whether the shipment is marked as problematic.

Possible values:
- `Y` — yes
- `N` — no ||
|| **dateMarked**
[`datetime`](../data-types.md) | Date the marking flag was changed ||
|| **reasonMarked** 
[`string`](../data-types.md) | Reason the shipment was marked with the marking flag ||
|| **empMarkedId** 
[`user.id`](../data-types.md) | User who set the marking flag to `Y` ||
|| **deliveryDocDate**
[`datetime`](../data-types.md) | Date of the shipment document || 
|| **deliveryDocNum** 
[`string`](../data-types.md) | Number of the shipment document ||
|| **trackingNumber** 
[`string`](../data-types.md) | Identifier of the shipment ||
|| **trackingDescription**
[`string`](../data-types.md) | Description of the shipment status ||
|| **trackingLastCheck**
[`datetime`](../data-types.md) | Time of the last status check of the shipment ||
|| **trackingStatus** 
[`string`](../data-types.md) | Status of the shipment ||
|| **currency** 
[`string`](../data-types.md) | Currency of the shipment ||
|| **customPriceDelivery** 
[`string`](../data-types.md) | Indicator of custom delivery cost.

For example, if the delivery service automatically calculated the cost at 500 dollars, but the manager manually set the cost to 200 dollars, then the custom delivery cost indicator will automatically be set to `Y`.

Possible values:
- `Y` — yes
- `N` — no ||
|| **basePriceDelivery** 
[`double`](../data-types.md) | Base delivery cost (without discounts/surcharges). ||
|| **priceDelivery** 
[`double`](../data-types.md) | Delivery cost ||
|| **discountPrice** 
[`double`](../data-types.md) | Delivery discount ||
|| **comments** 
[`string`](../data-types.md) | Manager's comment ||
|| **companyId** 
[`integer`](../data-types.md) | Identifier of the company from the "Online Store" module. Not used in the cloud version ||
|| **responsibleId** 
[`user.id`](../data-types.md) | Identifier of the user responsible for the shipment ||
|| **dateResponsibleId** 
[`datetime`](../data-types.md) | Date the responsible user for the shipment was changed ||
|| **empResponsibleId** 
[`user.id`](../data-types.md) | User who assigned the responsible person ||
|| **xmlId** 
[`string`](../data-types.md) | External identifier of the shipment.

Can be used for synchronizing the shipment with an external system ||
|| **externalDelivery** 
[`string`](../data-types.md) | Indicator of whether the shipment was uploaded from an external system (for example, QuickBooks and other similar platforms)

Possible values:
- `Y` — yes
- `N` — no ||
|| **id1c** 
[`string`](../data-types.md) | Identifier of the shipment in QuickBooks and other similar platforms ||
|| **updated1c** 
[`string`](../data-types.md) | Indicator of whether this shipment has been synchronized (updated) with QuickBooks and other similar platforms ||
|| **version1c** 
[`string`](../data-types.md) | Version of QuickBooks and other similar platforms (if the shipment was updated from QuickBooks and other similar platforms) ||
|| **shipmentItems** 
[`sale_order_shipment_item[]`](#sale_order_shipment_item) | Array containing the items of the shipment's table part ||
|#

### sale_status

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Symbolic identifier of the status ||
|| **type**
[`string`](../data-types.md) | Type of status:
- `O` — order status
- `D` — delivery status
||
|| **notify**
[`string`](../data-types.md) | Indicator of the need to send an email notification to the user when the entity (order or delivery) transitions to this status:
- `Y` — notify
- `N` — do not notify
||
|| **color**
[`string`](../data-types.md) | HEX code of the status color (for example, `#FF0000`) ||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **xmlId**
[`string`](../data-types.md) | External identifier of the status ||
|#

### sale_status_lang

#|
|| **Value**
`type` | **Description** ||
|| **statusId**
[`sale_status.id`](#sale_status) | Symbolic identifier of the status ||
|| **lid**
[`sale_lang.lid`](#sale_lang) | Language identifier ||
|| **name**
[`string`](../data-types.md) | Name of the status ||
|| **description**
[`string`](../data-types.md) | Description of the status ||
|#

### sale_lang

#|
|| **Value**
`type` | **Description** ||
|| **active**
[`string`](../data-types.md) | Indicator of the language's activity
- `Y` — active
- `N` — inactive ||
|| **lid**
[`string`](../data-types.md) | Symbolic identifier of the language ||
|| **name**
[`string`](../data-types.md) | Name of the language ||
|#

### sale_order_property

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the order property ||
|| **personTypeId**
[`sale_person_type.id`](#sale_person_type) | Identifier of the payer type ||
|| **propsGroupId**
[`sale_order_property_group.id`](#sale_order_property_group) | Identifier of the property group ||
|| **name**
[`string`](../data-types.md) | Name of the order property ||
|| **type**
[`string`](../data-types.md) | Type of the order property.

Possible values:
- `STRING` 
- `Y/N` 
- `NUMBER` 
- `ENUM` 
- `FILE` 
- `DATE` 
- `LOCATION` 
- `ADDRESS` ||
|| **code**
[`string`](../data-types.md) | Symbolic code of the order property ||
|| **active**
[`string`](../data-types.md) | Indicator of the order property’s activity.

Possible values:
- `Y` — yes
- `N` — no ||
|| **util**
[`string`](../data-types.md) | Indicator of whether the order property is a service property. Service properties are not displayed in the public part.

Possible values:
- `Y` — yes
- `N` — no ||
|| **userProps**
[`string`](../data-types.md) | Indicator of whether the order property is included in the customer profile.

Possible values:
- `Y` — yes
- `N` — no  ||
|| **isFiltered**
[`string`](../data-types.md) | Indicator of whether the order property is available in the filter on the order list page.

Possible values:
- `Y` — yes
- `N` — no  ||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|| **description**
[`string`](../data-types.md) | Description of the order property ||
|| **required**
[`string`](../data-types.md) | Indicator of whether filling in the order property value is mandatory.

Possible values:
- `Y` — yes
- `N` — no  ||

|| **multiple**
[`string`](../data-types.md) | Indicator of whether the order property is multiple. For multiple properties, it is possible to specify several values.

Possible values:
- `Y` — yes
- `N` — no || 
|| **xmlId**
[`string`](../data-types.md) | External identifier of the order property ||
|| **defaultValue**
[`string`\|`number`\|`string[]`\|`number[]`](../data-types.md) | Default value of the order property. 

For multiple order properties (`multiple`), an array of values is supported ||
|| **settings**
[`object`](../data-types.md) | See the description of the `settings` parameter in the method [sale.property.add](./property/sale-property-add.md) ||
|| **isProfileName**
[`string`](../data-types.md) | Indicator of whether to use the value of this order property as the user's profile name.

Possible values:
- `Y` — yes
- `N` — no 

Relevant only for order properties of type `STRING` ||
|| **isPayer**
[`string`](../data-types.md) | Indicator of whether to use the value of this order property as the payer's name.

Possible values:
- `Y` — yes
- `N` — no 

Relevant only for order properties of type `STRING` ||
|| **isEmail**
[`string`](../data-types.md) | Indicator of whether to use the value of this order property as an e-mail (for example, when registering a new user during order placement).

Possible values:
- `Y` — yes
- `N` — no 

Relevant only for order properties of type `STRING` ||
|| **isPhone**
[`string`](../data-types.md) | Indicator of whether to use the value of this order property as a phone number.

Possible values:
- `Y` — yes
- `N` — no 

Relevant only for order properties of type `STRING` ||
|| **isZip**
[`string`](../data-types.md) | Indicator of whether to use the value of this order property as a postal code.

Possible values:
- `Y` — yes
- `N` — no 

Relevant only for order properties of type `STRING` ||
|| **isAddress**
[`string`](../data-types.md) | Indicator of whether to use the value of this order property as an address.

Possible values:
- `Y` — yes
- `N` — no 

Relevant only for order properties of type `STRING` ||
|| **isLocation**
[`string`](../data-types.md) | Indicator of whether to use the value of this order property as the customer's location for calculating delivery costs.

Possible values:
- `Y` — yes
- `N` — no 

Relevant only for order properties of type `LOCATION` ||
|| **isLocation4tax**
[`string`](../data-types.md) | Indicator of whether to use the value of this order property as the customer's location for determining tax rates.

Possible values:
- `Y` — yes
- `N` — no 

Relevant only for order properties of type `LOCATION` ||
|| **inputFieldLocation**
[`string`](../data-types.md) | Deprecated field. Not used.

Relevant only for order properties of type `LOCATION` ||
|| **isAddressFrom**
[`string`](../data-types.md) | Indicator of whether to use the value of this order property as the customer's address from where the order needs to be picked up for calculating delivery costs.

Possible values:
- `Y` — yes
- `N` — no 

Relevant only for order properties of type `ADDRESS` ||
|| **isAddressTo**
[`string`](../data-types.md) | Indicator of whether to use the value of this order property as the customer's address to which the order needs to be delivered for calculating delivery costs.

Possible values:
- `Y` — yes
- `N` — no 

Relevant only for order properties of type `ADDRESS` ||
|#

### sale_shipment_property

See the description of [sale_order_property](#sale_order_property).

### sale_shipment_property_value

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the property value ||
|| **name**
[`string`](../data-types.md) | Name of the property ||
|| **shipmentId**
[`sale_order_shipment.id`](#sale_order_shipment) | Identifier of the shipment ||
|| **code**
[`string`](../data-types.md) | Symbolic code of the property ||
|| **value**
[`string`](../data-types.md)
[`sale_order_property_value_file_value`](#sale_order_property_value_file_value) | Value of the property ||
|| **shipmentPropsId**
[`sale_shipment_property.id`](#sale_shipment_property) | Identifier of the property ||
|| **shipmentPropsXmlId**
[`string`](../data-types.md) | External identifier of the shipment property ||
|#

### sale_order_property_value

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the property value ||
|| **name**
[`string`](../data-types.md) | Name of the property ||
|| **orderId**
[`sale_order.id`](#sale_order) | Identifier of the order ||
|| **code**
[`string`](../data-types.md) | Symbolic code of the property ||
|| **value**
[`string`](../data-types.md)
[`sale_order_property_value_file_value`](#sale_order_property_value_file_value) | Value of the property ||
|| **orderPropsId**
[`sale_order_property.id`](#sale_order_property) | Identifier of the property ||
|| **orderPropsXmlId**
[`string`](../data-types.md) | External identifier of the order property ||
|#

### sale_order_property_value_file_value

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Identifier of the file ||
|| **contentType**
[`string`](../data-types.md) | MIME type of the file ||
|| **description**
[`string`](../data-types.md) | Description of the file ||
|| **externalId**
[`string`](../data-types.md) | External identifier of the file ||
|| **fileName**
[`string`](../data-types.md) | Name of the file ||
|| **fileSize**
[`integer`](../data-types.md) | Size of the file in bytes ||
|| **moduleId**
[`string`](../data-types.md) | Module affiliation ||
|| **originalName**
[`string`](../data-types.md) | Original name of the file ||
|| **src**
[`string`](../data-types.md) | Full path to the file on the server ||
|| **subdir**
[`string`](../data-types.md) | Subdirectory where the file is located on the disk ||
|| **timestampX**
[`string`](../data-types.md) | Date of the file record modification ||
|| **versionOriginalId**
[`string`](../data-types.md) | Version of the file ||
|| **width**
[`string`](../data-types.md) | Width of the image in pixels (relevant only for image files) ||
|| **height**
[`string`](../data-types.md) | Height of the image in pixels (relevant only for image files) ||
|#

### sale_order_property_variant

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the property value variant ||
|| **name**
[`string`](../data-types.md) | Name of the property value variant ||
|| **value**
[`string`](../data-types.md) | Code of the property value variant ||
|| **orderPropsId**
[`sale_order_property.id`](#sale_order_property) | Identifier of the property ||
|| **description**
[`string`](../data-types.md) | Description of the property value variant ||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|#

### sale_order_property_group

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the property group ||
|| **name**
[`string`](../data-types.md) | Name of the property group ||
|| **personTypeId**
[`sale_person_type.id`](#sale_person_type) | Identifier of the payer type ||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|#

### sale_order_property_relation

#|
|| **Value**
`type` | **Description** ||
|| **entityId**
[`integer`](../data-types.md) | Identifier of the entity ||
|| **entityType**
[`string`](../data-types.md) | Type of the entity:

- `P` — payment system
- `D` — delivery
- `L` — landing
- `T` — trading platform ||
|| **propertyId**
[`sale_order_property.id`](#sale_order_property) | Identifier of the property ||
|#

### sale_person_type

#|
|| **Value**
`type` | **Description** ||
|| **id** 
[`integer`](../data-types.md) | Identifier of the payer type ||
|| **name**
[`string`](../data-types.md) | Name of the payer type ||
|| **code**
[`string`](../data-types.md) | Code of the payer type ||
|| **sort**
[`string`](../data-types.md) | Sorting ||
|| **active**
[`string`](../data-types.md) | Indicator of the payer type's activity: 
- `Y` — active
- `N` — inactive
||
|| **xmlId**
[`string`](../data-types.md) | External identifier.

Can be used to synchronize the current payer type with a similar position in an external system ||
|#

### sale_business_value_person_domain

#|
|| **Value**
`type` | **Description** ||
|| **personTypeId**
[`sale_person_type.id`](#sale_person_type) | Identifier of the payer type ||
|| **domain**
[`string`](../data-types.md) | Value corresponding to the payer type: individual or legal entity. 

- `I` — individual
- `E` — legal entity 

This option is needed for the business meanings mechanism ||
|#

### sale_order

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the order ||
|| **lid**
[`string`](../data-types.md) | Identifier of the site where this payer type will be used. Has a constant value of `s1` ||
|| **dateInsert**
[`datetime`](../data-types.md) | Date of order creation ||
|| **dateUpdate**
[`datetime`](../data-types.md) | Date of order update ||
|| **personTypeId**
[`sale_person_type.id`](#sale_person_type) | Identifier of the payer type ||
|| **personTypeXmlId**
[`string`](../data-types.md) | External identifier of the payer type ||
|| **statusId**
[`sale_status.id`](#sale_status) | Identifier of the status ||
|| **empStatusId**
[`user.id`](../data-types.md) | Identifier of the user who changed the order status ||
|| **dateStatus**
[`datetime`](../data-types.md) | Date of status change ||
|| **marked**
[`string`](../data-types.md) | Marking flag. Indicates whether the shipment is marked as problematic. The value `Y` is set automatically if an error occurred during saving.
- `Y` — yes
- `N` — no
||
|| **dateMarked**
[`datetime`](../data-types.md) | Date of order marking ||
|| **empMarkedId**
[`user.id`](../data-types.md) | Identifier of the user who marked the order ||
|| **reasonMarked**
[`string`](../data-types.md) | Reason why the order was marked ||
|| **price**
[`double`](../data-types.md) | Price ||
|| **discountValue**
[`double`](../data-types.md) | Discount value ||
|| **taxValue**
[`double`](../data-types.md) | Tax rate on the order ||
|| **userDescription**
[`string`](../data-types.md) | Customer's comment on the order ||
|| **additionalInfo**
[`string`](../data-types.md) | Deprecated. Additional information ||
|| **comments**
[`string`](../data-types.md) | Manager's comment on the order ||
|| **responsibleId**
[`user.id`](../data-types.md) | Identifier of the user responsible for the order ||
|| **recurringId**
[`integer`](../data-types.md) | Identifier of the subscription renewal ||
|| **lockedBy**
[`user.id`](../data-types.md) | Relevant only for on-premise. 

Identifier of the user who locked the order. The order is locked in the admin panel when the user opens the detail form of the order
 ||
|| **dateLock**
[`datetime`](../data-types.md) | Date of locking ||
|| **recountFlag**
[`string`](../data-types.md) | Deprecated. Recount flag:
- `Y` — yes
- `N` — no 
||
|| **affiliateId**
[`integer`](../data-types.md) | Relevant only for on-premise. Identifier of the affiliate ||
|| **updated1c**
[`string`](../data-types.md) | Whether the order was updated through QuickBooks and other similar platforms:
- `Y` — yes
- `N` — no 
||
|| **orderTopic**
[`string`](../data-types.md) | Deprecated. Order topic ||
|| **xmlId**
[`string`](../data-types.md) | External identifier ||
|| **statusXmlId**
[`string`](../data-types.md) | External identifier of the status ||
|| **id1c**
[`string`](../data-types.md) | Identifier in QuickBooks and other similar platforms ||
|| **version**
[`integer`](../data-types.md) | Document version ||
|| **version1c**
[`string`](../data-types.md) | Version in QuickBooks and other similar platforms ||
|| **externalOrder**
[`string`](../data-types.md) | Whether the order is from an external system
- `Y` — yes
- `N` — no 
||
|| **canceled**
[`string`](../data-types.md) | Whether the order was canceled:
- `Y` — yes
- `N` — no 
||
|| **dateCanceled**
[`datetime`](../data-types.md) | Date of cancellation ||
|| **empCanceledId**
[`user.id`](../data-types.md) | Identifier of the user who canceled the order ||
|| **reasonCanceled**
[`string`](../data-types.md) | Reason for cancellation ||
|| **userId**
[`user.id`](../data-types.md) | Identifier of the client ||
|| **currency**
[`string`](../data-types.md) | Currency. The list of currencies can be obtained through the method [crm.currency.list](../crm/currency/crm-currency-list.md) ||
|| **accountNumber**
[`string`](../data-types.md) | Order number ||
|| **payed**
[`string`](../data-types.md) | Whether the order is paid:
- `Y` — yes
- `N` — no 
||
|| **deducted**
[`string`](../data-types.md) | Deprecated. Whether the order is shipped:
- `Y` — yes
- `N` — no 
||
|| **basketItems**
[`sale_basket_item[]`](#sale_basket_item) | Items in the order basket ||
|| **clients**
[`sale_order_crm_client[]`](#sale_order_crm_client)
Array of contacts and companies of the order in the CRM module | Array of objects containing information about the order's links to clients in the CRM module ||
|| **payments**
[`sale_order_payment[]`](#sale_order_payment) | Payments for the order ||
|| **shipments**
[`sale_order_shipment[]`](#sale_order_shipment) | Shipments of the order ||
|| **propertyValues**
[`sale_order_property[]`](#sale_order_property) | Properties of the order ||
|| **requisiteLink**
Array of links to requisites in the CRM module | Links of requisites to the order. The list of links to requisites with CRM entities can be obtained through the method [crm.requisite.link.list](../crm/requisites/links/crm-requisite-link-list.md), where for the order [entity_type_id = 14](../crm/data-types.md) ||
|| **tradeBindings**
[`sale_order_trade_binding[]`](#sale_order_trade_binding) | Trading platforms of the order ||
|#

### sale_order_crm_client

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the binding ||
|| **entityId**
[`integer`](../data-types.md) | Identifier of the CRM object ||
|| **entityTypeId**
[`integer`](../data-types.md) | Identifier of the [`CRM object type`](../crm/data-types.md#object_type) ||
|| **isPrimary**
[`string`](../data-types.md) | Indicator of whether the client is primary.

Possible values:
- `Y` - yes
- `N` - no ||
|| **orderId**
[`sale_order.id`](#sale_order) | Identifier of the order ||
|| **roleId**
[`integer`](../data-types.md) | Deprecated ||
|| **sort**
[`integer`](../data-types.md) | Sorting ||
|#

### sale_order_payment

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Payment identifier ||
|| **orderId**
[`sale_order.id`](#sale_order) | Order identifier ||
|| **paySystemXmlId**
[`string`](../data-types.md) | External identifier of the payment system ||
|| **paySystemIsCash**
[`string`](../data-types.md) | Is the payment system cash-based:
- `Y` — yes
- `N` — no 
||
|| **accountNumber**
[`string`](../data-types.md) | System payment number ||
|| **paid**
[`string`](../data-types.md) | Has the payment been made:
- `Y` — yes
- `N` — no 
||
|| **datePaid**
[`datetime`](../data-types.md) | Payment date ||
|| **empPaidId**
[`user.id`](../data-types.md) | User who made the payment ||
|| **paySystemId**
[`sale_paysystem.id`](#sale_paysystem) | Payment system identifier ||
|| **psStatus**
[`string`](../data-types.md) | Status of the payment system transaction — whether the order has been successfully paid (for payment systems that allow automatic retrieval of data for orders processed through them):
- `Y` — yes
- `N` — no 
||
|| **psStatusCode**
[`string`](../data-types.md) | Payment system transaction status code ||
|| **psStatusDescription**
[`string`](../data-types.md) | Description of the payment system transaction status ||
|| **psStatusMessage**
[`string`](../data-types.md) | Message of the payment system transaction status ||
|| **psSum**
[`double`](../data-types.md) | Amount of the payment system transaction ||
|| **psCurrency**
[`string`](../data-types.md) | Currency of the payment system ||
|| **psResponseDate**
[`datetime`](../data-types.md) | Payment system response date ||
|| **payVoucherNum**
[`string`](../data-types.md) | Payment document number ||
|| **payVoucherDate**
[`date`](../data-types.md) | Payment document date ||
|| **datePayBefore**
[`datetime`](../data-types.md) | Date by which the invoice must be paid (not used in the store) ||
|| **dateBill**
[`datetime`](../data-types.md) | Invoice date ||
|| **xmlId**
[`string`](../data-types.md) | External identifier ||
|| **sum**
[`double`](../data-types.md) | Payment amount ||
|| **currency**
[`string`](../data-types.md) | Payment currency ||
|| **paySystemName**
[`string`](../data-types.md) | Name of the payment system ||
|| **companyId**
[`integer`](../data-types.md) | Identifier of the company that will receive the payment ||
|| **payReturnNum**
[`string`](../data-types.md) | Return document number ||
|| **priceCod**
[`string`](../data-types.md) | Cost of payment upon delivery (used, for example, for cash on delivery) ||
|| **payReturnDate**
[`date`](../data-types.md) | Return document date ||
|| **empReturnId**
[`user.id`](../data-types.md) | Identifier of the user who processed the return ||
|| **payReturnComment**
[`string`](../data-types.md) | Comment on the return ||
|| **responsibleId**
[`user.id`](../data-types.md) | Identifier of the user responsible for the payment ||
|| **empResponsibleId**
[`user.id`](../data-types.md) | Identifier of the user who assigned the responsible person ||
|| **dateResponsibleId**
[`datetime`](../data-types.md) | Date of assigning the responsible person ||
|| **isReturn**
[`string`](../data-types.md) | Was the return processed:
- `Y` — yes
- `N` — no 
||
|| **comments**
[`string`](../data-types.md) | Comments on the payment ||
|| **updated1c**
[`string`](../data-types.md) | Was the payment updated through QuickBooks and other similar platforms:
- `Y` — yes
- `N` — no 
||
|| **id1c**
[`string`](../data-types.md) | Identifier in QuickBooks and other similar platforms ||
|| **version1c**
[`string`](../data-types.md) | Payment document version from QuickBooks and other similar platforms ||
|| **externalPayment**
[`string`](../data-types.md) | Is the payment external:
- `Y` — yes
- `N` — no 
||
|| **psInvoiceId**
[`integer`](../data-types.md) | Payment identifier in the payment system ||
|| **marked**
[`string`](../data-types.md) | Marking flag. Indicates whether the payment is marked as problematic:
- `Y` — yes
- `N` — no 
||
|| **reasonMarked**
[`string`](../data-types.md) | Reason for marking ||
|| **dateMarked**
[`datetime`](../data-types.md) | Date of marking the order ||
|| **empMarkedId**
[`user.id`](../data-types.md) | Identifier of the user who marked the payment ||
|#

### sale_basket_item

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the basket item ||
|| **orderId**
[`sale_order.id`](#sale_order) | Order identifier ||
|| **sort**
[`integer`](../data-types.md) | Position in the list of order items ||
|| **productId**
[`integer`](../data-types.md) | Product identifier. For products not available on the site, it is zero ||
|| **price**
[`double`](../data-types.md) | Price of the product including discounts and surcharges ||
|| **customPrice**
[`string`](../data-types.md) | Is the price set manually:
- `Y` — yes
- `N` — no 
||
|| **currency**
[`string`](../data-types.md) | Currency of the price. Must match the currency of the order. The list of currencies can be obtained using the method [crm.currency.list](../crm/currency/crm-currency-list.md), detailed information about the currency — using the method [crm.currency.get](../crm/currency/crm-currency-get.md) ||
|| **quantity**
[`double`](../data-types.md) | Quantity ||
|| **xmlId**
[`string`](../data-types.md) | External code of the basket item.
If not specified, it will be generated automatically.
Used for synchronization with external systems (for example, QuickBooks and other similar platforms)
 ||
|| **dateInsert**
[`datetime`](../data-types.md) | Date the basket item was added ||
|| **dateUpdate**
[`datetime`](../data-types.md) | Date the basket item was updated ||
|| **properties**
[`sale_basket_item_property[]`](#sale_basket_item_property) | Properties of the basket item ||
|| **name**
[`string`](../data-types.md) | Name of the product ||
|| **basePrice**
[`double`](../data-types.md) | Price of the product excluding discounts and surcharges

The rule must always be followed: `basePrice = price + discountPrice`
 ||
|| **discountPrice**
[`double`](../data-types.md) | Amount of the final discount/surcharge. For surcharges, the value is negative
 ||
|| **weight**
[`double`](../data-types.md) | Weight in grams. 
For on-premise versions, the unit of weight change is specified in the settings of the online store module (sale)
 ||
|| **dimensions**
[`string`](../data-types.md) | Dimensions of the product in millimeters.

The field is either empty or contains a serialized array with keys:
- `WIDTH` — width
- `HEIGHT` — height
- `LENGTH` — length
 ||
|| **measureCode**
[`string`](../data-types.md) | Unit of measure code ||
|| **measureName**
[`string`](../data-types.md) | Name of the unit of measure ||
|| **canBuy**
[`string`](../data-types.md) | Is the product available for purchase:
- `Y` — yes
- `N` — no 
||
|| **vatRate**
[`double`](../data-types.md) | Tax rate in percentage. Can be `null` ("No VAT" — in case VAT rates are used) ||
|| **vatIncluded**
[`string`](../data-types.md) | Is the tax included in the price:
- `Y` — yes
- `N` — no 
 ||
|| **barcodeMulti**
[`string`](../data-types.md) | This field is only available when inventory management is enabled. Is the barcode unique:
- `Y` — yes
- `N` — no 

Makes sense only for enabled inventory management. It is strongly discouraged to fill this field manually for products from the catalog
||
|| **type**
[`integer`](../data-types.md) | Type of the item. Does not correspond to the type of product in the catalog. Possible values:
- `1` — set
- `2` — service
- `null` — any other

It is strongly discouraged to set this manually
 ||
|| **catalogXmlId**
[`string`](../data-types.md) | External identifier of the catalog.

Used for synchronization with external systems (for example, QuickBooks and other similar platforms)
 ||
|| **productXmlId**
[`string`](../data-types.md) | External identifier of the product.

Used for synchronization with external systems (for example, QuickBooks and other similar platforms) ||
|| **reservations**
[`sale_basket_item_reservation[]`](#sale_basket_item_reservation) | Reservations of the basket item ||
|#

### sale_basket_item_property
#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the basket item property ||
|| **basketId**
[`sale_basket_item.id`](#sale_basket_item) | Identifier of the basket item ||
|| **name**
[`string`](../data-types.md) | Name of the property ||
|| **value**
[`string`](../data-types.md) | Value of the property ||
|| **code**
[`string`](../data-types.md) | Code of the property ||
|| **sort**
[`integer`](../data-types.md) | Sort order ||
|| **xmlId**
[`string`](../data-types.md) | External identifier ||
|#

### sale_order_trade_platform
#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the trading platform ||
|| **code**
[`string`](../data-types.md) | Code of the trading platform ||
|| **active**
[`string`](../data-types.md) | Is the trading platform active

- `Y` — yes
- `N` — no ||
|| **name**
[`string`](../data-types.md) | Name of the trading platform ||
|| **description**
[`string`](../data-types.md) | Description of the trading platform ||
|| **settings**
[`string`](../data-types.md) | Settings of the trading platform in serialized form ||
|| **catalogSectionTabClassName**
[`string`](../data-types.md) | Class for processing the tab in the catalog section settings ||
|| **class**
[`string`](../data-types.md) | Class of the trading platform ||
|| **xmlId**
[`string`](../data-types.md) | External identifier ||
|#

### sale_order_trade_binding
#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the binding ||
|| **xmlId**
[`string`](../data-types.md) | External identifier of the binding ||
|| **externalOrderId**
[`string`](../data-types.md) | Order number in the external system ||
|| **orderId**
[`sale_order.id`](#sale_order) | Order identifier ||
|| **tradingPlatformId**
[`sale_order_trade_platform.id`](#sale_order_trade_platform) | Identifier of the trading platform ||
|| **tradingPlatformXmlId**
[`string`](../data-types.md) | External identifier of the trading platform ||
|| **params**
[`string`](../data-types.md) | Parameters in serialized form ||
|#

### sale_basket_item_reservation

#|
|| **Value**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Identifier of the reservation ||
|| **basketId**
[`sale_basket_item.id`](#sale_basket_item) | Identifier of the basket item reservation ||
|| **storeId**
[`integer`](../data-types.md) | Identifier of the inventory ||
|| **quantity**
[`double`](../data-types.md) | Quantity ||
|| **dateReserve**
[`datetime`](../data-types.md) | Reservation date ||
|| **dateReserveEnd**
[`datetime`](../data-types.md) | Reservation end date ||
|| **reservedBy**
[`user.id`](../data-types.md) | Identifier of the user who added the reservation ||
|#

### sale_delivery_handler

#|
|| **Value**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the delivery service handler.

You can obtain the identifiers of delivery service handlers using the method [sale.delivery.handler.list](./delivery/handler/sale-delivery-handler-list.md)  ||
|| **NAME**
[`string`](../data-types.md) | Name of the delivery service handler ||
|| **CODE**
[`string`](../data-types.md) | Symbolic code of the delivery service handler ||
|| **SORT**
[`integer`](../data-types.md) | Sorting ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the delivery service handler ||
|| **SETTINGS**
[`sale_delivery_handler_settings`](#sale_delivery_handler_settings) | Object containing information about the delivery service settings ||
|| **PROFILES**
[`sale_delivery_handler_profile[]`](#sale_delivery_handler_profile) | Array containing a list of delivery profile objects ||
|#

### sale_delivery_handler_settings

#|
|| **Value**
`type` | **Description** ||
|| **CALCULATE_URL**
[`string`](../data-types.md) | URL for calculating delivery costs.

Data about the parcel (what to deliver, where, and how) is sent to this URL to calculate the delivery cost in response.

The request and response format is detailed in the webhook documentation [Calculating Delivery Costs](./delivery/webhooks/calculate.md)
 ||
|| **CREATE_DELIVERY_REQUEST_URL**
[`string`](../data-types.md) | URL for creating a delivery order.

Data about the parcel (what to deliver, where, and how) is sent to this URL to create an order with the delivery service.

The request and response format is detailed in the webhook documentation [Creating a Delivery Order](./delivery/webhooks/create-delivery-request.md)
 ||
|| **CANCEL_DELIVERY_REQUEST_URL**
[`string`](../data-types.md) | URL for canceling a delivery order.

Data about the parcel (what to deliver, where, and how) is sent to this URL to cancel the order with the delivery service.

The request and response format is detailed in the webhook documentation [Canceling a Delivery Order](./delivery/webhooks/cancel-delivery-request.md) ||
|| **HAS_CALLBACK_TRACKING_SUPPORT**
[`string`](../data-types.md) | Indicator of whether the delivery service will send notifications about the status of the delivery order (see the method [sale.delivery.request.sendmessage](./delivery/delivery-request/sale-delivery-request-send-message.md)). 

If event support is indicated, a delivery activity will be created in the manager's interface when ordering delivery, which can receive updates related to the current delivery status.

Possible values:

`Y` — support available
`N` — no support ||
|| **CONFIG**
[`sale_delivery_handler_settings_config_item[]`](#sale_delivery_handler_settings_config_item) | Array of objects with available settings for the delivery service created using this handler ||
|#

### sale_delivery_handler_settings_config_item

#|
|| **Value**
`type` | **Description** ||
|| **TYPE**
[`string`](../data-types.md) | Type of the setting field. 

Possible values:

`STRING` — string
`Y/N` — checkbox (yes / no)
`NUMBER` — number
`ENUM` — list
`DATE` — date
`LOCATION` — location ||
|| **CODE**
[`string`](../data-types.md) | Symbolic code of the setting ||
|| **NAME**
[`string`](../data-types.md) | Name of the setting ||
|| **OPTIONS**
[`object`](../data-types.md) | List of options for selection. An object in the format `key=value`. Where `key` is the option code, and `value` is the options.

Example:

```
{
    "Option1Code": "Option1Value",
    "Option2Code": "Option2Value",
    "Option3Code": "Option3Value"
}
```

This parameter is relevant only for settings of type `ENUM` ||
|#

### sale_delivery_handler_profile

#|
|| **Value**
`type` | **Description** ||
|| **NAME**
[`string`](../data-types.md) | Name of the delivery service handler profile ||
|| **CODE**
[`string`](../data-types.md) | Symbolic code of the delivery service handler profile ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the delivery service handler profile ||
|#

### sale_delivery_service

#|
|| **Value**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the delivery service.

You can obtain the identifiers of delivery services using the method [sale.delivery.getlist](./delivery/delivery/sale-delivery-get-list.md) ||
|| **PARENT_ID**
[`integer`](../data-types.md) | Identifier of the parent delivery service.

You can obtain the identifiers of delivery services using the method [sale.delivery.getlist](./delivery/delivery/sale-delivery-get-list.md) ||
|| **NAME**
[`string`](../data-types.md) | Name of the delivery service ||
|| **CURRENCY**
[`string`](../data-types.md) | Symbolic code of the delivery service currency ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the delivery service ||
|| **SORT**
[`integer`](../data-types.md) | Sorting ||
|| **ACTIVE**
[`string`](../data-types.md) | Indicator of the delivery service's activity.

Possible values:

`Y` — active
`N` — inactive ||
|#

### sale_delivery_config_value_item

#|
|| **Value**
`type` | **Description** ||
|| **CODE**
[`string`](../data-types.md) | Symbolic code of the setting ||
|| **VALUE**
[`any`](../data-types.md) | Value of the setting ||
|#

### sale_delivery_extra_service

#|
|| **Value**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the additional service of the delivery service.

You can obtain the identifiers of delivery service services using the method [sale.delivery.extra.service.get](./delivery/extra-service/sale-delivery-extra-service-get.md) ||
|| **TYPE**
[`string`](../data-types.md) | Type of service. 

Possible values:

`enum` — list (selecting an option from a pre-formed list)
`checkbox` — single service (e.g., delivery to the door)
`quantity` — quantitative service (e.g., required number of movers) ||
|| **NAME**
[`string`](../data-types.md) | Name of the service ||
|| **ACTIVE**
[`string`](../data-types.md) | Indicator of the service's activity.

Possible values:
`Y` — active
`N` — inactive ||
|| **CODE**
[`string`](../data-types.md) | Symbolic code of the service ||
|| **SORT**
[`integer`](../data-types.md) | Sorting ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the service ||
|| **PRICE**
[`double`](../data-types.md) | This field is relevant only for services of type **single service** (`checkbox`) and **quantitative service** (`quantity`) ||
|| **ITEMS**
[`sale_delivery_extra_service_enum_item[]`](#sale_delivery_extra_service_enum_item) | List of available options for selection.

This field is relevant only for services of type **list** (`enum`) ||
|#

### sale_delivery_extra_service_enum_item

#|
|| **Value**
`type` | **Description** ||
|| **TITLE**
[`string`](../data-types.md) | Name of the list option ||
|| **CODE**
[`string`](../data-types.md) | Symbolic code of the list option ||
|| **PRICE**
[`double`](../data-types.md) | Cost of the service when selecting this option in the currency of the delivery service ||
|#

### sale_paysystem_handler

#|
|| **Value**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Identifier of the payment system handler ||
|| **NAME**
[`string`](../data-types.md) | Name of the handler ||
|| **CODE**
[`string`](../data-types.md) | Code of the handler ||
|| **SORT**
[`string`](../data-types.md) | Sorting ||
|| **SETTINGS**
[`object`](../data-types.md) | Settings of the handler. The structure corresponds to that specified when adding the handler via [sale.paysystem.handler.add](../pay-system/sale-pay-system-handler-add.md) in the `SETTINGS` parameter ||
|#

### sale_paysystem

#|
|| **Value**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Identifier of the payment system ||
|| **NAME**
[`string`](../data-types.md) | Name of the payment system ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the payment system ||
|| **XML_ID**
[`string`](../data-types.md) | Symbolic code ||
|| **PERSON_TYPE_ID**
[`sale_person_type.id`](#sale_person_type) | Identifier of the payer type ||
|| **ACTION_FILE**
[`string`](../data-types.md) | For REST payment systems, this is the REST handler code specified when adding the handler via the method [sale.paysystem.handler.add](../pay-system/sale-pay-system-handler-add.md).

For system payment systems, this is the code of the system payment system handler
||
|| **ACTIVE**
[`string`](../data-types.md) | Is the payment system active? Available values: 
- `Y` — yes
- `N` — no 
||
|| **ENTITY_REGISTRY_TYPE**
[`string`](../data-types.md) | Binding of the payment system:
- `ORDER` — value for store orders, deals, smart processes
- `CRM_INVOICE` — value for CRM invoices
- `CRM_QUOTE` — value for CRM estimates
||
|| **NEW_WINDOW**
[`string`](../data-types.md) | Flag for the setting "Open in a new window". Available values: 
- `Y` — yes
- `N` — no 
||
|| **ALLOW_EDIT_PAYMENT**
[`string`](../data-types.md) | Flag for the setting "Allow automatic payment recalculation". Available values: 
- `Y` — yes
- `N` — no
||
|| **AUTO_CHANGE_1C**
[`string`](../data-types.md) | Flag for the setting "Allow automatic payment change when importing from QuickBooks and other similar platforms". Available values: 
- `Y` — yes
- `N` — no
||
|| **CAN_PRINT_CHECK**
[`string`](../data-types.md) | Flag for the setting "Allow printing receipts". Available values: 
- `Y` — yes
- `N` — no
||
|| **ENCODING**
[`string`](../data-types.md) | Setting "Encoding". Available values: `windows-1251`, `utf-8`, `iso-8859-1`. 

Not used for REST handlers ||
|| **IS_CASH**
[`string`](../data-types.md) | Type of payment. Possible values: 
- `N` — cashless 
- `Y` — cash 
- `A` — acquiring operation ||
|| **PSA_NAME**
[`string`](../data-types.md) | Title of the payment system ||
|| **PS_MODE**
[`string`](../data-types.md) | Operating mode of the payment system for handlers that support multiple operating modes ||
|| **SORT**
[`string`](../data-types.md) | Sorting ||
|| **PLAN**
[`string`](../data-types.md) | Not used ||
|#

### sale_cashbox_handler

#|
|| **Value**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Identifier of the cash register handler ||
|| **NAME**
[`string`](../data-types.md) | Name of the handler ||
|| **CODE**
[`string`](../data-types.md) | Code of the handler ||
|| **SORT**
[`string`](../data-types.md) | Sorting ||
|| **SETTINGS**
[`object`](../data-types.md) | Settings of the handler. The structure corresponds to that specified when adding the handler via [sale.cashbox.handler.add](./cashbox/sale-cashbox-handler-add.md) in the `SETTINGS` parameter ||
|#

### sale_cashbox

#|
|| **Value**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Identifier of the payment system handler ||
|| **NAME**
[`string`](../data-types.md) | Name of the cash register ||
|| **ENABLED**
[`string`](../data-types.md) | Availability of the cash register. Possible values: 
- `Y` — yes
- `N` — no ||
|| **ACTIVE**
[`string`](../data-types.md) | Activity of the cash register. Possible values: 
- `Y` — yes
- `N` — no ||
|| **OFD**
[`string`](../data-types.md) | Code of the OFD handler. Available OFD handlers: 
- `bx_firstofd` — First OFD 
- `bx_platformaofd` — OFD Platform 
- `bx_yarusofd` — OFD YARUS
- `bx_taxcomofd` — Taxcom OFD 
- `bx_ofdruofd` — OFD.RU 
- `bx_tenzorofd` — Tensor OFD 
- `bx_conturofd` — Contour OFD ||
|| **EMAIL**
[`string`](../data-types.md) | Email address to which notifications will be sent in case of errors when printing receipts ||
|| **KKM_ID**
[`string`](../data-types.md) | Brand of the KKM ||
|| **SORT**
[`string`](../data-types.md) | Sorting ||
|| **USE_OFFLINE**
[`string`](../data-types.md) | Is the cash register used offline? Possible values:
- `Y` — yes
- `N` — no ||
|#

## Objects Used in Responses

### rest_field_description

#|
|| **Value**
`type` | **Description** ||
|| **isImmutable**
[`boolean`](../data-types.md) | Indicator of whether the field value can be changed after creation. 

If this indicator is set for a field, the value can be specified when creating the entity, but cannot be changed during updates ||
|| **isReadOnly**
[`boolean`](../data-types.md) | Read-only indicator. 

If this indicator is set for a field, the value of the field does not need to be passed in add and update operations. The value is generated automatically and is intended for read-only access ||
|| **isRequired**
[`boolean`](../data-types.md) | Indicator of whether the field is required for add or update operations ||
|| **type**
[`string`](../data-types.md) | Data type of the field values. Possible values: 
- `integer`
- `double`
- `string`
- `char`
- `list`
- `text`
- `file`
- `date`
- `datetime`
- `datatype`
- `productproperty`
||
|#
