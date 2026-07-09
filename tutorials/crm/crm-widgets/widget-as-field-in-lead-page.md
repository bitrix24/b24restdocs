# How to Embed a Widget into a Lead as a Custom Field

> Scope: [`placement`, `crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods:
>
> - `userfieldtype.add` — administrator
> - `app.info` — any user
> - `crm.lead.userfield.add` — CRM administrator
> - `crm.item.get` — any user with lead read permissions

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A custom field type allows you to display an application interface directly within a lead card. In this scenario, we will create a field with the code `PHONE_DATA` without the `UF_CRM_` prefix.

If the field is empty, the handler receives the card context, reads the lead's phone number, and passes the found value into the form.

To embed a widget into a lead field, perform the following methods and commands in sequence:

1. [userfieldtype.add](../../../api-reference/widgets/user-field/userfieldtype-add.md) — register a custom field type and the handler URL
2. [app.info](../../../api-reference/common/system/app-info.md) — retrieve the App ID and generate the full field type code
3. [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) — create a field in the lead card
4. [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — retrieve the lead's phone number in the field handler
5. [BX24.placement.call](../../../api-reference/widgets/ui-interaction/bx24-placement-call.md) — pass the new field value into the card form

{% note info "" %}

The scenario requires an [application](../../../settings/app-installation/index.md) context: the `userfieldtype.*` methods will register the field type, and `app.info` will return the application's `ID`. An incoming webhook will not work.

{% endnote %}

## How the Scenario Works

During registration, the `userfieldtype.add` method saves the handler for the private placement point `USERFIELD_TYPE`.

When a user opens a lead card containing a field of this type, Bitrix24 opens the handler URL inside the field and passes `PLACEMENT_OPTIONS` to it. In edit mode, the handler can change the field value by calling:

```js
BX24.placement.call('setValue', value, () => {});
```

For the `setValue` command, the field value itself is passed as the second parameter. The command writes it to a hidden field in the card form. In a lead, the value will be retained after the card is saved. In view mode, the handler can only display an interface or a text value.

## Prepare the Handler

Create an application page with a public address. This address is required for the `HANDLER` parameter of the `userfieldtype.add` method. We recommend using HTTPS so that the browser does not block the loading of the field content.

The handler address must use the `http` or `https` protocol and include a domain.

The following examples use this address:

```text
https://your-domain.example/handler.php
```

If you are executing JS registration examples on an application page, call `BX24.callMethod` after initializing the SDK with the [BX24.init](../../../sdk/bx24-js-sdk/system-functions/bx24-init.md) method.

## 1. Register a Field Type

Register a field type using the [userfieldtype.add](../../../api-reference/widgets/user-field/userfieldtype-add.md) method. Specify the type code, handler URL, and field display configurations.

- `USER_TYPE_ID` — the string type code. We will specify `phone_data`

- `HANDLER` — the public field handler URL. We will pass `https://your-domain.example/handler.php`

- `TITLE` — the field type name in the settings interface. We will specify `Phone data`

- `DESCRIPTION` — the field type description

- `OPTIONS` — additional configurations. In the example, we will set the field height to `height: 60`

{% list tabs %}

- JS

    ```js
    const handlerUrl = 'https://your-domain.example/handler.php';
    const userTypeId = 'phone_data';

    BX24.callMethod(
        'userfieldtype.add',
        {
            USER_TYPE_ID: userTypeId,
            HANDLER: handlerUrl,
            TITLE: 'Phone data',
            DESCRIPTION: 'Lead phone data field',
            OPTIONS: {
                height: 60,
            },
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error() + ': ' + result.error_description());
                return;
            }

            console.info('User field type registered');
        }
    );
    ```

- PHP CRest

    ```php
    <?php
    require_once('crest.php');

    $handlerUrl = 'https://your-domain.example/handler.php';
    $userTypeId = 'phone_data';

    $result = CRest::call(
        'userfieldtype.add',
        [
            'USER_TYPE_ID' => $userTypeId,
            'HANDLER' => $handlerUrl,
            'TITLE' => 'Phone data',
            'DESCRIPTION' => 'Lead phone data field',
            'OPTIONS' => [
                'height' => 60,
            ],
        ]
    );

    if (!empty($result['error']))
    {
        echo $result['error'] . ': ' . $result['error_description'];
    }
    else
    {
        echo 'User field type registered';
    }
    ```

{% endlist %}

If the field type is successfully registered, the method returns `true`. If an error is received `error`, review the possible error descriptions in the [userfieldtype.add](../../../api-reference/widgets/user-field/userfieldtype-add.md) method documentation.

```json
{
    "result": true,
    "time": {
        "start": 1724421710.397825,
        "finish": 1724421711.040353,
        "duration": 0.6425280570983887,
        "processing": 0.00005888938903808594,
        "date_start": "2024-08-23T16:01:50+02:00",
        "date_finish": "2024-08-23T16:01:51+02:00",
        "operating": 0
    }
}
```

The method registers a type with the short code `phone_data`. To create a field in the CRM, you will need the full code in the form of `rest_<APP_ID>_phone_data`.

## 2. Retrieve the App ID

Retrieve the App ID using the [app.info](../../../api-reference/common/system/app-info.md) method. The method does not accept parameters. You will need the `ID` field from the response.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'app.info',
        {},
        (result) => {
            if (result.error())
            {
                console.error(result.error() + ': ' + result.error_description());
                return;
            }

            const applicationId = result.data().ID;
            const userTypeId = 'rest_' + applicationId + '_phone_data';

            console.info('Full user type ID: ' + userTypeId);
        }
    );
    ```

- PHP CRest

    ```php
    <?php
    require_once('crest.php');

    $result = CRest::call('app.info', []);

    if (!empty($result['error']))
    {
        echo $result['error'] . ': ' . $result['error_description'];
    }
    else
    {
        $applicationId = (int)$result['result']['ID'];
        $userTypeId = 'rest_' . $applicationId . '_phone_data';

        echo 'Full user type ID: ' . $userTypeId;
    }
    ```

{% endlist %}

For an app with `ID = 123`, the full type code will be `rest_123_phone_data`.

Response fragment:

```json
{
    "result": {
        "ID": 123,
        "INSTALLED": true
    }
}
```

If `INSTALLED` is set to `false`, complete the app installation using the [BX24.installFinish](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) method.

## 3. Create a Lead Field

Create a lead custom field using the [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) method. Specify the field configurations in the `fields` object.

- `USER_TYPE_ID` — the full code of the registered field type. For an app with `ID = 123`, we will pass `rest_123_phone_data`

- `FIELD_NAME` — the field code without the `UF_CRM_` prefix. We will specify `PHONE_DATA`

- `XML_ID` — the external field code. In the example, this matches `FIELD_NAME`

- `MANDATORY` — field mandatory status. We will pass `N`

- `SHOW_IN_LIST` — whether to show the field in the list. In the CRM, this parameter does not affect field display, but we will include it in the request as a standard custom field parameter

- `EDIT_IN_LIST` — whether the field is editable. We will pass `Y`

- `EDIT_FORM_LABEL` — the field label in the lead card

- `LIST_COLUMN_LABEL` — the field heading in the list

- `SETTINGS` — configurations for the created CRM custom field. For a custom type, we will pass an empty object

{% list tabs %}

- JS

    ```js
    const applicationId = 123;
    const registeredUserTypeId = 'phone_data';
    const userTypeId = 'rest_' + applicationId + '_' + registeredUserTypeId;
    const fieldName = 'PHONE_DATA';

    BX24.callMethod(
        'crm.lead.userfield.add',
        {
            fields: {
                USER_TYPE_ID: userTypeId,
                FIELD_NAME: fieldName,
                XML_ID: fieldName,
                MANDATORY: 'N',
                SHOW_IN_LIST: 'Y',
                EDIT_IN_LIST: 'Y',
                EDIT_FORM_LABEL: 'Phone data',
                LIST_COLUMN_LABEL: 'Phone data',
                SETTINGS: {},
            },
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error() + ': ' + result.error_description());
                return;
            }

            console.info('Lead field created, ID: ' + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    <?php
    require_once('crest.php');

    $applicationId = 123;
    $registeredUserTypeId = 'phone_data';
    $userTypeId = 'rest_' . $applicationId . '_' . $registeredUserTypeId;
    $fieldName = 'PHONE_DATA';

    $result = CRest::call(
        'crm.lead.userfield.add',
        [
            'fields' => [
                'USER_TYPE_ID' => $userTypeId,
                'FIELD_NAME' => $fieldName,
                'XML_ID' => $fieldName,
                'MANDATORY' => 'N',
                'SHOW_IN_LIST' => 'Y',
                'EDIT_IN_LIST' => 'Y',
                'EDIT_FORM_LABEL' => 'Phone data',
                'LIST_COLUMN_LABEL' => 'Phone data',
                'SETTINGS' => [],
            ],
        ]
    );

    if (!empty($result['error']))
    {
        echo $result['error'] . ': ' . $result['error_description'];
    }
    else
    {
        echo 'Lead field created, ID: ' . $result['result'];
    }
    ```

{% endlist %}

If the field is successfully created, the method returns its identifier. If an error `error` is received, review the possible error descriptions in the [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) method documentation.

```json
{
    "result": 6997,
    "time": {
        "start": 1753789240.8146,
        "finish": 1753789241.058695,
        "duration": 0.2440950870513916,
        "processing": 0.19217395782470703,
        "date_start": "2025-07-29T14:40:40+03:00",
        "date_finish": "2025-07-29T14:40:41+03:00",
        "operating_reset_at": 1753789840,
        "operating": 0.19216084480285645
    }
}
```

After creation, the field will appear in the list of lead custom fields. To see it in the card, add the field to the lead card form.

## 4. Handle the Field Call

When a user opens a lead card, Bitrix24 calls the handler with `PLACEMENT=USERFIELD_TYPE`. `PLACEMENT_OPTIONS` receives the custom field parameters and the current lead identifier.

The handler performs two actions:

1. If the field is empty, retrieve the lead phone using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method.
2. Pass the new value to the card form using the [BX24.placement.call](../../../api-reference/widgets/ui-interaction/bx24-placement-call.md) command with the `setValue` command.

For the lead in `crm.item.get`, specify `entityTypeId: 1`. In the `id` parameter, pass the lead identifier from `PLACEMENT_OPTIONS.ENTITY_VALUE_ID`. If the card is already saved, `ENTITY_VALUE_ID` contains the lead identifier. For a new card, the value may be `0`.

If the field already contains a value, the handler will display it without reloading the phone number.

The PHP CRest variant assumes that [app authorization for CRest](../../../first-steps/how-to-use-examples.md) is already configured. `CRest::call` will execute the method with the permissions of the user whose token is stored in the CRest settings. If an administrator token is stored there, the `crm.item.get` permission check will be performed for that administrator rather than for the user who opened the lead card.

{% list tabs %}

- JS

    ```html
    <!DOCTYPE html>
    <html lang="de">
        <head>
            <meta charset="UTF-8">
            <title>Phone data</title>
            <script src="https://api.bitrix24.com/api/v1/"></script>
        </head>
        <body style="margin: 0; padding: 0;">
            <div id="field-content"></div>

            <script>
                BX24.init(() => {
                    const placementOptions = BX24.getPlacementOptions();

                    if (BX24.getPlacement() !== 'USERFIELD_TYPE')
                    {
                        document.getElementById('field-content').textContent =
                            'Failed to determine the embedding type';
                        return;
                    }

                    const currentValue = placementOptions.VALUE || '';
                    const leadId = Number(placementOptions.ENTITY_VALUE_ID);

                    if (currentValue !== '')
                    {
                        renderValue(currentValue, placementOptions);
                        return;
                    }

                    if (
                        placementOptions.ENTITY_ID !== 'CRM_LEAD'
                        || !Number.isInteger(leadId)
                        || leadId <= 0
                    )
                    {
                        renderValue(currentValue, placementOptions);
                        return;
                    }

                    BX24.callMethod(
                        'crm.item.get',
                        {
                            entityTypeId: 1,
                            id: leadId,
                        },
                        (result) => {
                            if (result.error())
                            {
                                renderValue(currentValue, placementOptions);
                                console.error(
                                    result.error() + ': ' + result.error_description()
                                );
                                return;
                            }

                            const item = result.data().item;
                            const phone = (item?.fm || [])
                                .find((field) => field.typeId === 'PHONE' && field.value)
                                ?.value
                                ?.trim()
                                || item?.phone?.trim()
                                || '';
                            const value = phone
                                ? 'Lead phone: ' + phone
                                : 'Phone is empty';

                            renderValue(value, placementOptions);
                        }
                    );
                });

                function renderValue(value, placementOptions)
                {
                    const container = document.getElementById('field-content');

                    document.body.style.backgroundColor =
                        placementOptions.MODE === 'edit' ? '#fff' : '#f9fafb';

                    if (placementOptions.MODE === 'edit')
                    {
                        container.innerHTML =
                            '<input id="phone-data" type="text" style="width: 90%;" />';
                        const input = document.getElementById('phone-data');

                        input.value = value;
                        input.addEventListener('keyup', () => setValue(input.value));
                        setValue(value);
                    }
                    else
                    {
                        container.textContent = value;
                    }
                }

                function setValue(value)
                {
                    BX24.placement.call('setValue', value, () => {});
                }
            </script>
        </body>
    </html>
    ```

- PHP CRest

    ```php
    <?php
    require_once('crest.php');

    $placement = (string)($_REQUEST['PLACEMENT'] ?? '');
    $placementOptionsJson = (string)($_REQUEST['PLACEMENT_OPTIONS'] ?? '{}');
    $placementOptions = json_decode($placementOptionsJson, true);

    if ($placement !== 'USERFIELD_TYPE' || !is_array($placementOptions))
    {
        exit;
    }

    $value = (string)($placementOptions['VALUE'] ?? '');
    $errorMessage = '';

    if (
        $value === ''
        && ($placementOptions['ENTITY_ID'] ?? '') === 'CRM_LEAD'
        && (int)($placementOptions['ENTITY_VALUE_ID'] ?? 0) > 0
    )
    {
        $lead = CRest::call(
            'crm.item.get',
            [
                'entityTypeId' => 1,
                'id' => (int)$placementOptions['ENTITY_VALUE_ID'],
            ]
        );

        if (!empty($lead['error']))
        {
            $errorMessage = ($lead['error'] ?? 'ERROR')
                . ': '
                . ($lead['error_description'] ?? 'Failed to retrieve lead data');
        }
        else
        {
            $item = $lead['result']['item'] ?? [];
            $phone = '';

            foreach (($item['fm'] ?? []) as $field)
            {
                if (
                    ($field['typeId'] ?? '') === 'PHONE'
                    && trim((string)($field['value'] ?? '')) !== ''
                )
                {
                    $phone = trim((string)$field['value']);
                    break;
                }
            }

            if ($phone === '')
            {
                $phone = trim((string)($item['phone'] ?? ''));
            }

            $value = $phone !== '' ? 'Lead phone: ' . $phone : 'Phone is empty';
        }
    }
    ?>
    <!DOCTYPE html>
    <html lang="de">
        <head>
            <meta charset="UTF-8">
            <title>Phone data</title>
            <script src="https://api.bitrix24.com/api/v1/"></script>
        </head>
        <body style="margin: 0; padding: 0; background-color: <?=($placementOptions['MODE'] ?? '') === 'edit' ? '#fff' : '#f9fafb'?>;">
            <?php if ($errorMessage !== ''): ?>
                <div><?=htmlspecialchars($errorMessage, ENT_QUOTES, 'UTF-8')?></div>
            <?php endif; ?>

            <?php if (($placementOptions['MODE'] ?? '') === 'edit'): ?>
                <input
                    id="phone-data"
                    type="text"
                    style="width: 90%;"
                    value="<?=htmlspecialchars($value, ENT_QUOTES, 'UTF-8')?>"
                >
                <script>
                    BX24.init(() => {
                        const input = document.getElementById('phone-data');

                        input.addEventListener('keyup', () => setValue(input.value));
                        setValue(input.value);
                    });

                    function setValue(value) {
                        BX24.placement.call('setValue', value, () => {});
                    }
                </script>
            <?php else: ?>
                <?=htmlspecialchars($value, ENT_QUOTES, 'UTF-8')?>
            <?php endif; ?>
        </body>
    </html>
    ```

{% endlist %}

The `crm.item.get` method returns a `item` object containing lead fields. In the example, the phone number is retrieved from the `fm` array, which contains multiple fields: phones, e-mails, sites, and messengers.

```json
{
    "result": {
        "item": {
            "id": 123,
            "phone": "+499990000000",
            "fm": [
                {
                    "id": 456,
                    "valueType": "WORK",
                    "value": "+499990000000",
                    "typeId": "PHONE"
                }
            ]
        }
    }
}
```

## What the Handler Receives

In the handler's HTTP request, `PLACEMENT_OPTIONS` is passed as a JSON string. The `BX24.getPlacementOptions()` method returns this data as an object. In PHP, `$_REQUEST['PLACEMENT_OPTIONS']` contains a JSON string that must be converted into an array.

#|
|| **Field**
`type` | **Description** ||
|| **MODE**
[`string`](../../../api-reference/data-types.md) | Field display mode. The source code uses values `edit` and `view` ||
|| **ENTITY_ID**
[`string`](../../../api-reference/data-types.md) | Object code for the card where the field is open. For a lead, `CRM_LEAD` is received ||
|| **FIELD_NAME**
[`string`](../../../api-reference/data-types.md) | Full name of the custom field with the prefix `UF_CRM_`. For the field `PHONE_DATA`, `UF_CRM_PHONE_DATA` will be received ||
|| **ENTITY_VALUE_ID**
[`string`, `integer`](../../../api-reference/data-types.md) | CRM item identifier. In this scenario, it is the lead identifier. For a new card, it may have the value `0` ||
|| **VALUE**
[`string`, `array`, `null`](../../../api-reference/data-types.md) | Current field value. For a single field, one value is received; for a multiple field, an array is received ||
|| **MULTIPLE**
[`string`](../../../api-reference/data-types.md) | Multiple field flag: `Y` or `N` ||
|| **MANDATORY**
[`string`](../../../api-reference/data-types.md) | Required field flag: `Y` or `N` ||
|| **XML_ID**
[`string`, `null`](../../../api-reference/data-types.md) | External code of the field ||
|#

## Verify the Widget

1. Execute `userfieldtype.add` and ensure that the method returned `true`.
2. Execute `app.info`, substituting `result.ID` into the full `rest_<APP_ID>_phone_data` type code.
3. Create a field using the `crm.lead.userfield.add` method and add it to the lead card form.
4. Open a saved lead with a populated phone number and check the field value in view mode.
5. Enter edit mode, change the field value, and save the card.
6. Reopen the lead and ensure the new value has been retained.

If the scenario does not work:

- An "Invalid custom type specified" error means that a short code was passed in `crm.lead.userfield.add` instead of `rest_<APP_ID>_phone_data`, or the app installation is incomplete.
- If the field does not load, check the `HANDLER` HTTPS address, its domain, and its availability from the internet.
- A `ACCESS_DENIED` error in `crm.item.get` means the user does not have permission to read the lead.
- If a `NOT_FOUND` error occurs in `crm.item.get`, check `ENTITY_VALUE_ID` and the `entityTypeId` value.
- If the field interface does not launch, check the SDK connection and the execution of client-side code within `BX24.init`.

## How to Adapt a Scenario for Other CRM Cards

To embed the same field into another CRM card, replace the field creation method, the `ENTITY_ID` check, and the `entityTypeId` value in `crm.item.get`.

#|
|| **CRM Card** | **Field creation method** | **ENTITY_ID** | **entityTypeId** ||
|| Lead | [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) | `CRM_LEAD` | `1` ||
|| Deal | [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) | `CRM_DEAL` | `2` ||
|| Contact | [crm.contact.userfield.add](../../../api-reference/crm/contacts/userfield/crm-contact-userfield-add.md) | `CRM_CONTACT` | `3` ||
|| Company | [crm.company.userfield.add](../../../api-reference/crm/companies/userfields/crm-company-userfield-add.md) | `CRM_COMPANY` | `4` ||
|#

## Continue Learning

- [Custom Field Types in CRM](../../../api-reference/crm/universal/user-defined-fields/userfield-type.md)
- [Get app.info Information](../../../api-reference/common/system/app-info.md)
- [Get a List of Custom Field Types userfieldtype.list](../../../api-reference/widgets/user-field/userfieldtype-list.md)
- [Initialize the BX24.init Library](../../../sdk/bx24-js-sdk/system-functions/bx24-init.md)
- [Call the BX24.placement.call Interface Command](../../../api-reference/widgets/ui-interaction/bx24-placement-call.md)
