# Add Delivery Service sale.delivery.add

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method adds a delivery service.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **REST_CODE***
[`string`](../../../data-types.md) | Symbolic code of the delivery service handler (see the `CODE` field of the [`sale_delivery_handler`](../../data-types.md) object). ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the delivery service ||
|| **CURRENCY***
[`crm_currency.CURRENCY`](../../../crm/data-types.md) | Symbolic code of the delivery service currency ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description of the delivery service ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Indicator of the delivery service's activity.
Possible values:
- `Y` — yes
- `N` — no

If not provided, defaults to `Y` ||
|| **CONFIG**
[`object[]`](../../../data-types.md) | Values of the delivery service settings (detailed description provided [below](#parametr-config)).
The structure of the settings (code, name, data type) is defined when creating or updating the delivery service handler using the methods:
- [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md)
- [sale.delivery.handler.update](../handler/sale-delivery-handler-update.md)||
|| **LOGOTYPE**
[`string`](../../../data-types.md) | Logo of the delivery service (base64-encoded string) ||
|#

### CONFIG Parameter

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../../data-types.md) | Symbolic code of the setting ||
|| **VALUE***
[`any`](../../../data-types.md) | Value of the setting ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"REST_CODE":"uber","NAME":"Uber Taxi","CURRENCY":"USD","DESCRIPTION":"Uber Taxi Description","SORT":"500","ACTIVE":"Y","CONFIG":[{"CODE":"SETTING_1","VALUE":"SETTING_1 string value"},{"CODE":"SETTING_2","VALUE":"Y"},{"CODE":"SETTING_3","VALUE":199.99},{"CODE":"SETTING_4","VALUE":"Option3Code"},{"CODE":"SETTING_5","VALUE":"10.04.2024"},{"CODE":"SETTING_6","VALUE":"0000144961"}],"LOGOTYPE":"iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"REST_CODE":"uber","NAME":"Uber Taxi","CURRENCY":"USD","DESCRIPTION":"Uber Taxi Description","SORT":"500","ACTIVE":"Y","CONFIG":[{"CODE":"SETTING_1","VALUE":"SETTING_1 string value"},{"CODE":"SETTING_2","VALUE":"Y"},{"CODE":"SETTING_3","VALUE":199.99},{"CODE":"SETTING_4","VALUE":"Option3Code"},{"CODE":"SETTING_5","VALUE":"10.04.2024"},{"CODE":"SETTING_6","VALUE":"0000144961"}],"LOGOTYPE":"iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.delivery.add', {
    			REST_CODE: "uber",
    			NAME: "Uber Taxi",
    			CURRENCY: "USD",
    			DESCRIPTION: "Uber Taxi Description",
    			SORT: "500",
    			ACTIVE: "Y",
    			CONFIG: [{
    					CODE: "SETTING_1",
    					VALUE: "SETTING_1 string value",
    				},
    				{
    					CODE: "SETTING_2",
    					VALUE: "Y",
    				},
    				{
    					CODE: "SETTING_3",
    					VALUE: 199.99,
    				},
    				{
    					CODE: "SETTING_4",
    					VALUE: "Option3Code",
    				},
    				{
    					CODE: "SETTING_5",
    					VALUE: "10.04.2024",
    				},
    				{
    					CODE: "SETTING_6",
    					VALUE: "0000144961",
    				},
    			],
    			LOGOTYPE: "iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElFTkSuQmCCiVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BM­VEX37ff////58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGVKw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCoc­SfQFGKP3+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA­/q2TwrXZib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt­3qSQtwdJSsku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+2­8tICq4rTqXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQ­EFhV3CCNTph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKr­ihqje7Y9iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guv­ayybW1i3Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWt­JSyP21r+FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0h­Ptw86hMX99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC",
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
                'sale.delivery.add',
                [
                    'REST_CODE'    => "uber",
                    'NAME'         => "Uber Taxi",
                    'CURRENCY'     => "USD",
                    'DESCRIPTION'  => "Uber Taxi Description",
                    'SORT'         => "500",
                    'ACTIVE'       => "Y",
                    'CONFIG'       => [
                        [
                            'CODE'  => "SETTING_1",
                            'VALUE' => "SETTING_1 string value",
                        ],
                        [
                            'CODE'  => "SETTING_2",
                            'VALUE' => "Y",
                        ],
                        [
                            'CODE'  => "SETTING_3",
                            'VALUE' => 199.99,
                        ],
                        [
                            'CODE'  => "SETTING_4",
                            'VALUE' => "Option3Code",
                        ],
                        [
                            'CODE'  => "SETTING_5",
                            'VALUE' => "10.04.2024",
                        ],
                        [
                            'CODE'  => "SETTING_6",
                            'VALUE' => "0000144961",
                        ],
                    ],
                    'LOGOTYPE'     => "iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElF­TkSuQmCCiVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BM­VEX37ff////58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7E­AAAOxAGVKw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCoc­SfQFGKP3+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA­/q2TwrXZib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt­3qSQtwdJSsku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+2­8tICq4rTqXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQ­EFhV3CCNTph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKr­ihqje7Y9iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guv­ayybW1i3Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWt­JSyP21r+FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0h­Ptw86hMX99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC",
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding delivery method: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.add', {
            REST_CODE: "uber",
            NAME: "Uber Taxi",
            CURRENCY: "USD",
            DESCRIPTION: "Uber Taxi Description",
            SORT: "500",
            ACTIVE: "Y",
            CONFIG: [{
                    CODE: "SETTING_1",
                    VALUE: "SETTING_1 string value",
                },
                {
                    CODE: "SETTING_2",
                    VALUE: "Y",
                },
                {
                    CODE: "SETTING_3",
                    VALUE: 199.99,
                },
                {
                    CODE: "SETTING_4",
                    VALUE: "Option3Code",
                },
                {
                    CODE: "SETTING_5",
                    VALUE: "10.04.2024",
                },
                {
                    CODE: "SETTING_6",
                    VALUE: "0000144961",
                },
            ],
    LOGOTYPE: "iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xfAAAAAElFTkSuQmCC",
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
        'sale.delivery.add',
        [
            'REST_CODE' => "uber",
            'NAME' => "Uber Taxi",
            'CURRENCY' => "USD",
            'DESCRIPTION' => "Uber Taxi Description",
            'SORT' => "500",
            'ACTIVE' => "Y",
            'CONFIG' => [
                ['CODE' => "SETTING_1", 'VALUE' => "SETTING_1 string value"],
                ['CODE' => "SETTING_2", 'VALUE' => "Y"],
                ['CODE' => "SETTING_3", 'VALUE' => 199.99],
                ['CODE' => "SETTING_4", 'VALUE' => "Option3Code"],
                ['CODE' => "SETTING_5", 'VALUE' => "10.04.2024"],
                ['CODE' => "SETTING_6", 'VALUE' => "0000144961"],
            ],
            'LOGOTYPE' => "iVBORw0KGgoAAAANSUhEUgAAAMgAAADIBAMAAABfdrOtAAAAG1BMVEX37ff/­///58fn9+v3+/P779vv8+Pz47/j68/oDfe+3AAAACXBIWXMAAA7EAAAOxAGV­Kw4bAAABrUlEQVR4nO3UT0/CMBjH8ccx2I56IFynkHg1SgxHHCocSfQFGKP3­+e++xL1wn7bPUCAeKF5Mvp+EluX3ZN3ariIAAAAAAAAAAAAAAAAA/q2TwrXZ­ib94LTbj5GdgVbtKxhdXS+2uL270ajQbL9fz4WzcXwVWtbNeIdmt3qSQtwdJ­Ssku1/NHkfdVEKriHFey0G4haS3+ty4ZtEGoipMW+VS7T2m0zc+28tICq4rT­qXtuJV7kWdvsUJtuoc1Hm08ssKo4B1Wn1i6tJu5qrj9dA8lWEzOQEFhV3CCN­Tph2naJ0V+eu0SV+ry3WWQqBVcUNsgiP16ndS4SnzuffL5LWEgKrihqje7Y9­iDTN6mZ38geDNNX2dEm338b5XPafrmRuj/dj4fULfGoXeFTJ/guvayybW1i3­Vl7aM7h+3y2c+y07FfeZjaT9GHVrNYXPG/fkIbCqCPf+9d1WKiWtJSyP21r+­FaTrZ8+CULW7XliCUe0PyIUdkD29qQzdv7A0FoSq3R0fqaU78d0hPtw86hMX­99vAqqJlp757/W3vhMCqAAAAAAAAAAAAAAAAAPxbX82/SILlk9xf­AAAAAElFTkSuQmCC"
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
   "result":{
      "parent":{
         "NAME":"Uber Taxi",
         "ACTIVE":"Y",
         "DESCRIPTION":"Uber Taxi Description",
         "CURRENCY":"USD",
         "ID":196,
         "PARENT_ID":null,
         "SORT":500,
         "LOGOTYPE":973
      },
      "profiles":[
         {
            "NAME":"Taxi",
            "ACTIVE":"Y",
            "DESCRIPTION":"Taxi Delivery",
            "CURRENCY":"USD",
            "ID":197,
            "PARENT_ID":196,
            "SORT":500,
            "LOGOTYPE":null
         },
         {
            "NAME":"Cargo",
            "ACTIVE":"Y",
            "DESCRIPTION":"Cargo Delivery",
            "CURRENCY":"USD",
            "ID":198,
            "PARENT_ID":196,
            "SORT":500,
            "LOGOTYPE":null
         }
      ]
   },
   "time":{
      "start":1713946187.579457,
      "finish":1713946187.925688,
      "duration":0.34623098373413086,
      "processing":0.06604909896850586,
      "date_start":"2024-04-24T11:09:47+02:00",
      "date_finish":"2024-04-24T11:09:47+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **parent**
[`sale_delivery_service`](../../data-types.md) | Object with information about the added delivery service ||
|| **profiles**
[`sale_delivery_service[]`](../../data-types.md) | Array of objects with information about the added child delivery services (delivery profiles) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
   "error":"ERROR_CHECK_FAILURE",
   "error_description":"Parameter NAME is not defined"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | 400 ||
|| `ERROR_DELIVERY_ADD` | Error while trying to add a delivery service | 400 ||
|| `ERROR_HANDLER_NOT_FOUND` | Delivery service handler with the specified `REST_CODE` not found | 400 ||
|| `ACCESS_DENIED` | Insufficient rights to add a delivery service | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-update.md)
- [{#T}](./sale-delivery-delete.md)
- [{#T}](./sale-delivery-config-update.md)
- [{#T}](./sale-delivery-config-get.md)
- [{#T}](./sale-delivery-get-list.md)