# How to Configure Rounding for a Number Custom Field

> Scope: [`crm`, `userfieldconfig`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed permissions is required — "Allow changing settings"
>
> - [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) and [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) — a user with "Allow changing settings" permission in CRM
> - [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) — a user with permission to read deals

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields have standard configurations: name, mandatory status, and multiple values.

Additionally, there are specialized configurations, the set of which depends on the field type:

- list values
- rounding precision for numbers
- currency for money fields

For the "Number" type — `double` — precision is defined by the `PRECISION` setting. This is an integer from 0 to 12: it represents the number of decimal places. Bitrix24 rounds the value at the moment of saving. For example, when `PRECISION`: `3` the entered `1,23456` will be saved as `1.235`.

## Choose a Scenario

There are two independent scenarios on this page. They are not connected: the second does not use the result of the first, but begins by searching for an existing field.

- [Creating a field with rounding configuration immediately](#create) — one call to [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md). Suitable when the field does not yet exist.
- [Changing the configuration of an existing field](#update) — two steps: retrieve the `ID` of the field using the [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) method, then pass it to [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md).

In both scenarios, the examples work with deal fields. For another CRM object, the object identifier in `entityId` and the method to retrieve the field list will change — for example, [crm.lead.userfield.list](../../../api-reference/crm/leads/userfield/crm-lead-userfield-list.md) for leads.

## Prepare the Data

To run the examples, you need:

- An incoming webhook with scopes `crm` and `userfieldconfig`. The webhook executes requests with the permissions of the user who created it. Do not publish the secret webhook code in client-side code or repositories — store it in environment variables, as shown in the JS example. In the PHP and Python examples, a placeholder is used instead of the webhook address; replace it with your own method for storing the secret.
- The "Allow to change settings" permission for the webhook user. This is a general permission for CRM configurations: it is granted to a role as a whole and is not set separately for deals or other objects. The exception is SPAs within an automated solution: for these, the permission is checked at the level of the solution itself. Without this permission, [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) and [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) will return an access error.

For server-side JS examples with `B24Hook`, Node.js 18, 20, 22, or newer is required; for new projects, 22 or newer is required. B24JsSDK is an ES module: save the code in a `.mjs` file or add `"type": "module"` to `package.json`. For examples using `b24pysdk`, Python 3.9 or newer is required; for version 3 examples using [B24PhpSDK](../../../sdk/b24phpsdk/index.md), PHP 8.4 or newer is required.

{% include [Note on examples](../../../_includes/examples.md) %}

The step examples follow one another. The SDK is initialized once here; subsequent examples use the ready-made instance: `$b24` in JS, `$sb` in PHP, and `client` in Python.

{% list tabs %}

- JS
  
    ```JavaScript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'
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
    ```

- Go

    ```go
    // The webhook path is a secret, so it comes from the environment, not from the code.
    // The client is built once per portal: it holds the HTTP client and the
    // authorization state.
    core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()
    ```

{% endlist %}

## Note the Different Field Casing {#case}

The methods on this page return the same data in different casing. This is how the API works — there is no need to convert formats to match each other, but they can be easily confused in code.

- `userfieldconfig.*` methods accept and return fields in camelCase: `fieldName`, `userTypeId`, `editFormLabel`, `settings`
- `crm.*.userfield.*` methods, including [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md), return fields in UPPER_SNAKE: `FIELD_NAME`, `USER_TYPE_ID`, `EDIT_FORM_LABEL`, `SETTINGS`

Keys within the configurations themselves are in uppercase in both cases: `PRECISION`, `SIZE`, `MIN_VALUE`, `MAX_VALUE`, `DEFAULT_VALUE`. Therefore, the precision of the created field lies in `settings.PRECISION`, while the precision of the field from the deal list lies in `SETTINGS.PRECISION`.

## Creating a Field with Rounding Configuration Immediately {#create}

We will create a deal field with a "Number" type and a precision of three decimal places. If a value with four or more decimal places is entered into such a field, it will be rounded to three decimal places upon saving.

To create a custom field, use the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method with the following parameters:

- `moduleId` — the module identifier where the method will create the field, a required parameter. In the example, we are creating a field for deals, so the module is `crm`

- `field[entityId]` — the object identifier, a required parameter. For deals and other base CRM objects, the identifier is fixed: `CRM_DEAL`, `CRM_LEAD`, `CRM_CONTACT`, `CRM_COMPANY`. The format `CRM_{ID}` with a numeric identifier is used only for custom SPAs, while system objects use their own string identifiers such as `CRM_SMART_INVOICE`. A complete list can be found in the article [User Field Settings](../../../api-reference/crm/universal/userfieldconfig/index.md#entity-id). In the example, we will specify `CRM_DEAL`

- `field[fieldName]` — the field code according to the formula `UF_ + {object identifier} + _ + {arbitrary string in UPPERCASE}`. A required parameter. The code must start with `UF_` and the object identifier from `entityId`, otherwise the method will return an error. Allowed characters are `A-Z`, `0-9`, and `_`, with a length limit of 50 characters. In the example, we will specify `UF_CRM_DEAL_NEW_DOUBLE_FIELD`

- `field[userTypeId]` — the [field type](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md) identifier, a required parameter. In the example, we will specify `double` to create a field of the number type

- `field[editFormLabel]` — an array of names to display the field in Bitrix24 in different languages. An optional parameter; if no name is provided, Bitrix24 will display the field code

- `field[settings]` — an array of additional field configurations depending on its type. In the example, we will specify the `PRECISION` configuration — precision. We will pass an integer equal to the number of decimal places. This parameter is optional, but it is recommended to pass it for the "Number" type: without it, the precision will be `0` and values will be rounded to integers

{% list tabs %}

- JS
  
    ```JavaScript
    const addResponse = await $b24.actions.v2.call.make({
        method: 'userfieldconfig.add',
        params: {
            moduleId: 'crm', // Module identifier
            field: {
                entityId: 'CRM_DEAL', // Object identifier
                fieldName: 'UF_CRM_DEAL_NEW_DOUBLE_FIELD', // Field code
                userTypeId: 'double', // Field type identifier
                editFormLabel: {
                    'de': 'Number with rounding', // Field name in Russian
                    'en': 'PRECISION double' // Field name in English
                },
                settings: { // Additional field settings
                    PRECISION: 3 // Number of decimal places
                }
            }
        },
        requestId: 'userfieldconfig-add'
    });

    if (!addResponse.isSuccess) {
        throw new Error(addResponse.getErrorMessages().join('; '))
    }

    const createdField = addResponse.getData().result.field;
    console.log(createdField.id, createdField.settings.PRECISION);
    ```

- PHP
  
    ```php
    // userfieldconfig.add has no typed wrapper in the SDK — calling the method via the core
    $createdField = $sb->core->call(
        'userfieldconfig.add',
        [
            'moduleId' => 'crm', // Module identifier
            'field' => [
                'entityId' => 'CRM_DEAL', // Object identifier
                'fieldName' => 'UF_CRM_DEAL_NEW_DOUBLE_FIELD', // Field code
                'userTypeId' => 'double', // Field type identifier
                'editFormLabel' => [
                    'de' => 'Number with rounding', // Field name in Russian
                    'en' => 'PRECISION double' // Field name in English
                ],
                'settings' => [ // Additional field settings
                    'PRECISION' => 3 // Number of decimal places
                ]
            ]
        ]
    )->getResponseData()->getResult()['field'];

    echo $createdField['id'] . ': ' . $createdField['settings']['PRECISION'];
    ```

- Python

    ```python
    try:
        created_field = client.userfieldconfig.add(
            module_id="crm",
            field={
                "entityId": "CRM_DEAL",
                "fieldName": "UF_CRM_DEAL_NEW_DOUBLE_FIELD",
                "userTypeId": "double",
                "editFormLabel": {
                    "de": "Number with rounding",
                    "en": "PRECISION double",
                },
                "settings": {
                    "PRECISION": 3,
                },
            },
        ).response.result["field"]
    except BitrixAPIError as error:
        print(f"Error: {error}")
    else:
        print(created_field["id"], created_field["settings"]["PRECISION"])
    ```

- Go

    ```go
    res, err := core.Call(ctx, "userfieldconfig.add", b24.Params{
    	"moduleId": "crm",
    	"field": b24.Params{
    		"entityId":   "CRM_DEAL",
    		"fieldName":  fieldName,
    		"userTypeId": "double",
    		"editFormLabel": b24.Params{
    			"ru": fieldLabel,
    			"en": "PRECISION double",
    		},
    		// The parameter is optional, but for the "Number" type it is better to
    		// pass it: without it the precision will be 0 and the values will be rounded
    		// to whole numbers.
    		"settings": b24.Params{"PRECISION": 3},
    	},
    })
    if err != nil {
    	// The error code is compared with errors.Is rather than as a string: a typo in the
    	// literal would compile and silently take a different branch.
    	if errors.Is(err, b24.ErrAccessDenied) {
    		return fmt.Errorf("the \"Allow to change settings\" permission is required in CRM: %w", err)
    	}
    	return fmt.Errorf("userfieldconfig.add: %w", err)
    }

    // The method wraps the response in an object with the field key and responds in camelCase:
    // the precision is in settings.PRECISION rather than in SETTINGS.PRECISION.
    var added struct {
    	Field struct {
    		ID       b24.ID         `json:"id"`
    		Settings map[string]any `json:"settings"`
    	} `json:"field"`
    }
    if err := json.Unmarshal(res.Result, &added); err != nil {
    	return fmt.Errorf("parse the created field: %w", err)
    }
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
    }
}
```

The response confirms the result: `settings.PRECISION` contains the passed precision `3`, and other type settings are filled with default values. Save the `id` of the field — `6961` in the example. You can use it to update the field using the [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) method or delete it using the [userfieldconfig.delete](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-delete.md) method without requesting the field list again.

This concludes the first scenario. Go to the [Verify the Result](#check) section — the second scenario is only required for fields that already exist.

## Modifying a Setting for an Existing Field {#update}

This scenario is independent of the first one: the field has already been created, and its precision needs to be changed. The [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) method accepts `id` fields, so the scenario consists of two steps.

1. Retrieve the `ID` and current field settings using the [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) method.
2. Pass them to [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md), changing only the precision.

If the field was just created by the first scenario, the first step is unnecessary: both `id` and the settings already arrived in the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) response — in `field.id` and `field.settings`. Note that in this response, they are in camelCase.

### 1. Retrieving the Field ID {#field-id}

To retrieve the field ID, use the [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) method with the following parameters:

- `filter[LANG]` — a language filter used to output field names in the required language. Without this filter, names are not returned at all, making it impossible to find a field by name.

- `filter[USER_TYPE_ID]` — a field type filter used to ensure only fields with the "Number" type are returned in the result.

The method returns fields in UPPER_SNAKE case — keep this in mind when [ comparing its response with the responses from `userfieldconfig.*`](#case).

{% list tabs %}

- JS
  
    ```JavaScript
    const listResponse = await $b24.actions.v2.call.make({
        method: 'crm.deal.userfield.list',
        params: {
            filter: {
                LANG: 'de', // Language filter for field name output
                USER_TYPE_ID: 'double' // Field type filter
            }
        },
        requestId: 'userfield-list'
    });

    if (!listResponse.isSuccess) {
        throw new Error(listResponse.getErrorMessages().join('; '))
    }

    // Select the required field by name — replace with your field name
    const targetField = listResponse.getData().result
        .find(field => field.EDIT_FORM_LABEL === 'Amount to return');

    if (!targetField) {
        throw new Error('Field with the specified name not found')
    }
    ```

- PHP
  
    ```php
    $fields = $sb->getCRMScope()->dealUserfield()->list(
        order: [],
        filter: [
            'LANG' => 'de', // Language filter for field name output
            'USER_TYPE_ID' => 'double' // Field type filter
        ]
    )->getUserfields();

    // Select the required field by name — replace with your field name
    $targetField = null;
    foreach ($fields as $field) {
        if ($field->EDIT_FORM_LABEL === 'Amount to return') {
            $targetField = $field;
            break;
        }
    }

    if ($targetField === null) {
        throw new \RuntimeException('Field with the specified name not found');
    }
    ```

- Python

    ```python
    fields = client.crm.deal.userfield.list(
        filter={
            "LANG": "de",
            "USER_TYPE_ID": "double",
        }
    ).response.result

    # Select the required field by name — replace with your field name
    target_field = next(
        (field for field in fields if field["EDIT_FORM_LABEL"] == "Amount to return"),
        None,
    )

    if target_field is None:
        raise RuntimeError("Field with the specified name not found")
    ```

- Go

    ```go
    // Without the LANG filter the titles are not returned at all, and finding a field by its title
    // will not work.
    res, err = core.Call(ctx, "crm.deal.userfield.list", b24.Params{
    	"filter": b24.Params{"LANG": "ru", "USER_TYPE_ID": "double"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.deal.userfield.list: %w", err)
    }

    // And this method responds in UPPER_SNAKE — the same data, a different case.
    // The keys inside the settings themselves are uppercase in both cases.
    var fields []struct {
    	ID            b24.ID         `json:"ID"`
    	FieldName     string         `json:"FIELD_NAME"`
    	EditFormLabel string         `json:"EDIT_FORM_LABEL"`
    	Settings      map[string]any `json:"SETTINGS"`
    }
    if err := json.Unmarshal(res.Result, &fields); err != nil {
    	return fmt.Errorf("parse custom fields: %w", err)
    }

    target := -1
    for i, f := range fields {
    	if f.EditFormLabel == fieldLabel {
    		target = i
    		break
    	}
    }
    if target < 0 {
    	return fmt.Errorf("field %q not found", fieldLabel)
    }
    ```

{% endlist %}

As a result, we obtain all numeric deal fields along with their names.

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
            "EDIT_FORM_LABEL": "Amount to return",
            "LIST_COLUMN_LABEL": "Amount to return",
            "LIST_FILTER_LABEL": "Amount to return",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        }
    ],
    "total": 2
}
```

Two values from the selected field are required from the response:

- `ID` — this will be passed to `id` of the next call. In the example, this is the `6807` of the "Amount to Refund" field.
- `SETTINGS` — the current field settings. These will be passed back to change only the precision without resetting other settings. Currently, `2` is set in `SETTINGS.PRECISION`.

The field `EDIT_FORM_LABEL` is the name used to search for the field in the list. It is returned as a string only because `LANG` was passed in the filter.

### 2. Modifying the Rounding Setting {#precision}

To change a setting for an existing field, use the [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) method with the following parameters:

- `moduleId` — the module identifier in which the method will modify the field; this is a required parameter. In the example, we are modifying a deal field, and the module is `crm`.

- `id` — the custom field identifier; this is a required parameter. In the example, we pass the `ID` from [step 1](#field-id) — `6807`.

- `field[settings]` — an array of additional field settings depending on its type. In the example, we specify the `PRECISION` setting — precision. We pass an integer representing the number of decimal places.

{% note warning "" %}

`field[settings]` replaces the entire set of settings rather than appending the passed keys to the existing ones. If you pass only `PRECISION`, other "Number" type settings will be reset to default values: `SIZE` — `20`, `MIN_VALUE` and `MAX_VALUE` — `0`, `DEFAULT_VALUE` — `null`. Therefore, in the examples, we take `SETTINGS` from step 1 and change only the precision within them.

Reducing the precision of a populated field is risky: extra characters are not hidden, but are discarded during the next deal save. It is no longer possible to restore them by setting the previous value `PRECISION`.

See the full set of type configurations in the `settings` of any `userfieldconfig.*` response — all five keys are located there. The [crm.userfield.settings.fields](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-settings-fields.md) method for type `double` lists only `DEFAULT_VALUE` and `PRECISION`, so it cannot be relied upon in this context.

{% endnote %}

{% list tabs %}

- JS
  
    ```JavaScript
    const updateResponse = await $b24.actions.v2.call.make({
        method: 'userfieldconfig.update',
        params: {
            moduleId: 'crm', // Module identifier
            id: Number(targetField.ID), // Field ID from step 1
            field: {
                settings: { // Additional field settings
                    ...targetField.SETTINGS, // Transfer current settings from step 1
                    PRECISION: 3 // Number of decimal places
                }
            }
        },
        requestId: 'userfieldconfig-update'
    });

    if (!updateResponse.isSuccess) {
        throw new Error(updateResponse.getErrorMessages().join('; '))
    }

    const updatedField = updateResponse.getData().result.field;
    console.log(updatedField.id, updatedField.settings.PRECISION);
    ```

- PHP
  
    ```php
    // userfieldconfig.update has no typed wrapper in the SDK — calling the method via the core
    $updatedField = $sb->core->call(
        'userfieldconfig.update',
        [
            'moduleId' => 'crm', // Module identifier
            'id' => (int)$targetField->ID, // Field ID from step 1
            'field' => [
                'settings' => array_merge(
                    (array)$targetField->SETTINGS, // Transfer current settings from step 1
                    ['PRECISION' => 3] // Number of decimal places
                )
            ]
        ]
    )->getResponseData()->getResult()['field'];

    echo $updatedField['id'] . ': ' . $updatedField['settings']['PRECISION'];
    ```

- Python

    ```python
    updated_field = client.userfieldconfig.update(
        module_id="crm",
        bitrix_id=int(target_field["ID"]),
        field={
            "settings": {
                **target_field["SETTINGS"],  # Transfer current settings from step 1
                "PRECISION": 3,
            }
        },
    ).response.result["field"]

    print(updated_field["id"], updated_field["settings"]["PRECISION"])
    ```

- Go

    ```go
    // settings are REPLACED as a whole rather than appended to. If you pass one
    // PRECISION, the remaining settings of the "Number" type will reset to their default
    // values — that is why the settings from step 1 are taken and one of them is changed.
    settings := fields[target].Settings
    settings["PRECISION"] = 5

    res, err = core.Call(ctx, "userfieldconfig.update", b24.Params{
    	"moduleId": "crm",
    	"id":       fields[target].ID,
    	"field":    b24.Params{"settings": settings},
    })
    if err != nil {
    	return fmt.Errorf("userfieldconfig.update: %w", err)
    }

    var updated struct {
    	Field struct {
    		ID       b24.ID         `json:"id"`
    		Settings map[string]any `json:"settings"`
    	} `json:"field"`
    }
    if err := json.Unmarshal(res.Result, &updated); err != nil {
    	return fmt.Errorf("parse the updated field: %w", err)
    }
    ```

{% endlist %}

As a result, we obtain the data for the modified field.

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
                "de": "Amount to return"
            },
            "listColumnLabel": {
                "de": "Amount to return"
            },
            "listFilterLabel": {
                "de": "Amount to return"
            },
            "errorMessage": {
                "de": null
            },
            "helpMessage": {
                "de": null
            }
        }
    }
}
```

The method returned the field in camelCase, so the new precision is located in `settings.PRECISION`, rather than in `SETTINGS.PRECISION`, as in the response of step 1. The value changed from `2` to `3` — the scenario is complete.

`languageId` lists the languages for which the field has defined Salutations — this set is defined when creating and modifying the field, not by the portal. Therefore, the field in the example only has `ru`, while for the field created in the first scenario with `editFormLabel` in two languages, there will be `ru` and `en`. The set of languages does not depend on the `LANG` filter from step 1.

## Code Example {#example}

The example assembles the second scenario in its entirety: it finds a deal field by name and changes its precision. The field name is defined as a constant at the beginning — replace it with the name of your own field.

{% list tabs %}

- JS
  
    ```JavaScript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const FIELD_LABEL = 'Amount to return' // Field name to be changed
    const PRECISION = 3 // Number of decimal places

    async function updateUserField() {
        try {
            // Step 1: get deal user fields of type double
            const listResponse = await $b24.actions.v2.call.make({
                method: 'crm.deal.userfield.list',
                params: {
                    filter: {
                        LANG: 'de', // Language filter for field name output
                        USER_TYPE_ID: 'double' // Field type filter
                    }
                },
                requestId: 'userfield-list'
            });

            if (!listResponse.isSuccess) {
                throw new Error(listResponse.getErrorMessages().join('; '))
            }

            // The crm.deal.userfield.list response comes in UPPER_SNAKE
            const targetField = listResponse.getData().result
                .find(field => field.EDIT_FORM_LABEL === FIELD_LABEL);

            if (!targetField) {
                throw new Error('Field with the specified name not found')
            }

            // Step 2: update settings of the found field
            const updateResponse = await $b24.actions.v2.call.make({
                method: 'userfieldconfig.update',
                params: {
                    moduleId: 'crm', // Module identifier
                    id: Number(targetField.ID), // ID of the found user field
                    field: {
                        settings: {
                            ...targetField.SETTINGS, // Transfer current field settings
                            PRECISION // Number of decimal places
                        }
                    }
                },
                requestId: 'userfieldconfig-update'
            });

            if (!updateResponse.isSuccess) {
                throw new Error(updateResponse.getErrorMessages().join('; '))
            }

            // The userfieldconfig.update response comes in camelCase
            console.log('Field precision:', updateResponse.getData().result.field.settings.PRECISION);
        } catch (error) {
            console.error(error);
        }
    }

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

    const FIELD_LABEL = 'Amount to return'; // Field name to be changed
    const PRECISION = 3; // Number of decimal places

    function updateUserField(ServiceBuilder $sb, string $fieldLabel, int $precision): void {
        try {
            // Step 1: get deal user fields of type double
            $fields = $sb->getCRMScope()->dealUserfield()->list(
                order: [],
                filter: [
                    'LANG' => 'de', // Language filter for field name output
                    'USER_TYPE_ID' => 'double' // Field type filter
                ]
            )->getUserfields();

            // The crm.deal.userfield.list response comes in UPPER_SNAKE
            $targetField = null;
            foreach ($fields as $field) {
                if ($field->EDIT_FORM_LABEL === $fieldLabel) {
                    $targetField = $field;
                    break;
                }
            }

            if ($targetField === null) {
                throw new \RuntimeException('Field with the specified name not found');
            }

            // Step 2: update settings of the found field
            // userfieldconfig.update has no typed wrapper in the SDK — calling the method via the core
            $updatedField = $sb->core->call(
                'userfieldconfig.update',
                [
                    'moduleId' => 'crm', // Module identifier
                    'id' => (int)$targetField->ID, // ID of the found user field
                    'field' => [
                        'settings' => array_merge(
                            (array)$targetField->SETTINGS, // Transfer current field settings
                            ['PRECISION' => $precision] // Number of decimal places
                        )
                    ]
                ]
            )->getResponseData()->getResult()['field'];

            // The userfieldconfig.update response comes in camelCase
            echo 'Field precision: ' . $updatedField['settings']['PRECISION'];
        } catch (\Throwable $e) {
            echo 'Error: ' . $e->getMessage();
        }
    }

    updateUserField($sb, FIELD_LABEL, PRECISION);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    FIELD_LABEL = "Amount to return"  # Field name to be changed
    PRECISION = 3  # Number of decimal places

    def update_user_field(client, field_label: str, precision: int) -> None:
        try:
            # Step 1: get deal user fields of type double
            fields = client.crm.deal.userfield.list(
                filter={
                    "LANG": "de",
                    "USER_TYPE_ID": "double",
                }
            ).response.result

            # The crm.deal.userfield.list response comes in UPPER_SNAKE
            target_field = next(
                (field for field in fields if field["EDIT_FORM_LABEL"] == field_label),
                None,
            )

            if target_field is None:
                raise RuntimeError("Field with the specified name not found")

            # Step 2: update settings of the found field
            updated_field = client.userfieldconfig.update(
                module_id="crm",
                bitrix_id=int(target_field["ID"]),
                field={
                    "settings": {
                        **target_field["SETTINGS"],  # Transfer current field settings
                        "PRECISION": precision,
                    }
                },
            ).response.result["field"]
        except (BitrixAPIError, RuntimeError) as error:
            print(f"Error: {error}")
        else:
            # The userfieldconfig.update response comes in camelCase
            print("Field precision:", updated_field["settings"]["PRECISION"])

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    update_user_field(client, FIELD_LABEL, PRECISION)
    ```

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    //
    // The example is self-contained: it creates a deal field with a precision of 2, finds it
    // by title, changes the precision to 3, and deletes the field afterwards. The second scenario
    // of the page requires an already existing field — the example prepares it in the first
    // scenario, so it runs on any portal and nothing needs to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"errors"
    	"fmt"
    	"log"
    	"os"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    const (
    	fieldName  = "UF_CRM_DEAL_NEW_DOUBLE_FIELD"
    	fieldLabel = "Rounded number"
    )

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	// The client is built once per portal: it holds the HTTP client and the
    	// authorization state.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()
    	// --- scenario 1: create the field with the precision right away
    	res, err := core.Call(ctx, "userfieldconfig.add", b24.Params{
    		"moduleId": "crm",
    		"field": b24.Params{
    			"entityId":   "CRM_DEAL",
    			"fieldName":  fieldName,
    			"userTypeId": "double",
    			"editFormLabel": b24.Params{
    				"ru": fieldLabel,
    				"en": "PRECISION double",
    			},
    			// The parameter is optional, but for the "Number" type it is better to
    			// pass it: without it the precision will be 0 and the values will be rounded
    			// to whole numbers.
    			"settings": b24.Params{"PRECISION": 3},
    		},
    	})
    	if err != nil {
    		// The error code is compared with errors.Is rather than as a string: a typo in the
    		// literal would compile and silently take a different branch.
    		if errors.Is(err, b24.ErrAccessDenied) {
    			return fmt.Errorf("the \"Allow to change settings\" permission is required in CRM: %w", err)
    		}
    		return fmt.Errorf("userfieldconfig.add: %w", err)
    	}

    	// The method wraps the response in an object with the field key and responds in camelCase:
    	// the precision is in settings.PRECISION rather than in SETTINGS.PRECISION.
    	var added struct {
    		Field struct {
    			ID       b24.ID         `json:"id"`
    			Settings map[string]any `json:"settings"`
    		} `json:"field"`
    	}
    	if err := json.Unmarshal(res.Result, &added); err != nil {
    		return fmt.Errorf("parse the created field: %w", err)
    	}
    	defer del(ctx, core, "userfieldconfig.delete", b24.Params{
    		"moduleId": "crm", "id": added.Field.ID,
    	})
    	fmt.Printf("field %d created, PRECISION=%v\n", added.Field.ID, added.Field.Settings["PRECISION"])

    	// --- scenario 2, step 1: find the field by its title
    	// Without the LANG filter the titles are not returned at all, and finding a field by its title
    	// will not work.
    	res, err = core.Call(ctx, "crm.deal.userfield.list", b24.Params{
    		"filter": b24.Params{"LANG": "ru", "USER_TYPE_ID": "double"},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.deal.userfield.list: %w", err)
    	}

    	// And this method responds in UPPER_SNAKE — the same data, a different case.
    	// The keys inside the settings themselves are uppercase in both cases.
    	var fields []struct {
    		ID            b24.ID         `json:"ID"`
    		FieldName     string         `json:"FIELD_NAME"`
    		EditFormLabel string         `json:"EDIT_FORM_LABEL"`
    		Settings      map[string]any `json:"SETTINGS"`
    	}
    	if err := json.Unmarshal(res.Result, &fields); err != nil {
    		return fmt.Errorf("parse custom fields: %w", err)
    	}

    	target := -1
    	for i, f := range fields {
    		if f.EditFormLabel == fieldLabel {
    			target = i
    			break
    		}
    	}
    	if target < 0 {
    		return fmt.Errorf("field %q not found", fieldLabel)
    	}
    	fmt.Printf("found field %d (%s), currently PRECISION=%v\n",
    		fields[target].ID, fields[target].FieldName, fields[target].Settings["PRECISION"])

    	// --- scenario 2, step 2: change the precision
    	// settings are REPLACED as a whole rather than appended to. If you pass one
    	// PRECISION, the remaining settings of the "Number" type will reset to their default
    	// values — that is why the settings from step 1 are taken and one of them is changed.
    	settings := fields[target].Settings
    	settings["PRECISION"] = 5

    	res, err = core.Call(ctx, "userfieldconfig.update", b24.Params{
    		"moduleId": "crm",
    		"id":       fields[target].ID,
    		"field":    b24.Params{"settings": settings},
    	})
    	if err != nil {
    		return fmt.Errorf("userfieldconfig.update: %w", err)
    	}

    	var updated struct {
    		Field struct {
    			ID       b24.ID         `json:"id"`
    			Settings map[string]any `json:"settings"`
    		} `json:"field"`
    	}
    	if err := json.Unmarshal(res.Result, &updated); err != nil {
    		return fmt.Errorf("parse the updated field: %w", err)
    	}
    	fmt.Printf("field %d: PRECISION=%v, the remaining settings are retained: %v\n",
    		updated.Field.ID, updated.Field.Settings["PRECISION"], updated.Field.Settings)
    	return nil
    }

    // del removes what was created. A cleanup error is printed but not returned: it must not
    // mask the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, params b24.Params) {
    	if _, err := core.Call(ctx, method, params); err != nil {
    		fmt.Fprintf(os.Stderr, "cleanup, %s: %v
", method, err)
    	}
    }
    ```

{% endlist %}

## Verify the Result {#check}

The scenario is executed correctly if the method response contains:

- For [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md), `field.id` is present, but `field.settings.PRECISION` contains the passed precision
- For [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md), `field.settings.PRECISION` contains the new precision, and `field.id` matches the identifier from step 1

The [userfieldconfig.get](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-get.md) method returns the current field configurations at any time using the `moduleId` parameters: `crm` and `id` of the field. It returns data in the same camelCase format as `add` and `update`.

You can verify rounding with data as follows: open a deal card — the field is displayed with the name from `editFormLabel`. Enter a value with more decimal places than specified in `PRECISION`, and save the deal. Via REST, the same is done by methods [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) and [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md): write to the field `1,23456` and read it — with `PRECISION`: `3`, `1.235` will be returned.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Error** | **Cause and action** ||
|| You cannot create custom fields | The user does not have the "Allow changing settings" permission in CRM, or an object was passed in `field[entityId]` to which there is no access, or the field type from `field[userTypeId]` is prohibited from being created via REST. Check which user the incoming webhook was created on behalf of ||
|| Incorrect field code | `field[fieldName]` does not start with `UF_` and the object identifier from `field[entityId]`. For `CRM_DEAL` the code must start with `UF_CRM_DEAL_`. The prefix is case-sensitive, so `uf_crm_deal_...` will not work. This same error occurs if `field[fieldName]` is not passed at all. If the prefix is correct, but the code contains lowercase letters, Cyrillic, or characters outside of `A-Z`, `0-9`, and `_`, the `Field name contains invalid characters...` error will be returned; if the code is longer than 50 characters, the `Field name is too long...` error will be returned ||
|| `The field #FIELD_NAME# for object #ENTITY_ID# already exists.` | An object already has a field with this `field[fieldName]`; instead of `#FIELD_NAME#` and `#ENTITY_ID#`, the passed values are substituted. You do not need to create the field again — go to the [second scenario](#update) and change the precision of the existing field ||
|| You cannot change the custom field settings | Insufficient permissions to change the field. This same error occurs if the field with the passed `id` has been deleted or belongs to a different module than the one specified in `moduleId`. The original Bitrix24 error message contains a typo in the word "custom"; search for the error using the exact string returned by the API ||
|| `The current method required more scopes. (crm)` | The incoming webhook or application does not have the module scope from `moduleId`. For CRM, both scopes are required: `userfieldconfig` and `crm` ||
|| `Access denied.` | Error [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md): the user does not have permission to read deals ||
|#

The method may execute without an error, but the result may not be what you expected.

- the [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) response does not contain field labels — `LANG` was not passed in `filter`. Without it, labels are not returned at all, and searching for a field by name will not work
- the field was not found in the response — check `USER_TYPE_ID`: for the "Number" type, it is `double`, not `integer` or `money`
- the precision changed, but other field configurations were reset — only `PRECISION` was passed in `field[settings]`. Repeat step 2, passing the configurations from step 1 in their entirety
- the value in the field was not rounded — rounding is applied when saving a value, not when changing a configuration. Previously saved values are not recalculated

The steps of the second scenario are repeated independently: step 1 does not change anything and can be executed any number of times. If step 2 returns an error, the configuration has not changed — correct the parameters and repeat only step 2.

## Key Considerations

- `PRECISION` accepts an integer from 0 to 12. Bitrix24 does not reject values outside this range but snaps them to the boundary: a negative value becomes `0`, and more than 12 becomes `12`
- field values are accepted with both dots and commas; spaces are removed
- `MIN_VALUE` and `MAX_VALUE`, equal to `0`, mean there is no limit
- [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) does not change `entityId`, `fieldName`, `userTypeId`, and `multiple` — these parameters are ignored. To change them, delete the field using the [userfieldconfig.delete](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-delete.md) method and create it again
- `update` language labels are overwritten in the same way as configurations: if `field[editFormLabel]` is passed with at least one language, labels in other languages will be deleted. In the examples on this page, labels are not passed, so they are preserved
- `userfieldconfig.*` methods work with modules other than CRM. For fields in another module, `moduleId` and `entityId` change, and the precision for the type `double` is set using the same `PRECISION` configuration
- if you need a single field case for the entire scenario, [userfieldconfig.list](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-list.md) is a suitable alternative to [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md): it returns camelCase, like `add` and `update`. It has different calling rules: a mandatory `moduleId`, the field list arrives in `result.fields`, not in the root `result`, and field names will only be returned if passed to the `select` key `language` — this is the equivalent of the `LANG` filter from step 1

## Continue Learning

- [{#T}](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md)
- [{#T}](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md)
- [{#T}](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-list.md)
- [{#T}](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-get.md)
- [{#T}](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md)
- [{#T}](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-settings-fields.md)
- [{#T}](../../../api-reference/crm/universal/userfieldconfig/index.md)
- [{#T}](./how-to-add-user-field-to-spa.md)
