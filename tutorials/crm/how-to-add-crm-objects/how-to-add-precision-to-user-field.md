# How to Configure Rounding for a Custom Field of Type "Number"

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields have standard settings: name, required status, and multiple values.

Additionally, there are specialized settings depending on the field type:
- list values
- rounding precision for numbers
- currency for money fields

To retrieve specialized settings for the "Number" type — `double`, use the [crm.userfield.settings.fields](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-settings-fields.md) method:

{% list tabs %}

- JS
  
    ```JavaScript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: 'crm.userfield.settings.fields',
        params: {
            type: 'double' // user field type
        },
        requestId: 'settings-fields'
    });
    console.dir(result.getData().result);
    ```

- PHP
  
    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $sb->getCRMScope()->userfield()->settingsFields(
        'double' // User field type
    )->getFieldsDescription();

    print_r($result);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    try:
        result = client.crm.userfield.settings.fields(type="double").response.result
        print(result)
    except BitrixAPIError as error:
        print(f"Error: {error}")
    ```

{% endlist %}

As a result, you will receive two settings: default value and precision.

```JSON
{
    "result": {
        "DEFAULT_VALUE": {
            "type": "double",
            "title": "Default value"
        },
        "PRECISION": {
            "type": "int",
            "title": "Precision"
        }
    }
}
```

## Creating a Numeric Field with Rounding Configuration

We will create a field of type number with a precision setting of three decimal places. If a value with four or more decimal places is entered into the field, it will automatically be rounded to three decimal places.

To create a custom field, use the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method with the following parameters:

- `moduleId` — the module identifier in which the method will create the field, a required parameter. In this example, we are creating a field for deals, so the module is `crm`
- `field[entityId]` — the object identifier according to the formula `CRM_ + {ID}`, a required parameter. A list of object identifiers can be found in the article [User field settings](../../../api-reference/crm/universal/userfieldconfig/index.md#entity-id). In the example, we will specify `CRM_DEAL`

- `field[fieldName]` — the field code according to the formula `UF_ + {object identifier} + _ + {arbitrary string in UPPERCASE}`. The maximum length for the code is 50 characters, a required parameter. In the example, we will specify `UF_CRM_DEAL_NEW_DOUBLE_FIELD`

- `field[userTypeId]` — the [field type](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md) identifier, a required parameter. In the example, we will specify `double` to create a field of type number

- `field[editFormLabel]` — an array of names for displaying the field in Bitrix24 in different languages. An optional parameter; if no name is provided, Bitrix24 will display the field code

- `field[settings]` — an array of additional field settings depending on its type. An optional parameter; if omitted, default settings will be used. In the example, we will specify the `PRECISION` setting — precision. We will pass an integer equal to the number of decimal places.

{% list tabs %}

- JS
  
    ```JavaScript
    const result = await $b24.actions.v2.call.make({
        method: 'userfieldconfig.add',
        params: {
            moduleId: 'crm', // Module ID
            field: {
                entityId: 'CRM_DEAL', // Object ID
                fieldName: 'UF_CRM_DEAL_NEW_DOUBLE_FIELD', // Field code
                userTypeId: 'double', // Field type ID
                editFormLabel: { 
                    'de': 'Number with rounding', // Field name in German
                    'en': 'PRECISION double' // Field name in English
                },
                settings: { // Additional field settings
                        PRECISION: 3, // Number of decimal places
                    },
            }
        },
        requestId: 'userfieldconfig-add'
    });
    ```

- PHP

    ```php
    // userfieldconfig.add has no wrapper in the SDK — calling the method directly
    $result = $sb->core->call(
        'userfieldconfig.add',
        [
            'moduleId' => 'crm', // Module ID
            'field' => [
                'entityId' => 'CRM_DEAL', // Object ID
                'fieldName' => 'UF_CRM_DEAL_NEW_DOUBLE_FIELD', // Field code
                'userTypeId' => 'double', // Field type ID
                'editFormLabel' => [
                    'de' => 'Number with rounding', // Field name in German
                    'en' => 'PRECISION double' // Field name in English
                ],
                'settings' => [ // Additional field settings
                    'PRECISION' => 3 // Number of decimal places
                ]
            ]
        ]
    );
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    try:
        field = client.userfieldconfig.add(
            module_id="crm",
            field={
                "entityId": "CRM_DEAL",
                "fieldName": "UF_CRM_DEAL_NEW_DOUBLE_FIELD",
                "userTypeId": "double",
                "editFormLabel": {
                    "de": "Number with rounding",
                },
                "settings": {
                    "PRECISION": 3,
                },
            },
        ).response.result["field"]
        print(field)
    except BitrixAPIError as error:
        print(f"Error: {error}")
    ```

{% endlist %}

As a result, you will receive the data for the created field.

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
                "en": "en",
                "de": "de"
            },
            "editFormLabel": {
                "en": "PRECISION double",
                "de": "Number with rounding"
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
            }
        }
    },
}
```

