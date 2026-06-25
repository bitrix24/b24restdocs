# How to Embed a Widget into a Lead as a Custom Field

> Scope: [`placement`, `crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods:
>
> - `userfieldtype.add` — administrator
> - `crm.lead.userfield.add` — CRM administrator
> - `crm.item.get` — any user with lead read permission

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A custom field type allows you to display an application interface directly within a lead card. In this scenario, we will consider an example of a custom field with the code `PHONE_DATA` without the `UF_CRM_` prefix.

The handler receives the card context, reads the lead's phone number, and writes the found data to the field.

To embed a widget into a lead field, we will execute the following methods and commands in sequence:

1. [userfieldtype.add](../../../api-reference/widgets/user-field/userfieldtype-add.md) — register a custom field type and the handler URL
2. [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) — create a field in the lead card
3. [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — retrieve the lead's phone number in the field handler
4. [BX24.placement.call](../../../api-reference/widgets/ui-interaction/bx24-placement-call.md) — pass the new field value to the card form

{% note info "" %}

`userfieldtype.*` methods work only within the context of an [application](../../../settings/app-installation/index.md). An incoming webhook is not suitable for registering a custom field type.

{% endnote %}

## How the Scenario Works

During registration, the `userfieldtype.add` method saves a handler for a private placement point `USERFIELD_TYPE`.

When a user opens a lead card containing a field of this type, Bitrix24 opens the handler URL inside the field and passes `PLACEMENT_OPTIONS` to it. In edit mode, the handler can change the field value by calling:

```js
BX24.placement.call('setValue', value);
```

The `setValue` command writes the value to a hidden field of the card form. In a lead, it will be retained after the card is saved. In view mode, the handler can only display an interface or a text value.

## Prepare the Handler

Create an application page with a public HTTPS address. This address is required for the `HANDLER` parameter of the `userfieldtype.add` method.

The following address is used in the examples below:

```text
https://your-domain.example/handler.php
```

## 1. Register the Field Type

Register the field type using [userfieldtype.add](../../../api-reference/widgets/user-field/userfieldtype-add.md). Pass the type code, the handler URL, and the field display configurations to the method.

- `USER_TYPE_ID` — a string type code. We will specify `phone_data`

- `HANDLER` — the public URL of the field handler. We will pass `https://your-domain.example/handler.php`

- `TITLE` — the field type name in the settings interface. We will specify `Phone data`

- `DESCRIPTION` — a description of the field type

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

If the field type is successfully registered, the method will return `true`. If you received an error `error`, study the description of possible errors in the [userfieldtype.add](../../../api-reference/widgets/user-field/userfieldtype-add.md) method documentation.

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

Now the `phone_data` type can be used when creating a custom field in the CRM.

## 2. Create a Lead Field

Create a lead custom field using [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md). Pass a `fields` object with the field configurations to the method.

- `USER_TYPE_ID` — the code of the registered field type. We will pass `phone_data`

- `FIELD_NAME` — the field code without the `UF_CRM_` prefix. We will specify `PHONE_DATA`

- `XML_ID` — the external code of the field. In the example, it matches `FIELD_NAME`

- `MANDATORY` — field mandatory status. We will pass `N`

- `SHOW_IN_LIST` — whether to show the field in the list. In the CRM, this parameter does not affect field visibility, but we will include it in the request as a standard custom field parameter

- `EDIT_IN_LIST` — whether the field is editable. We will pass `Y`

- `EDIT_FORM_LABEL` — the field label in the lead card

- `LIST_COLUMN_LABEL` — the field heading in the list

- `SETTINGS` — configurations for the CRM custom field being created. For a custom type, we will pass an empty object

{% list tabs %}

- JS

    ```js
    const userTypeId = 'phone_data';
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

    $userTypeId = 'phone_data';
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

If the field is successfully created, the method returns its identifier. If you receive error `error`, review the possible error descriptions in the [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md) method documentation.

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

## 3. Handle the Field Call

When a user opens a lead card, Bitrix24 calls the handler with `PLACEMENT=USERFIELD_TYPE`. In `PLACEMENT_OPTIONS`, the custom field parameters and the current lead identifier are received.

In the handler, we will perform two actions:

1. Retrieve the lead phone using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method
2. Pass the new value to the card form using the [BX24.placement.call](../../../api-reference/widgets/ui-interaction/bx24-placement-call.md) command with the `setValue` command

In `crm.item.get`, we will pass `entityTypeId: 1`, because `1` is the identifier for the "Lead" CRM object type. In the `id` parameter, we will pass the lead identifier from `PLACEMENT_OPTIONS.ENTITY_VALUE_ID`. If the card is already saved, `ENTITY_VALUE_ID` contains the lead identifier. For a new card, the value may be `0`.

{% list tabs %}

- JS

    ```js
    const placementOptions = BX24.getPlacementOptions();

    if (BX24.getPlacement() === 'USERFIELD_TYPE')
    {
        const currentValue = placementOptions.VALUE || '';

        if (
            placementOptions.ENTITY_ID === 'CRM_LEAD'
            && Number(placementOptions.ENTITY_VALUE_ID) > 0
        )
        {
            BX24.callMethod(
                'crm.item.get',
                {
                    entityTypeId: 1,
                    id: Number(placementOptions.ENTITY_VALUE_ID),
                },
                (result) => {
                    if (result.error())
                    {
                        renderValue(currentValue);
                        console.error(result.error() + ': ' + result.error_description());
                        return;
                    }

                    const item = result.data().item;
                    const phone = (item?.fm || [])
                        .find((field) => field.typeId === 'PHONE' && field.value)
                        ?.value
                        ?.trim()
                        || item?.phone?.trim()
                        || '';
                    const value = phone ? 'Lead phone: ' + phone : 'Phone is empty';

                    renderValue(value);
                }
            );
        }
        else
        {
            renderValue(currentValue);
        }
    }

    function renderValue(value)
    {
        document.body.style.margin = '0';
        document.body.style.padding = '0';
        document.body.style.backgroundColor = placementOptions.MODE === 'edit' ? '#fff' : '#f9fafb';

        if (placementOptions.MODE === 'edit')
        {
            document.body.innerHTML = '<input id="phone-data" type="text" style="width: 90%;" />';
            const input = document.getElementById('phone-data');

            input.value = value;
            input.addEventListener('keyup', () => setValue(input.value));
            setValue(value);
        }
        else
        {
            document.body.textContent = value;
        }
    }

    function setValue(value)
    {
        BX24.placement.call('setValue', value);
    }
    ```

- PHP CRest

    ```php
    <?php
    require_once('crest.php');

    $placement = $_REQUEST['PLACEMENT'] ?? '';
    $placementOptions = isset($_REQUEST['PLACEMENT_OPTIONS'])
        ? json_decode($_REQUEST['PLACEMENT_OPTIONS'], true)
        : [];

    if ($placement !== 'USERFIELD_TYPE')
    {
        exit;
    }

    $value = $placementOptions['VALUE'] ?? '';

    if (
        ($placementOptions['ENTITY_ID'] ?? '') === 'CRM_LEAD'
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

        $item = $lead['result']['item'] ?? [];
        $phone = '';

        foreach (($item['fm'] ?? []) as $field)
        {
            if (($field['typeId'] ?? '') === 'PHONE' && trim((string)($field['value'] ?? '')) !== '')
            {
                $phone = trim((string)$field['value']);
                break;
            }
        }

        if ($phone === '')
        {
            $phone = trim((string)($item['phone'] ?? ''));
        }

        $value = $phone !== '' ? 'Lead phone: ' . trim($phone) : 'Phone is empty';
    }
    ?>
    <!DOCTYPE html>
    <html>
        <head>
            <script src="//api.bitrix24.com/api/v1/"></script>
        </head>
        <body style="margin: 0; padding: 0; background-color: <?=($placementOptions['MODE'] ?? '') === 'edit' ? '#fff' : '#f9fafb'?>;">
            <?php if (($placementOptions['MODE'] ?? '') === 'edit'): ?>
                <input id="phone-data" type="text" style="width: 90%;" value="<?=htmlspecialchars($value)?>">
                <script>
                    const input = document.getElementById('phone-data');

                    function setValue(value)
                    {
                        BX24.placement.call('setValue', value);
                    }

                    input.addEventListener('keyup', () => setValue(input.value));
                    setValue(input.value);
                </script>
            <?php else: ?>
                <?=htmlspecialchars($value)?>
            <?php endif; ?>
        </body>
    </html>
    ```

{% endlist %}

The `crm.item.get` method returns a `item` object containing the lead fields. In the example, the phone is retrieved from the `fm` array, which contains multiple fields: phones, e-mail, sites, and messengers.

```json
{
    "result": {
        "item": {
            "id": 123,
            "phone": "+19990000000",
            "fm": [
                {
                    "id": 456,
                    "valueType": "WORK",
                    "value": "+19990000000",
                    "typeId": "PHONE"
                }
            ]
        }
    }
}
```

## What the Handler Receives

In the `PLACEMENT_OPTIONS` HTTP request of the handler, the data is passed as a JSON string. The `BX24.getPlacementOptions()` method returns this data as an object. In PHP, `$_REQUEST['PLACEMENT_OPTIONS']` contains a JSON string that must be converted into an array.

#|
|| **Field**
`type` | **Description** ||
|| **MODE**
[`string`](../../../api-reference/data-types.md) | Field display mode. The source code uses values `edit` and `view` ||
|| **ENTITY_ID**
[`string`](../../../api-reference/data-types.md) | Object code for the card in which the field is open. For a lead, `CRM_LEAD` is received ||
|| **FIELD_NAME**
[`string`](../../../api-reference/data-types.md) | Full name of the custom field with the prefix `UF_CRM_`. For the field `PHONE_DATA`, `UF_CRM_PHONE_DATA` will be received ||
|| **ENTITY_VALUE_ID**
[`string`](../../../api-reference/data-types.md) | CRM item identifier. In this scenario, it is the lead identifier ||
|| **VALUE**
[`string`, `array`, `null`](../../../api-reference/data-types.md) | Current field value. For a single field, one value is received; for a multiple field, an array is received ||
|| **MULTIPLE**
[`string`](../../../api-reference/data-types.md) | Multiple field flag: `Y` or `N` ||
|| **MANDATORY**
[`string`](../../../api-reference/data-types.md) | Required field flag: `Y` or `N` ||
|| **XML_ID**
[`string`, `null`](../../../api-reference/data-types.md) | External code of the field. ||
|#
