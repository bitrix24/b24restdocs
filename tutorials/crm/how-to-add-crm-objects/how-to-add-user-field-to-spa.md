# How to Create a Custom Field in a SPA

> Scope: [`crm, userfieldconfig`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify the SPA

Custom fields enhance the functionality of the CRM to meet your business needs:

- You can create fields to store information in various formats: string, money, number, address, file, and others.

- You can configure field characteristics: names for different languages, multiple field flag, rounding settings for numeric fields, and more.

To create a custom field in a SPA, we will sequentially execute two methods:

1. [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) — retrieve the SPA ID.

2. [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-add.md) — create a custom field in the SPA.

## 1. Retrieve the SPA Identifier {#spa-id}

To obtain the SPA ID, we use the method [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) with the filter:

- `title` — specify the name of the SPA.

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```JavaScript
    BX24.callMethod(
        'crm.type.list',
        {
            filter: { // array of fields for filtering
                "title": "Equipment Purchase" // name of the SPA
            }
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.type.list',
        [
            'filter' => [
                'title' => 'Equipment Purchase' // name of the SPA
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive the id — this is the ordinal number of the SPA in Bitrix24. In the example, `id`: `7`.

```json
{
    "result": {
        "types": [
            {
                "id": 7,
                "title": "Equipment Purchase",
                "code": "",
                "createdBy": 1,
                "entityTypeId": 177,
                "customSectionId": null,
                "isCategoriesEnabled": "Y",
                "isStagesEnabled": "Y",
                "isBeginCloseDatesEnabled": "Y",
                "isClientEnabled": "Y",
                "isUseInUserfieldEnabled": "Y",
                "isLinkWithProductsEnabled": "Y",
                "isMycompanyEnabled": "Y",
                "isDocumentsEnabled": "Y",
                "isSourceEnabled": "Y",
                "isObserversEnabled": "Y",
                "isRecyclebinEnabled": "Y",
                "isAutomationEnabled": "Y",
                "isBizProcEnabled": "Y",
                "isSetOpenPermissions": "Y",
                "isPaymentsEnabled": "N",
                "isCountersEnabled": "N",
                "createdTime": "2021-11-26T10:52:17+03:00",
                "updatedTime": "2024-11-12T15:32:39+03:00",
                "updatedBy": 1
            }
        ]
    }
}
```

## 2. Create a Custom Field in the SPA

To create a custom field, we use the method [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-add.md) with the following parameters:

- `moduleId` — the identifier of the module in which the method will create the field, a required parameter. The module for SPAs is `crm`.

- `field[entityId]` — the identifier of the object in the format `CRM_ + {ID}`, where ID is the ordinal number of the SPA in Bitrix24 from the result of [crm.type.list](./how-to-add-user-field-to-spa.md#spa-id), a required parameter. In the example, we will specify `CRM_7`.

- `field[fieldName]` — the field code in the format `UF_ + {object identifier} + _ + {arbitrary string in UPPERCASE}`. The length limit for the code is 50 characters, a required parameter. In the example, we will specify `UF_CRM_7_NEW_REST_LIST`.

- `field[userTypeId]` — the identifier of the [field type](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md), a required parameter. In the example, we will specify `enumeration` to create a list-type field, and we will pass the list values in a separate `enum` array.

- `field[multiple]` — the multiple field flag, an optional parameter. The multiplicity flag cannot be changed after the field is created.

- `field[editFormLabel]` — an array of names for displaying the field in Bitrix24 in different languages. An optional parameter; if no name is provided, the field code will be displayed in Bitrix24.

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'userfieldconfig.add',
        {
            moduleId: 'crm', // Module identifier
            field: {
                entityId: 'CRM_7', // Object identifier
                fieldName: 'UF_CRM_7_NEW_REST_LIST', // Field code
                userTypeId: 'enumeration', // Field type identifier
                multiple: 'Y', // Multiple field flag
                editFormLabel: { 
                    'de': 'Merkmal Liste', // Field name in German
                    'en': 'List of characteristics' // Field name in English
                },
                enum: [ // List field values
                    {
                        value: 'Characteristic 1', // Option value
                        def: 'N', // Default value flag
                        sort: 100, // Sort index
                    },
                    {
                        value: 'Characteristic 2',
                        def: 'Y', // This option will be the default value
                        sort: 200,
                    }
                ]
            }
        },
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldconfig.add',
        [
            'moduleId' => 'crm', // Module identifier
            'field' => [
                'entityId' => 'CRM_7', // Object identifier
                'fieldName' => 'UF_CRM_7_NEW_REST_LIST', // Field code
                'userTypeId' => 'enumeration', // Field type identifier
                'multiple' => 'Y', // Multiple field flag
                'editFormLabel' => [
                    'de' => 'Merkmal Liste', // Field name in German
                    'en' => 'List of characteristics' // Field name in English
                ],
                'enum' => [ // List field values
                    [
                        'value' => 'Characteristic 1', // Option value
                        'def' => 'N', // Default value flag
                        'sort' => 100, // Sort index
                    ],
                    [
                        'value' => 'Characteristic 2',
                        'def' => 'Y', // This option will be the default value
                        'sort' => 200,
                    ]
                ]
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive the data of the created field.

```json
{
    "result": {
        "field": {
            "id": "6953",
            "entityId": "CRM_7",
            "fieldName": "UF_CRM_7_NEW_REST_LIST",
            "userTypeId": "enumeration",
            "xmlId": null,
            "sort": "100",
            "multiple": "Y",
            "mandatory": "N",
            "showFilter": "N",
            "showInList": "Y",
            "editInList": "Y",
            "isSearchable": "N",
            "settings": {
                "DISPLAY": "LIST",
                "LIST_HEIGHT": 1,
                "CAPTION_NO_VALUE": "",
                "SHOW_NO_VALUE": "Y"
            },
            "languageId": {
                "en": "en",
                "de": "de"
            },
            "editFormLabel": {
                "en": "List of characteristics",
                "de": "Merkmal Liste"
            },
            "listColumnLabel": {
                "en": null,
                "de": null
            },
            "listFilterLabel": {
                "en": null,
                "de": null
            },
            "errorMessage": {
                "en": null,
                "de": null
            },
            "helpMessage": {
                "en": null,
                "de": null
            },
            "enum": [
                {
                    "id": "3363",
                    "userFieldId": "6953",
                    "value": "Characteristic 1",
                    "def": "N",
                    "sort": "100",
                    "xmlId": "56dff18efcfe25f3bae0117a6b372567"
                },
                {
                    "id": "3365",
                    "userFieldId": "6953",
                    "value": "Characteristic 2",
                    "def": "Y",
                    "sort": "200",
                    "xmlId": "42e3ebcf5506a65283bf3bf510d8f05a"
                }
            ]
        }
    },
}
```

## Code Example

{% list tabs %}

- JS
  
    ```JavaScript
    // Function to retrieve the SPA and create a custom field
    function getCrmTypeAndAddUserField() {
        // Variable for user input of the SPA name
        var processTitle = prompt("Enter the name of the SPA to search:", "Your_Process_Name");
        // Call the crm.type.list method to retrieve the SPA
        BX24.callMethod(
            'crm.type.list',
            {
                filter: {
                    "title": processTitle // Use the name entered by the user
                }
            },
            function(result) {
                if (result.error()) {
                    console.error('Error retrieving the SPA:', result.error());
                } else {
                    console.log('SPA successfully retrieved:', result.data());
                    var spaId = result.data().types[0].id; // Use the id from the result
                    addUserField(spaId);
                }
            }
        );
    }

    // Function to create a custom field
    function addUserField(spaId) {
        // Call the userfieldconfig.add method to create a custom field
        BX24.callMethod(
            'userfieldconfig.add',
            {
                moduleId: 'crm',
                field: {
                    entityId: 'CRM_' + spaId, // Use the id from the previous result
                    fieldName: 'UF_CRM_' + spaId + '_NEW_REST_LIST', // Use the id
                    userTypeId: 'enumeration',
                    multiple: 'Y',
                    editFormLabel: {
                        'de': 'Merkmal Liste',
                        'en': 'List of characteristics'
                    },
                    enum: [
                        {
                            value: 'Characteristic 1',
                            def: 'N',
                            sort: 100
                        },
                        {
                            value: 'Characteristic 2',
                            def: 'Y',
                            sort: 200
                        }
                    ]
                }
            },
            function(result) {
                if (result.error()) {
                    console.error('Error creating custom field:', result.error());
                } else {
                    console.log('Custom field successfully created:', result.data());
                }
            }
        );
    }

    // Call the function to retrieve SPA data and create a custom field
    getCrmTypeAndAddUserField();
    ```

- PHP
  
    ```php
    require_once('crest.php');

    // Function to retrieve the SPA and create a custom field
    function getCrmTypeAndAddUserField($processTitle) {
        // Call the crm.type.list method to retrieve the SPA
        $result = CRest::call('crm.type.list', [
            'filter' => [
                'title' => $processTitle // Use the name entered by the user
            ]
        ]);

        if (isset($result['error'])) {
            echo 'Error retrieving the SPA: ' . $result['error_description'];
        } else {
            echo 'SPA successfully retrieved: ';
            print_r($result['result']);
            
            if (!empty($result['result']['types'])) {
                $spaId = $result['result']['types'][0]['id']; // Use the id from the result
                addUserField($spaId);
            } else {
                echo 'SPA not found.';
            }
        }
    }

    // Function to create a custom field
    function addUserField($spaId) {
        // Call the userfieldconfig.add method to create a custom field
        $result = CRest::call('userfieldconfig.add', [
            'moduleId' => 'crm',
            'field' => [
                'entityId' => 'CRM_' . $spaId, // Use the id from the previous result
                'fieldName' => 'UF_CRM_' . $spaId . '_NEW_REST_LIST', // Use the id
                'userTypeId' => 'enumeration',
                'multiple' => 'Y',
                'editFormLabel' => [
                    'de' => 'Merkmal Liste',
                    'en' => 'List of characteristics'
                ],
                'enum' => [
                    [
                        'value' => 'Characteristic 1',
                        'def' => 'N',
                        'sort' => 100
                    ],
                    [
                        'value' => 'Characteristic 2',
                        'def' => 'Y',
                        'sort' => 200
                    ]
                ]
            ]
        ]);

        if (isset($result['error'])) {
            echo 'Error creating custom field: ' . $result['error_description'];
        } else {
            echo 'Custom field successfully created: ';
            print_r($result['result']);
        }
    }

    // Call the function to retrieve SPA data and create a custom field
    $processTitle = readline("Enter the name of the SPA to search: ");
    getCrmTypeAndAddUserField($processTitle);
    ```

{% endlist %}