## Modifying the Configuration of an Existing Numeric Field

To change the rounding configuration of an existing field, use the [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) method by specifying the field `ID`. You can obtain the field `ID` in two ways: during field creation using the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method, or via a method that retrieves a list of custom fields for an object. In this example, the field belongs to a deal, so we use the [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) method.

### 1. Retrieving the Field ID

To retrieve the field `ID`, use the [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) method with the following parameters:

- `filter[LANG]` — a language filter used to output field names in the required language. Without this filter, names will not be output.

- `filter[USER_TYPE_ID]` — a field type filter used to retrieve only fields of the "Number" type in the result.

{% list tabs %}

- JS
  
    ```JavaScript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.deal.userfield.list',
        params: {
            filter: {
                LANG: 'de', // Language filter for field name output
                USER_TYPE_ID: 'double' // Field type filter
            }
        },
        requestId: 'userfield-list'
    });
    ```

- PHP
  
    ```php
    $result = $sb->getCRMScope()->dealUserfield()->list(
        order: [],
        filter: [
            'LANG' => 'de', // Language filter for field name output
            'USER_TYPE_ID' => 'double' // Field type filter
        ]
    )->getUserfields();
    ```

- Python

    ```python
    fields = client.crm.deal.userfield.list(
        filter={
            "LANG": "de",
            "USER_TYPE_ID": "double",
        }
    ).response.result
    ```

{% endlist %}

As a result, you will receive all numeric deal fields along with their names.

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
            "EDIT_FORM_LABEL": "Advance payment",
            "LIST_COLUMN_LABEL": "Advance payment",
            "LIST_FILTER_LABEL": "Advance payment",
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
            "EDIT_FORM_LABEL": "Refund amount",
            "LIST_COLUMN_LABEL": "Refund amount",
            "LIST_FILTER_LABEL": "Refund amount",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        }
    ],
    "total": 2,
}
```

### 2. Modifying the Value Rounding Configuration in the Field

To change the configuration of an existing field, use the [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) method with the following parameters:

- `moduleId` — the module identifier in which the method will modify the field; this is a required parameter. In this example, we are modifying a deal field, so the module is `crm`.

- `id` — the custom field identifier; this is a required parameter. In this example, we pass the field `ID` obtained via the [crm.deal.userfield.list](#1-retrieving-the-field-id) method.

- `field[settings]` — an array of additional field configurations depending on its type. In this example, we specify the `PRECISION` configuration — precision. We pass an integer representing the number of decimal places.

{% list tabs %}

- JS
  
    ```JavaScript
    const result = await $b24.actions.v2.call.make({
        method: 'userfieldconfig.update',
        params: {
            moduleId: 'crm', // Module ID
            id: 6807, // user field id
            field: {
                settings: { // Additional field settings
                        PRECISION: 3, // Number of decimal places
                    },
            }
        },
        requestId: 'userfieldconfig-update'
    });
    ```

- PHP
  
    ```php
    // userfieldconfig.update has no wrapper in the SDK — calling the method directly
    $result = $sb->core->call(
        'userfieldconfig.update',
        [
            'moduleId' => 'crm', // Module ID
            'id' => 6807, // User field ID
            'field' => [
                'settings' => [ // Additional field settings
                    'PRECISION' => 3 // Number of decimal places
                ]
            ]
        ]
    );
    ```

- Python

    ```python
    field = client.userfieldconfig.update(
        module_id="crm",
        bitrix_id=6807,
        field={
            "settings": {
                "PRECISION": 3,
            }
        },
    ).response.result["field"]
    ```

{% endlist %}

As a result, you will receive the data for the modified field.

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
                "de": "de"
            },
            "editFormLabel": {
                "de": "Refund amount"
            },
            "listColumnLabel": {
                "de": "Refund amount"
            },
            "listFilterLabel": {
                "de": "Refund amount"
            },
            "errorMessage": {
                "de": null
            },
            "helpMessage": {
                "de": null
            }
        }
    },
}
```

