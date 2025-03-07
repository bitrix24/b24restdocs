# How to Set Up Rounding for a Custom Field of Type "Number"

Custom fields have standard settings: name, required status, and multiple values.

Additionally, there are specialized settings depending on the field type:
- values for the list
- rounding precision for numbers
- currency for monetary fields

To obtain specialized settings for the "Number" type — `double`, we use the method [crm.userfield.settings.fields](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-settings-fields.md):

{% list tabs %}

- JS
  
    ```JavaScript
    BX24.callMethod(
        "crm.userfield.settings.fields",
        {
            type: "double" // type of the custom field
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.userfield.settings.fields',
        [
            'type' => 'double' // Type of the custom field
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

{% endlist %}

As a result, we will receive two settings: default value and precision.

```JSON
{
    "result": {
        "DEFAULT_VALUE": {
            "type": "double",
            "title": "Default Value"
        },
        "PRECISION": {
            "type": "int",
            "title": "Precision"
        }
    }
}
```

## Creating a Numeric Field with Rounding Setting

We will create a field of type number with a precision setting of three decimal places. If a value with four or more decimal places is entered in the field, it will automatically round to three decimal places.

To create a custom field, we use the method [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-add.md) with the following parameters:

- `moduleId` — the identifier of the module in which the method will create the field, a required parameter. In the example, we create a field for deals, the module is `crm`.
- `field[entityId]` — the identifier of the object in the format `CRM_ + {ID}`, a required parameter. The list of identifiers for objects can be found in the article — [Entity ID Identifiers](../../../api-reference/crm/universal/userfieldconfig/entity-id.md). In the example, we will specify `CRM_DEAL`.

- `field[fieldName]` — the field code in the format `UF_ + {object identifier} + _ + {arbitrary string in UPPERCASE}`. The length limit for the code is 50 characters, a required parameter. In the example, we will specify `UF_CRM_DEAL_NEW_DOUBLE_FIELD`.

- `field[userTypeId]` — the identifier of the [field type](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md), a required parameter. In the example, we will specify `double` to create a number type field.

- `field[editFormLabel]` — an array of names for displaying the field in Bitrix24 in different languages. An optional parameter; if no name is provided, the field code will be displayed in Bitrix24.

- `field[settings]` — an array of additional settings for the field depending on its type. An optional parameter; if not provided, default settings will be used. In the example, we will specify the `PRECISION` setting — precision. We will pass an integer equal to the number of decimal places.

{% list tabs %}

- JS
  
    ```JavaScript
    BX24.callMethod(
        'userfieldconfig.add',
        {
            moduleId: 'crm', // Module identifier
            field: {
                entityId: 'CRM_DEAL', // Object identifier
                fieldName: 'UF_CRM_DEAL_NEW_DOUBLE_FIELD', // Field code
                userTypeId: 'double', // Field type identifier
                editFormLabel: { 
                    'en': 'Number with Rounding', // Field name in English
                    'en': 'PRECISION double' // Field name in English
                },
                settings: { // Additional field settings
                        PRECISION: 3, // Number of decimal places
                    },
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
                'entityId' => 'CRM_DEAL', // Object identifier
                'fieldName' => 'UF_CRM_DEAL_NEW_DOUBLE_FIELD', // Field code
                'userTypeId' => 'double', // Field type identifier
                'editFormLabel' => [
                    'en' => 'Number with Rounding', // Field name in English
                    'en' => 'PRECISION double' // Field name in English
                ],
                'settings' => [ // Additional field settings
                    'PRECISION' => 3 // Number of decimal places
                ]
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive the data of the created field.

```JSON
{
    "result": {
        "field": {
            "id": "6961",
            "entityId": "CRM_DEAL",
            "fieldName": "UF_CRM_DEAL_NEW_DOUBLE_FIELD",
            "userTypeId": "double",
            "xmlId": null,
            "sort": "100",
            "multiple": "N",
            "mandatory": "N",
            "showFilter": "N",
            "showInList": "Y",
            "editInList": "Y",
            "isSearchable": "N",
            "settings": {
                "PRECISION": 3,
                "SIZE": 20,
                "MIN_VALUE": 0,
                "MAX_VALUE": 0,
                "DEFAULT_VALUE": null
            },
            "languageId": {
                "en": "en"
            },
            "editFormLabel": {
                "en": "PRECISION double",
                "en": "Number with Rounding"
            },
            "listColumnLabel": {
                "en": null
            },
            "listFilterLabel": {
                "en": null
            },
            "errorMessage": {
                "en": null
            },
            "helpMessage": {
                "en": null
            }
        }
    },
}
```

## Modifying the Setting of an Existing Numeric Field

To change the rounding setting of an existing field, we use the method [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-update.md) by specifying the field ID. The field ID can be obtained in two ways: when creating the field using the method [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-add.md) or through the method to retrieve the list of custom fields of the object. In the example, the field is for deals, so we will use the method [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md).

### 1. Getting the Field ID

To get the field ID, we use the method [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) with the following parameters:

- `filter[LANG]` — a filter by language used to display field names in the desired language. Without this filter, names will not be displayed.

- `filter[USER_TYPE_ID]` — a filter by field type used to retrieve only fields of type "Number" in the result.

{% list tabs %}

- JS
  
    ```JavaScript
    BX24.callMethod(
        "crm.deal.userfield.list",
        {
            filter: {
                LANG: 'en', // Language filter for displaying field name
                USER_TYPE_ID: 'double' // Field type filter
            }
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.userfield.list',
        [
            'filter' => [
                'LANG' => 'en', // Language filter for displaying field name
                'USER_TYPE_ID' => 'double' // Field type filter
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive all numeric fields of deals with names.

```JSON
{
    "result": [
        {
            "ID": "6963",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1740471712",
            "USER_TYPE_ID": "double",
            "XML_ID": null,
            "SORT": "100",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "E",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "PRECISION": 2,
                "SIZE": 20,
                "MIN_VALUE": 0,
                "MAX_VALUE": 0,
                "DEFAULT_VALUE": null
            },
            "EDIT_FORM_LABEL": "Advance",
            "LIST_COLUMN_LABEL": "Advance",
            "LIST_FILTER_LABEL": "Advance",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "6807",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1723464314",
            "USER_TYPE_ID": "double",
            "XML_ID": null,
            "SORT": "150",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "E",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "PRECISION": 2,
                "SIZE": 20,
                "MIN_VALUE": 0,
                "MAX_VALUE": 0,
                "DEFAULT_VALUE": null
            },
            "EDIT_FORM_LABEL": "Refund Amount",
            "LIST_COLUMN_LABEL": "Refund Amount",
            "LIST_FILTER_LABEL": "Refund Amount",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        }
    ],
    "total": 2,
}
```

### 2. Modifying the Rounding Setting for the Field Value

To change the setting of an existing field, we use the method [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-update.md) with the following parameters:

- `moduleId` — the identifier of the module in which the method will modify the field, a required parameter. In the example, we modify the field for deals, the module is `crm`.

- `id` — the identifier of the custom field, a required parameter. In the example, we will pass the field ID obtained from the method [crm.deal.userfield.list](#1-getting-the-field-id).

- `field[settings]` — an array of additional settings for the field depending on its type. In the example, we will specify the `PRECISION` setting — precision. We will pass an integer equal to the number of decimal places.

{% list tabs %}

- JS
  
    ```JavaScript
    BX24.callMethod(
        'userfieldconfig.update',
        {
            moduleId: 'crm', // Module identifier
            id: 6807, // ID of the custom field
            field: {
                settings: { // Additional field settings
                        PRECISION: 3, // Number of decimal places
                    },
            }
        },
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldconfig.update',
        [
            'moduleId' => 'crm', // Module identifier
            'id' => 6807, // ID of the custom field
            'field' => [
                'settings' => [ // Additional field settings
                    'PRECISION' => 3 // Number of decimal places
                ]
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive the data of the modified field.

```JSON
{
    "result": {
        "field": {
            "id": "6807",
            "entityId": "CRM_DEAL",
            "fieldName": "UF_CRM_1723464314",
            "userTypeId": "double",
            "xmlId": null,
            "sort": "150",
            "multiple": "N",
            "mandatory": "N",
            "showFilter": "E",
            "showInList": "Y",
            "editInList": "Y",
            "isSearchable": "N",
            "settings": {
                "PRECISION": 3,
                "SIZE": 20,
                "MIN_VALUE": 0,
                "MAX_VALUE": 0,
                "DEFAULT_VALUE": null
            },
            "languageId": {
                "en": "en"
            },
            "editFormLabel": {
                "en": "Refund Amount"
            },
            "listColumnLabel": {
                "en": "Refund Amount"
            },
            "listFilterLabel": {
                "en": "Refund Amount"
            },
            "errorMessage": {
                "en": null
            },
            "helpMessage": {
                "en": null
            }
        }
    },
}
```

### Code Example

{% list tabs %}

- JS
  
    ```JavaScript
    // Function to find and update a custom field
    function updateUserField() {
        // Prompt the user for the field name
        var fieldName = prompt("Enter the field name:");

        // First method: Get a list of all custom fields of type 'double'
        BX24.callMethod(
            "crm.deal.userfield.list",
            {
                filter: {
                    LANG: 'en', // Language filter for displaying field name
                    USER_TYPE_ID: 'double' // Field type filter
                }
            },
            function(result) {
                if (result.error()) {
                    console.error(result.error());
                } else {
                    // Iterate through the retrieved fields to find the one by name
                    var fields = result.data();
                    var fieldId = null;

                    for (var i = 0; i < fields.length; i++) {
                        if (fields[i].EDIT_FORM_LABEL === fieldName) {
                            fieldId = fields[i].ID;
                            break;
                        }
                    }

                    if (fieldId) {
                        // Second method: Update the settings of the found field
                        BX24.callMethod(
                            'userfieldconfig.update',
                            {
                                moduleId: 'crm', // Module identifier
                                id: fieldId, // ID of the found custom field
                                field: {
                                    settings: { 
                                        PRECISION: 3 // Number of decimal places
                                    }
                                }
                            },
                            function(updateResult) {
                                if (updateResult.error()) {
                                    console.error(updateResult.error());
                                } else {
                                    console.log("Field settings successfully updated.");
                                }
                            }
                        );
                    } else {
                        console.log("Field with the specified name not found.");
                    }
                }
            }
        );
    }

    // Run the function
    updateUserField();
    ```

- PHP
  
    ```php
    require_once('crest.php');

    // Function to find and update a custom field
    function updateUserField($fieldName) {
        // First method: Get a list of all custom fields of type 'double'
        $result = CRest::call(
            'crm.deal.userfield.list',
            [
                'filter' => [
                    'LANG' => 'en', // Language filter for displaying field name
                    'USER_TYPE_ID' => 'double' // Field type filter
                ]
            ]
        );

        if (isset($result['error'])) {
            echo 'Error: ' . $result['error_description'];
        } else {
            // Iterate through the retrieved fields to find the one by name
            $fields = $result['result'];
            $fieldId = null;

            foreach ($fields as $field) {
                if ($field['EDIT_FORM_LABEL'] === $fieldName) {
                    $fieldId = $field['ID'];
                    break;
                }
            }

            if ($fieldId) {
                // Second method: Update the settings of the found field
                $updateResult = CRest::call(
                    'userfieldconfig.update',
                    [
                        'moduleId' => 'crm', // Module identifier
                        'id' => $fieldId, // ID of the found custom field
                        'field' => [
                            'settings' => [
                                'PRECISION' => 3 // Number of decimal places
                            ]
                        ]
                    ]
                );

                if (isset($updateResult['error'])) {
                    echo 'Error: ' . $updateResult['error_description'];
                } else {
                    echo 'Field settings successfully updated.';
                }
            } else {
                echo 'Field with the specified name not found.';
            }
        }
    }

    // Prompt the user for the field name
    $fieldName = readline("Enter the field name: ");

    // Run the function
    updateUserField($fieldName);
    ```

{% endlist %}