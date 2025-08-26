# Get Order Field Values and Related Objects sale.order.get

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.order.get` is designed to retrieve values for all fields of an order and related objects.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order.id`](../data-types.md) | Order identifier ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.order.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.order.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.order.get", {
    			"id": 6
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.order.get',
                [
                    'id' => 6
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Order data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting order: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.order.get", {
            "id": 6
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.order.get',
        [
            'id' => 6
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
        "order": {
            "accountNumber": "392",
            "additionalInfo": "",
            "affiliateId": null,
            "basketItems": [
                {
                    "barcodeMulti": "N",
                    "basePrice": 980,
                    "canBuy": "Y",
                    "catalogXmlId": "cbc2957f-09fc-4b2a-b9de-6f925c4c9047",
                    "currency": "USD",
                    "customPrice": "N",
                    "dateInsert": "2024-02-28T17:35:06+02:00",
                    "dateRefresh": null,
                    "dateUpdate": "2024-02-28T17:37:08+02:00",
                    "detailPageUrl": "",
                    "dimensions": "a:3:{s:5:\u0022WIDTH\u0022;N;s:6:\u0022HEIGHT\u0022;N;s:6:\u0022LENGTH\u0022;N;}",
                    "discountCoupon": "",
                    "discountName": "",
                    "discountPrice": 0,
                    "discountValue": "",
                    "fuserId": 4,
                    "id": "255",
                    "lid": "s1",
                    "measureCode": "796",
                    "measureName": "pcs",
                    "module": "catalog",
                    "name": "Men's Fire T-Shirt",
                    "notes": "BASE",
                    "orderId": "236",
                    "price": 980,
                    "priceTypeId": 1,
                    "productId": 348,
                    "productPriceId": 112,
                    "productProviderClass": "\\Bitrix\\Catalog\\Product\\CatalogProvider",
                    "productXmlId": "1000000386",
                    "properties": [
                        {
                            "basketId": 255,
                            "code": "CATALOG.XML_ID",
                            "id": 139,
                            "name": "Catalog XML_ID",
                            "sort": 100,
                            "value": "cbc2957f-09fc-4b2a-b9de-6f925c4c9047",
                            "xmlId": "bx_65df52a9ac502"
                        },
                        {
                            "basketId": 255,
                            "code": "PRODUCT.XML_ID",
                            "id": 140,
                            "name": "Product XML_ID",
                            "sort": 100,
                            "value": "1000000386",
                            "xmlId": "bx_65df52a9ace55"
                        }
                    ],
                    "quantity": 1,
                    "recommendation": "",
                    "reservations": [
                        {
                            "basketId": 255,
                            "dateReserve": "2024-02-28T17:36:51+02:00",
                            "dateReserveEnd": "2024-03-01T23:00:00+02:00",
                            "id": 39,
                            "quantity": 1,
                            "reservedBy": null,
                            "storeId": 1
                        }
                    ],
                    "setParentId": "",
                    "sort": 100,
                    "subscribe": "N",
                    "type": "",
                    "vatIncluded": "Y",
                    "vatRate": 0.2,
                    "weight": 0,
                    "xmlId": "bx_65df52a9ab47f"
                }
            ],
            "canceled": "N",
            "clients": [
                {
                    "entityId": 6,
                    "entityTypeId": 3,
                    "id": 901,
                    "isPrimary": "Y",
                    "orderId": 236,
                    "roleId": 0,
                    "sort": 0
                }
            ],
            "comments": "",
            "companyId": 0,
            "currency": "USD",
            "dateCanceled": null,
            "dateInsert": "2024-02-28T17:36:55+02:00",
            "dateLock": null,
            "dateMarked": null,
            "dateStatus": "2024-02-28T17:36:38+02:00",
            "dateUpdate": "2024-02-28T17:37:11+02:00",
            "deducted": "N",
            "discountValue": 0,
            "empCanceledId": null,
            "empMarkedId": null,
            "empStatusId": 1,
            "externalOrder": "N",
            "id": 236,
            "id1c": "",
            "lid": "s1",
            "lockedBy": "",
            "marked": "N",
            "orderTopic": "",
            "payed": "N",
            "payments": [
                {
                    "accountNumber": "392\/1",
                    "comments": "",
                    "companyId": 0,
                    "currency": "USD",
                    "dateBill": "2024-02-28T17:36:44+02:00",
                    "dateMarked": null,
                    "datePaid": null,
                    "datePayBefore": null,
                    "dateResponsibleId": null,
                    "empMarkedId": null,
                    "empPaidId": null,
                    "empResponsibleId": null,
                    "empReturnId": null,
                    "externalPayment": "N",
                    "id": 123,
                    "id1c": "",
                    "isReturn": "N",
                    "marked": "N",
                    "orderId": 236,
                    "paid": "N",
                    "payReturnComment": "",
                    "payReturnDate": null,
                    "payReturnNum": "",
                    "paySystemId": 48,
                    "paySystemIsCash": "N",
                    "paySystemName": "Card Payment",
                    "paySystemXmlId": "bx_65df3d512af59",
                    "payVoucherDate": null,
                    "payVoucherNum": "",
                    "priceCod": "0",
                    "psCurrency": "",
                    "psInvoiceId": 2,
                    "psResponseDate": null,
                    "psStatus": "",
                    "psStatusCode": "",
                    "psStatusDescription": "",
                    "psStatusMessage": "",
                    "psSum": null,
                    "reasonMarked": "",
                    "responsibleId": null,
                    "sum": 1480,
                    "updated1c": "N",
                    "version1c": "",
                    "xmlId": "bx_65df530c472fa"
                }
            ],
            "personTypeId": 3,
            "personTypeXmlId": "",
            "price": 1480,
            "propertyValues": [
                {
                    "code": "FIO",
                    "id": 1514,
                    "name": "First Last",
                    "orderPropsId": 20,
                    "orderPropsXmlId": null,
                    "value": "Artem Gavrilenko"
                },
                {
                    "code": "EMAIL",
                    "id": 1515,
                    "name": "E-Mail1",
                    "orderPropsId": 21,
                    "orderPropsXmlId": "bx_63a082af0d250",
                    "value": "Artemlxvl@my-mail.com"
                },
                {
                    "code": "PHONE",
                    "id": 1516,
                    "name": "Phone1",
                    "orderPropsId": 22,
                    "orderPropsXmlId": "bx_63a082a06864d",
                    "value": "14151234567"
                },
                {
                    "code": "CONTACT_ADDRESS",
                    "id": 1517,
                    "name": "Address",
                    "orderPropsId": 42,
                    "orderPropsXmlId": null,
                    "value": null
                },
                {
                    "code": "ADDRESS",
                    "id": 1518,
                    "name": "Delivery Address",
                    "orderPropsId": 26,
                    "orderPropsXmlId": null,
                    "value": "test"
                }
            ],
            "reasonCanceled": "",
            "reasonMarked": "",
            "recountFlag": "Y",
            "recurringId": "",
            "requisiteLink": {
                "bankDetailId": 0,
                "mcBankDetailId": 0,
                "mcRequisiteId": 0,
                "requisiteId": 3
            },
            "responsibleId": 1,
            "shipments": [
                {
                    "accountNumber": "392\/2",
                    "allowDelivery": "N",
                    "basePriceDelivery": 500,
                    "canceled": "N",
                    "comments": "",
                    "companyId": 0,
                    "currency": "USD",
                    "customPriceDelivery": "N",
                    "dateAllowDelivery": null,
                    "dateCanceled": null,
                    "dateDeducted": null,
                    "dateInsert": "2024-02-28T17:36:41+02:00",
                    "dateMarked": null,
                    "dateResponsibleId": null,
                    "deducted": "N",
                    "deliveryDocDate": null,
                    "deliveryDocNum": "",
                    "deliveryId": 1,
                    "deliveryName": "Courier Delivery",
                    "deliveryXmlId": "",
                    "discountPrice": 0,
                    "empAllowDeliveryId": null,
                    "empCanceledId": null,
                    "empDeductedId": null,
                    "empMarkedId": null,
                    "empResponsibleId": null,
                    "externalDelivery": "N",
                    "id": 338,
                    "id1c": "",
                    "marked": "N",
                    "orderId": 236,
                    "priceDelivery": 500,
                    "reasonMarked": "",
                    "reasonUndoDeducted": "",
                    "responsibleId": null,
                    "shipmentItems": [
                        {
                            "basketId": 255,
                            "dateInsert": "2024-02-28T17:36:53+02:00",
                            "id": 320,
                            "orderDeliveryId": 338,
                            "quantity": 1,
                            "reservedQuantity": 1,
                            "xmlId": "bx_65df5309cf490"
                        }
                    ],
                    "statusId": "DN",
                    "statusXmlId": "",
                    "system": "N",
                    "trackingDescription": "",
                    "trackingLastCheck": "",
                    "trackingNumber": "",
                    "trackingStatus": "",
                    "updated1c": "N",
                    "version1c": "",
                    "xmlId": "bx_65df5309939b8"
                }
            ],
            "statusId": "N",
            "statusXmlId": "",
            "taxValue": 163.33,
            "tradeBindings": [
                {
                    "externalOrderId": "236",
                    "id": 224,
                    "orderId": 236,
                    "params": null,
                    "tradingPlatformId": "9",
                    "tradingPlatformXmlId": "bx_659ff72864c8f",
                    "xmlId": "bx_65df53067ac59"
                }
            ],
            "updated1c": "N",
            "userDescription": "",
            "userId": 1,
            "version": 1,
            "version1c": "",
            "xmlId": "bx_65df53063d56d"
        }
    },
    "time": {
        "start": 1712938174.436428,
        "finish": 1712938175.432068,
        "duration": 0.9956400394439697,
        "processing": 0.5710320472717285,
        "date_start": "2024-04-12T19:09:34+02:00",
        "date_finish": "2024-04-12T19:09:35+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **order**
[`sale_order`](../data-types.md) | Order information ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200540400001,
    "error_description":"order does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201240400001` | Order not found ||
|| `200040300010` | Insufficient permissions to read the order ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-order-add.md)
- [{#T}](./sale-order-update.md)
- [{#T}](./sale-order-list.md)
- [{#T}](./sale-order-delete.md)
- [{#T}](./sale-order-get-fields.md)