### Code Example

{% list tabs %}

- JS
  
    ```JavaScript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Function to search and update a user field
    async function updateUserField() {
        // Requesting the field name from the user
        var fieldName = prompt("Enter field name:");

        try {
            // First method: Get a list of all user fields of type 'double'
            const result = await $b24.actions.v2.call.make({
                method: 'crm.deal.userfield.list',
                params: {
                    filter: {
                        LANG: 'de', // Language filter for field name output
                        USER_TYPE_ID: 'double' // Field type filter
                    }
                },
                requestId: 'userfield-list'
            });

            // Iterating through the retrieved fields to find the one matching the name
            var fields = result.getData().result;
            var fieldId = null;

            for (var i = 0; i < fields.length; i++) {
                if (fields[i].EDIT_FORM_LABEL === fieldName) {
                    fieldId = fields[i].ID;
                    break;
                }
            }

            if (fieldId) {
                // Second method: Update the settings of the found field
                await $b24.actions.v2.call.make({
                    method: 'userfieldconfig.update',
                    params: {
                        moduleId: 'crm', // Module ID
                        id: fieldId, // Found user field ID
                        field: {
                            settings: { 
                                PRECISION: 3 // Number of decimal places
                            }
                        }
                    },
                    requestId: 'userfieldconfig-update'
                });
                console.log("Field settings updated successfully.");
            } else {
                console.log("Field with the specified name not found.");
            }
        } catch (error) {
            console.error(error);
        }
    }

    // Running function
    updateUserField();
    ```

- PHP
  
    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Bitrix24\SDK\Services\ServiceBuilder;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Function to search and update a user field
    function updateUserField(ServiceBuilder $sb, $fieldName) {
        try {
            // First method: Get a list of all user fields of type 'double'
            $fields = $sb->getCRMScope()->dealUserfield()->list(
                order: [],
                filter: [
                    'LANG' => 'de', // Language filter for field name output
                    'USER_TYPE_ID' => 'double' // Field type filter
                ]
            )->getUserfields();

            // Iterating through the retrieved fields to find the one matching the name
            $fieldId = null;
            foreach ($fields as $field) {
                if ($field->EDIT_FORM_LABEL === $fieldName) {
                    $fieldId = $field->ID;
                    break;
                }
            }

            if ($fieldId) {
                // Second method: Update the settings of the found field
                // userfieldconfig.update has no wrapper in the SDK — calling the method directly
                $sb->core->call(
                    'userfieldconfig.update',
                    [
                        'moduleId' => 'crm', // Module ID
                        'id' => $fieldId, // Found user field ID
                        'field' => [
                            'settings' => [
                                'PRECISION' => 3 // Number of decimal places
                            ]
                        ]
                    ]
                );
                echo 'Field settings updated successfully.';
            } else {
                echo 'Field with the specified name not found.';
            }
        } catch (\Throwable $e) {
            echo 'Error: ' . $e->getMessage();
        }
    }

    // Requesting the field name from the user
    $fieldName = readline("Enter field name: ");

    // Running function
    updateUserField($sb, $fieldName);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    def update_user_field(client, field_name: str) -> None:
        try:
            fields = client.crm.deal.userfield.list(
                filter={
                    "LANG": "de",
                    "USER_TYPE_ID": "double",
                }
            ).response.result
        except BitrixAPIError as error:
            print(f"Error: {error}")
            return

        field_id = None
        for field in fields:
            if field["EDIT_FORM_LABEL"] == field_name:
                field_id = int(field["ID"])
                break

        if field_id is None:
            print("Field with the specified name not found.")
            return

        try:
            client.userfieldconfig.update(
                module_id="crm",
                bitrix_id=field_id,
                field={
                    "settings": {
                        "PRECISION": 3
                    }
                },
            ).response
        except BitrixAPIError as error:
                print(f"Error: {error}")
        else:
            print("Field settings updated successfully.")

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    field_name = input("Enter field name: ")
    update_user_field(client, field_name)
    ```

{% endlist %}
