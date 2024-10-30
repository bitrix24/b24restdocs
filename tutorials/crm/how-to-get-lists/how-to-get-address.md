# How to Get a Client's Address from CRM

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

A client's address can be stored in Bitrix:

* in a user-defined field of type "address" for any CRM entity. To retrieve the address from the field, call the get or list method for the desired entity type.
* in the [requisites](../../../api-reference/crm/requisites/index.md) of contacts, companies, and leads. Within a single field `Address`, multiple addresses can be stored with their types specified. A single client may have several requisites recorded.

To obtain a client's address from the requisites, sequentially execute two methods:

1. [crm.requisite.list](../../../api-reference/crm/requisites/universal/crm-requisite-list.md)
2. [crm.address.list](../../../api-reference/crm/requisites/addresses/crm-address-list.md)

## 1. Retrieving Requisites Associated with a Contact

Obtaining the requisite ID is a necessary step, as the address does not have a direct link to the contact or company. The address is linked to the requisite object.

To retrieve the requisites, we use the crm.requisite.list method with the following filter:

* in `ENTITY_TYPE_ID`, specify the value `3` — the identifier for [contact type](../../../api-reference/crm/data-types.md#object_type). For company type, use the identifier `4`.
* in `ENTITY_ID` — the contact ID, in this example `2429`. You can obtain the ID using the [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) method with a filter based on any known contact field. To get the company ID, use [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md). If you need to obtain the contact or company ID by phone number or email, refer to the tutorial [“Finding Duplicates by Phone Number”](./search-by-phone-and-email.md).

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
   BX24.callMethod(
    "crm.requisite.list",
        {
        filter: { 
             "ENTITY_TYPE_ID": "3", 
             "ENTITY_ID": "2429",      
            },
        select: [
            "ID",
            "ENTITY_TYPE_ID",
            "ENTITY_ID",
            ],
        },
    );
    ```

- PHP

    ```php  
    require_once('crest.php');

        $result = CRest::call(
            'crm.requisite.list',
            [
                'filter' => [
                    'ENTITY_TYPE_ID' => '3',
                    'ENTITY_ID' => '2429',
                ],
                'select' => [
                    'ID',
                    'ENTITY_TYPE_ID',
                    'ENTITY_ID',
                ],
            ]
        );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```
{% endlist %}

We obtained the requisite ID `361` — a parameter necessary for the next request.

 ```json
    Array
    (
     [result] => Array
        (
            [0] => Array
                (
                    [ID] => 361
                    [ENTITY_TYPE_ID] => 3
                    [ENTITY_ID] => 2429
                )
        )
     [total] => 1      
    )
 ```

## 2. Retrieving the Address

To obtain the address, we use the crm.address.list method with the following filter:

* in `ENTITY_TYPE_ID`, specify the value `8` — the identifier for [requisite type](../../../api-reference/crm/data-types.md#object_type)
* in `ENTITY_ID` — the requisite ID obtained from the previous request, in this example `361`
* in `TYPE_ID` — the [address type](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md), if you need to retrieve a specific one. For example, the delivery address type is `11`, and the legal address type is `6`.

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        "crm.address.list",
        {
            filter: { 
            "ENTITY_TYPE_ID": 8, 
            "ENTITY_ID": 361,  
            "TYPE_ID": 11, 
            },
        },
    );
    ```

- PHP

    ```php   
    require_once('crest.php');

        $result = CRest::call(
            'crm.address.list',
            [
                'filter' => [
                    'ENTITY_TYPE_ID' => 8,
                    'ENTITY_ID' => 361,
                    'TYPE_ID' => 11,
                ],
            ]
        );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```
{% endlist %}

We received the delivery address data for the contact.

```json
    Array
    (
        [result] => Array
            (
                [0] => Array
                    (
                        [TYPE_ID] => 11
                        [ENTITY_TYPE_ID] => 8
                        [ENTITY_ID] => 361
                        [ADDRESS_1] => 45th Lane, 10 c1
                        [ADDRESS_2] => 
                        [CITY] => New York
                        [POSTAL_CODE] => 10001
                        [REGION] => Manhattan
                        [PROVINCE] => New York
                        [COUNTRY] => USA
                        [COUNTRY_CODE] => 
                        [LOC_ADDR_ID] => 571
                        [ANCHOR_TYPE_ID] => 3
                        [ANCHOR_ID] => 2429
                    )
            )
        [total] => 1       
    )
 ```

## Code Example

{% list tabs %}

- JS

    ```javascript
    var contactId = "your_contact_ID_here"; // Replace with the actual contact ID

    // Method to retrieve the requisite ID
    BX24.callMethod(
        "crm.requisite.list",
        {
            filter: {
                "ENTITY_TYPE_ID": 3,
                "ENTITY_ID": contactId
            },
            select: ["ID"]
        },
        function(requisiteResult) {
            if (requisiteResult.error()) {
                console.error(requisiteResult.error());
            } else {
                var requisites = requisiteResult.data();
                if (requisites.length > 0) {
                    var requisiteId = requisites[0].ID;
                    console.log("Requisite ID:", requisiteId);

                    // Method to retrieve the address
                    BX24.callMethod(
                        "crm.address.list",
                        {
                            filter: {
                                "ENTITY_TYPE_ID": 8,
                                "ENTITY_ID": requisiteId,
                                "TYPE_ID": 11
                            }
                        },
                        function(addressResult) {
                            if (addressResult.error()) {
                                console.error(addressResult.error());
                            } else {
                                var addresses = addressResult.data();
                                if (addresses.length > 0) {
                                    // Create a table to display the addresses
                                    var table = [];
                                    addresses.forEach(function(address) {
                                        table.push({
                                            "Address": address.ADDRESS_1 || "Not specified",
                                            "City": address.CITY || "Not specified",
                                            "Postal Code": address.POSTAL_CODE || "Not specified",
                                            "Country": address.COUNTRY || "Not specified"
                                        });
                                    });
                                    console.table(table);
                                } else {
                                    console.log("Delivery address not found.");
                                }
                            }
                        }
                    );
                } else {
                    console.log("Requisite not found.");
                }
            }
        }
    );
    ```


- PHP

    ```php  
    require_once('crest.php');

        $contactId = 'your_contact_ID_here'; // Replace with the actual contact ID

        // Method to retrieve the requisite ID
        $requisiteResult = CRest::call(
            'crm.requisite.list',
            [
                'filter' => [
                    'ENTITY_TYPE_ID' => 3,
                    'ENTITY_ID' => $contactId
                ],
                'select' => ['ID']
            ]
        );

        if (isset($requisiteResult['error'])) {
            echo 'Error: ' . $requisiteResult['error_description'];
        } else {
            $requisites = $requisiteResult['result'];
            if (count($requisites) > 0) {
                $requisiteId = $requisites[0]['ID'];
                echo 'Requisite ID: ' . $requisiteId . PHP_EOL;

                // Method to retrieve the address
                $addressResult = CRest::call(
                    'crm.address.list',
                    [
                        'filter' => [
                            'ENTITY_TYPE_ID' => 8,
                            'ENTITY_ID' => $requisiteId,
                            'TYPE_ID' => 11
                        ]
                    ]
                );

                if (isset($addressResult['error'])) {
                    echo 'Error: ' . $addressResult['error_description'];
                } else {
                    $addresses = $addressResult['result'];
                    if (count($addresses) > 0) {
                        // Create a table to display the addresses
                        echo '<table border="1">';
                        echo '<tr><th>Address</th><th>City</th><th>Postal Code</th><th>Country</th></tr>';
                        foreach ($addresses as $address) {
                            echo '<tr>';
                            echo '<td>' . ($address['ADDRESS_1'] ?? 'Not specified') . '</td>';
                            echo '<td>' . ($address['CITY'] ?? 'Not specified') . '</td>';
                            echo '<td>' . ($address['POSTAL_CODE'] ?? 'Not specified') . '</td>';
                            echo '<td>' . ($address['COUNTRY'] ?? 'Not specified') . '</td>';
                            echo '</tr>';
                        }
                        echo '</table>';
                    } else {
                        echo 'Delivery address not found.';
                    }
                }
            } else {
                echo 'Requisite not found.';
            }
        }
    ```
{% endlist %}