# How to Work with the Binding to CRM Elements Field

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — administrative access to the CRM section
>
> - [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) — a CRM administrator
> - [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) — a user with permission to modify items of a CRM object
> - [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — a user with permission to read items of a CRM object
> - [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Binding to CRM Elements field stores a reference to a CRM element — a lead, deal, contact, or company. The value looks like `C_1`: the CRM object type code and the element number.

Let us walk through it with deals. We will create two fields: Responsible Contact for a single value and Contractors for several. We will fill them in for one deal, read them back, and find the elements themselves by the stored values.

The scenario consists of four steps.

1. Create the fields with the [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method
2. Write the values with the [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method
3. Read the bindings with the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method
4. Resolve the values into elements with the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) and [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) methods

## Before You Start

Prepare the scenario data:

- **A deal to fill the fields in.** You will need its `id`. Deals have `entityTypeId` equal to `2`. The fields themselves are created for all deals at once, not for a single one
- **The elements to bind to.** The example uses one contact and two companies. Their identifiers are returned by [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md), or by [crm.contact.list](../../../api-reference/crm/contacts/crm-contact-list.md) and [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md)
- **REST access.** A webhook or an application with the `crm` permission. Only a CRM administrator can create fields

{% include [Note on examples](../../../_includes/examples.md) %}

## 1. Create the Binding Fields

Create two fields with the [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method and the following parameters:

- `FIELD_NAME` — the field name. The method adds the `UF_CRM_` prefix itself, so pass only `BIND_ONE` and `BIND_MANY`
- `USER_TYPE_ID` — specify `crm`, this is the Binding to CRM Elements type
- `MULTIPLE` — `N` for a single value, `Y` for several
- `SETTINGS` — which objects are allowed in the field. The keys `LEAD`, `CONTACT`, `COMPANY`, and `DEAL` take the values `Y` or `N`

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const one = await $b24.actions.v2.call.make({
        method: 'crm.deal.userfield.add',
        params: {
            fields: {
                FIELD_NAME: 'BIND_ONE',
                USER_TYPE_ID: 'crm',
                MULTIPLE: 'N',
                EDIT_FORM_LABEL: { en: 'Responsible Contact' },
                SETTINGS: { LEAD: 'N', CONTACT: 'Y', COMPANY: 'N', DEAL: 'N' }
            }
        },
        requestId: 'userfield-add-one'
    })

    const many = await $b24.actions.v2.call.make({
        method: 'crm.deal.userfield.add',
        params: {
            fields: {
                FIELD_NAME: 'BIND_MANY',
                USER_TYPE_ID: 'crm',
                MULTIPLE: 'Y',
                EDIT_FORM_LABEL: { en: 'Contractors' },
                SETTINGS: { LEAD: 'N', CONTACT: 'N', COMPANY: 'Y', DEAL: 'N' }
            }
        },
        requestId: 'userfield-add-many'
    })

    console.log(one.getData().result, many.getData().result)
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )

    # Called through the SDK core: there is no typed wrapper for user fields
    one = token.call_method("crm.deal.userfield.add", {
        "fields": {
            "FIELD_NAME": "BIND_ONE",
            "USER_TYPE_ID": "crm",
            "MULTIPLE": "N",
            "EDIT_FORM_LABEL": {"en": "Responsible Contact"},
            "SETTINGS": {"LEAD": "N", "CONTACT": "Y", "COMPANY": "N", "DEAL": "N"},
        },
    })["result"]

    many = token.call_method("crm.deal.userfield.add", {
        "fields": {
            "FIELD_NAME": "BIND_MANY",
            "USER_TYPE_ID": "crm",
            "MULTIPLE": "Y",
            "EDIT_FORM_LABEL": {"en": "Contractors"},
            "SETTINGS": {"LEAD": "N", "CONTACT": "N", "COMPANY": "Y", "DEAL": "N"},
        },
    })["result"]

    print(one, many)
    ```

- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Called through the SDK core: there is no typed wrapper for user fields
    $one = $serviceBuilder->core->call('crm.deal.userfield.add', ['fields' => [
        'FIELD_NAME' => 'BIND_ONE',
        'USER_TYPE_ID' => 'crm',
        'MULTIPLE' => 'N',
        'EDIT_FORM_LABEL' => ['en' => 'Responsible Contact'],
        'SETTINGS' => ['LEAD' => 'N', 'CONTACT' => 'Y', 'COMPANY' => 'N', 'DEAL' => 'N'],
    ]])->getResponseData()->getResult();

    $many = $serviceBuilder->core->call('crm.deal.userfield.add', ['fields' => [
        'FIELD_NAME' => 'BIND_MANY',
        'USER_TYPE_ID' => 'crm',
        'MULTIPLE' => 'Y',
        'EDIT_FORM_LABEL' => ['en' => 'Contractors'],
        'SETTINGS' => ['LEAD' => 'N', 'CONTACT' => 'N', 'COMPANY' => 'Y', 'DEAL' => 'N'],
    ]])->getResponseData()->getResult();

    print_r([$one, $many]);
    ```
{% endlist %}

The method returns the identifier of the created field.

```json
{
    "result": 125
}
```

The full field names became `UF_CRM_BIND_ONE` and `UF_CRM_BIND_MANY`. Universal methods address them in a different form — `ufCrmBindOne` and `ufCrmBindMany`.

The name is not always converted the same way. If it contains a digit, it stays unchanged after the `ufCrm_` prefix: the field `UF_CRM_1688736288` arrives as `ufCrm_1688736288`. Do not assemble the name manually — use the name returned by [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md), which also shows the `type` and `isMultiple` of every field. The list of fields with their original names is returned by [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md).

## 2. Write the Values

The codes of all object types are listed in the [Format of Values for User Field "Binding to CRM Elements"](../../../api-reference/crm/data-types.md#crm-binding-format) section. In the example below, `C_1` is the contact with `id` `1`, and `CO_1` and `CO_2` are companies.

This is where the main difference between the two fields lies:

- a **single field** accepts a string — `"C_1"`
- a **multiple field** accepts an array of strings — `["CO_1", "CO_2"]`

Write both values with one [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) call.

{% list tabs %}

- JS

    ```javascript
    const updated = await $b24.actions.v2.call.make({
        method: 'crm.item.update',
        params: {
            entityTypeId: 2,
            id: 10,
            fields: {
                ufCrmBindOne: 'C_1',
                ufCrmBindMany: ['CO_1', 'CO_2']
            }
        },
        requestId: 'crm-item-update'
    })

    if (!updated.isSuccess) {
        throw new Error(updated.getErrorMessages().join('; '))
    }
    ```

- Python

    ```python
    token.call_method("crm.item.update", {
        "entityTypeId": 2,
        "id": 10,
        "fields": {
            "ufCrmBindOne": "C_1",
            "ufCrmBindMany": ["CO_1", "CO_2"],
        },
    })
    ```

- PHP

    ```php
    $serviceBuilder->core->call('crm.item.update', [
        'entityTypeId' => 2,
        'id' => 10,
        'fields' => [
            'ufCrmBindOne' => 'C_1',
            'ufCrmBindMany' => ['CO_1', 'CO_2'],
        ],
    ]);
    ```
{% endlist %}

## 3. Read the Bindings

Read the deal with the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method and see what was saved.

{% list tabs %}

- JS

    ```javascript
    const response = await $b24.actions.v2.call.make({
        method: 'crm.item.get',
        params: { entityTypeId: 2, id: 10 },
        requestId: 'crm-item-get'
    })

    const item = response.getData().result.item

    console.log(item.ufCrmBindOne)   // 'C_1'
    console.log(item.ufCrmBindMany)  // ['CO_1', 'CO_2']
    ```

- Python

    ```python
    item = token.call_method("crm.item.get", {
        "entityTypeId": 2,
        "id": 10,
    })["result"]["item"]

    print(item["ufCrmBindOne"])   # 'C_1'
    print(item["ufCrmBindMany"])  # ['CO_1', 'CO_2']
    ```

- PHP

    ```php
    $item = $serviceBuilder->core->call('crm.item.get', [
        'entityTypeId' => 2,
        'id' => 10,
    ])->getResponseData()->getResult()['item'];

    print_r($item['ufCrmBindOne']);   // 'C_1'
    print_r($item['ufCrmBindMany']);  // ['CO_1', 'CO_2']
    ```
{% endlist %}

The fields come back in the same form they were written in: the single one as a string, the multiple one as an array. An empty field arrives with the value `null`.

```json
{
    "result": {
        "item": {
            "id": 10,
            "title": "Server Purchase",
            "ufCrmBindOne": "C_1",
            "ufCrmBindMany": ["CO_1", "CO_2"]
        }
    }
}
```

## 4. Resolve the Values into Elements

The string `C_1` on its own tells the user nothing. To show a contact name or a company name, the value has to be split into a code and a number, and then the element has to be retrieved.

The correspondence between codes and object types is returned by the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. In the response, `SYMBOL_CODE_SHORT` is the very code from the value, and `ID` is the object type identifier that [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) accepts in the `entityTypeId` parameter.

{% list tabs %}

- JS

    ```javascript
    const enumResponse = await $b24.actions.v2.call.make({
        method: 'crm.enum.ownertype',
        params: {},
        requestId: 'crm-enum-ownertype'
    })

    const codes = {}
    for (const row of enumResponse.getData().result) {
        codes[row.SYMBOL_CODE_SHORT] = { entityTypeId: Number(row.ID), name: row.NAME }
    }

    const values = [item.ufCrmBindOne, ...item.ufCrmBindMany]

    for (const value of values) {
        const separator = value.lastIndexOf('_')
        const code = value.slice(0, separator)
        const elementId = Number(value.slice(separator + 1))
        const type = codes[code]

        const element = await $b24.actions.v2.call.make({
            method: 'crm.item.get',
            params: { entityTypeId: type.entityTypeId, id: elementId },
            requestId: 'crm-item-get-linked'
        })

        console.log(value, type.name, element.getData().result.item)
    }
    ```

- Python

    ```python
    rows = token.call_method("crm.enum.ownertype")["result"]
    codes = {r["SYMBOL_CODE_SHORT"]: {"entity_type_id": int(r["ID"]), "name": r["NAME"]} for r in rows}

    values = [item["ufCrmBindOne"], *item["ufCrmBindMany"]]

    for value in values:
        code, _, element_id = value.rpartition("_")
        entity_type = codes[code]

        element = token.call_method("crm.item.get", {
            "entityTypeId": entity_type["entity_type_id"],
            "id": int(element_id),
        })["result"]["item"]

        print(value, entity_type["name"], element)
    ```

- PHP

    ```php
    $rows = $serviceBuilder->core->call('crm.enum.ownertype')
        ->getResponseData()->getResult();

    $codes = [];
    foreach ($rows as $row) {
        $codes[$row['SYMBOL_CODE_SHORT']] = ['entityTypeId' => (int)$row['ID'], 'name' => $row['NAME']];
    }

    $values = array_merge([$item['ufCrmBindOne']], $item['ufCrmBindMany']);

    foreach ($values as $value) {
        $separator = strrpos($value, '_');
        $code = substr($value, 0, $separator);
        $elementId = (int)substr($value, $separator + 1);
        $type = $codes[$code];

        $element = $serviceBuilder->core->call('crm.item.get', [
            'entityTypeId' => $type['entityTypeId'],
            'id' => $elementId,
        ])->getResponseData()->getResult()['item'];

        echo $value . ' ' . $type['name'] . PHP_EOL;
        print_r($element);
    }
    ```
{% endlist %}

Split the value at the **last** underscore: the type code may contain one itself. For example, the code of a smart process looks like `T80`, and the code of a requisite is `RQ`.

Check that the code was found in the reference. The field also accepts an arbitrary prefix, so the value may contain a code that is absent from the method response — the examples below include such a check.

The method returns the correspondence between codes and types.

```json
{
    "result": [
        { "ID": "1", "SYMBOL_CODE": "LEAD", "SYMBOL_CODE_SHORT": "L", "NAME": "Lead" },
        { "ID": "3", "SYMBOL_CODE": "CONTACT", "SYMBOL_CODE_SHORT": "C", "NAME": "Contact" },
        { "ID": "4", "SYMBOL_CODE": "COMPANY", "SYMBOL_CODE_SHORT": "CO", "NAME": "Company" }
    ]
}
```

## Verify the Result

The scenario is successful if:

- the `ufCrmBindOne` field holds a string like `C_1`, not an array and not the value `Array`
- the `ufCrmBindMany` field holds an array of strings
- every value was split into a known code and a number, and [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returned an element for it

In the Bitrix24 interface, open the deal card. The created fields appear in it automatically, but not necessarily in the first section — scroll the card to the end. The Responsible Contact field will show the contact name, and the Contractors field will show the company names separated by commas.

The card substitutes the names itself, while [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns only codes and numbers. That is why an integration has to retrieve the names separately — this is what the fourth step is for.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `100` with the text `Expected iterable value for multiple field, but got string instead` | A string was passed to a multiple field. Pass an array of strings ||
|| `NOT_FOUND` in [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) | No element with this `id` exists, or the user has no permission to read it ||
|| `ERROR_CORE` with the text `Incorrect custom type specified.` | A non-existent type was passed in `USER_TYPE_ID`. Binding requires `crm` ||
|| An empty code with the text `The 'FIELD_NAME' field is not found` | The field name was not passed in `FIELD_NAME` ||
|#

An incorrect value is saved more often than it raises an error. If there was no error but the result is wrong, check the following:

- **the single field holds the string `Array`** — an array was passed to it instead of a string. The value is lost, write it again
- **an object of the wrong type was saved** — a type forbidden in `SETTINGS` is saved anyway. The field settings limit the choice in the interface, but do not validate a write through REST
- **the value is there, but the element does not open** — a binding to a non-existent element is saved as well. The existence of the element is not checked on write
- **the code in the value is not found in the reference** — an arbitrary prefix such as `XX_1` is saved without an error. Check the code against the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) response

## Key Considerations

- The field does not validate the value on write. Assemble and check it on your side: an incorrect binding is saved silently, and the error surfaces later
- The container check is asymmetric: a multiple field rejects a string, while a single field silently accepts an array. Choose the value type by `MULTIPLE`, not by how many elements you are binding right now
- The value stores only the type code and the number. A contact name or a company name has to be retrieved with a separate request
- Take the type code from [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) instead of composing it manually. For smart processes it is calculated by a special rule described in the [reference](../../../api-reference/crm/data-types.md#crm-binding-format)
- Do not confuse the binding format with the target object format in automation: there the full type name is used — `DEAL_25`, not `D_25`

## Where Else Binding Fields Are Found

Fields of this type exist outside CRM as well, and not only as user fields.

#|
|| **Where** | **Field** | **Methods** ||
|| CRM objects: lead, deal, contact, company, quote | A user field of type `crm` | [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md), [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) ||
|| Tasks | The system field `UF_CRM_TASK`, multiple | [tasks.task.add](../../../api-reference/tasks/tasks-task-add.md), [tasks.task.update](../../../api-reference/tasks/tasks-task-update.md) ||
|| Lists | An element property with the binding type | [lists.field.add](../../../api-reference/lists/fields/lists-field-add.md), [lists.element.add](../../../api-reference/lists/elements/lists-element-add.md) ||
|| Commercial catalog | A product property of type `ECrm` | [catalog.productProperty.add](../../../api-reference/catalog/product-property/catalog-product-property-add.md) ||
|| Calendar events | The `crm_fields` parameter | [calendar.event.add](../../../api-reference/calendar/calendar-event/calendar-event-add.md) ||
|#

The value format is the same everywhere — the type code and the element number. Only the field name and the method that fills it differ.

The scenario of binding a task to a smart process element is covered separately in the [{#T}](../../tasks/how-to-connect-task-to-spa.md) tutorial.

## Code Example

The code goes through all four steps: it creates the fields, writes the values, reads them, and resolves them into CRM elements.

Replace the webhook, the deal identifier, and the element identifiers.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    const entityTypeId = 2
    const dealId = 10

    async function call(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })

        if (!response.isSuccess) {
            throw new Error(method + ': ' + response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    // 1. Create the single and multiple binding fields
    await call('crm.deal.userfield.add', { fields: {
        FIELD_NAME: 'BIND_ONE',
        USER_TYPE_ID: 'crm',
        MULTIPLE: 'N',
        EDIT_FORM_LABEL: { en: 'Responsible Contact' },
        SETTINGS: { LEAD: 'N', CONTACT: 'Y', COMPANY: 'N', DEAL: 'N' }
    }}, 'userfield-add-one')

    await call('crm.deal.userfield.add', { fields: {
        FIELD_NAME: 'BIND_MANY',
        USER_TYPE_ID: 'crm',
        MULTIPLE: 'Y',
        EDIT_FORM_LABEL: { en: 'Contractors' },
        SETTINGS: { LEAD: 'N', CONTACT: 'N', COMPANY: 'Y', DEAL: 'N' }
    }}, 'userfield-add-many')

    // 2. Write the values: a string to the single field, an array to the multiple one
    await call('crm.item.update', {
        entityTypeId,
        id: dealId,
        fields: { ufCrmBindOne: 'C_1', ufCrmBindMany: ['CO_1', 'CO_2'] }
    }, 'crm-item-update')

    // 3. Read the bindings
    const { item } = await call('crm.item.get', { entityTypeId, id: dealId }, 'crm-item-get')

    // 4. Resolve the values into elements
    const rows = await call('crm.enum.ownertype', {}, 'crm-enum-ownertype')
    const codes = {}
    for (const row of rows) {
        codes[row.SYMBOL_CODE_SHORT] = { entityTypeId: Number(row.ID), name: row.NAME }
    }

    for (const value of [item.ufCrmBindOne, ...item.ufCrmBindMany]) {
        const separator = value.lastIndexOf('_')
        const type = codes[value.slice(0, separator)]

        if (!type) {
            console.warn('Unknown type code in the value', value)
            continue
        }

        const linked = await call('crm.item.get', {
            entityTypeId: type.entityTypeId,
            id: Number(value.slice(separator + 1))
        }, 'crm-item-get-linked')

        const label = linked.item.title || [linked.item.name, linked.item.lastName].filter(Boolean).join(' ')

        console.log(value, '->', type.name, label)
    }
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )

    entity_type_id = 2
    deal_id = 10

    # 1. Create the single and multiple binding fields
    token.call_method("crm.deal.userfield.add", {"fields": {
        "FIELD_NAME": "BIND_ONE",
        "USER_TYPE_ID": "crm",
        "MULTIPLE": "N",
        "EDIT_FORM_LABEL": {"en": "Responsible Contact"},
        "SETTINGS": {"LEAD": "N", "CONTACT": "Y", "COMPANY": "N", "DEAL": "N"},
    }})

    token.call_method("crm.deal.userfield.add", {"fields": {
        "FIELD_NAME": "BIND_MANY",
        "USER_TYPE_ID": "crm",
        "MULTIPLE": "Y",
        "EDIT_FORM_LABEL": {"en": "Contractors"},
        "SETTINGS": {"LEAD": "N", "CONTACT": "N", "COMPANY": "Y", "DEAL": "N"},
    }})

    # 2. Write the values: a string to the single field, an array to the multiple one
    token.call_method("crm.item.update", {
        "entityTypeId": entity_type_id,
        "id": deal_id,
        "fields": {"ufCrmBindOne": "C_1", "ufCrmBindMany": ["CO_1", "CO_2"]},
    })

    # 3. Read the bindings
    item = token.call_method("crm.item.get", {
        "entityTypeId": entity_type_id,
        "id": deal_id,
    })["result"]["item"]

    # 4. Resolve the values into elements
    rows = token.call_method("crm.enum.ownertype")["result"]
    codes = {r["SYMBOL_CODE_SHORT"]: {"entity_type_id": int(r["ID"]), "name": r["NAME"]} for r in rows}

    for value in [item["ufCrmBindOne"], *item["ufCrmBindMany"]]:
        code, _, element_id = value.rpartition("_")
        entity_type = codes.get(code)

        if not entity_type:
            print("Unknown type code in the value", value)
            continue

        linked = token.call_method("crm.item.get", {
            "entityTypeId": entity_type["entity_type_id"],
            "id": int(element_id),
        })["result"]["item"]

        label = linked.get("title") or " ".join(filter(None, [linked.get("name"), linked.get("lastName")]))

        print(value, "->", entity_type["name"], label)
    ```

- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $entityTypeId = 2;
    $dealId = 10;

    function call($serviceBuilder, $method, $params = [])
    {
        return $serviceBuilder->core->call($method, $params)->getResponseData()->getResult();
    }

    // 1. Create the single and multiple binding fields
    call($serviceBuilder, 'crm.deal.userfield.add', ['fields' => [
        'FIELD_NAME' => 'BIND_ONE',
        'USER_TYPE_ID' => 'crm',
        'MULTIPLE' => 'N',
        'EDIT_FORM_LABEL' => ['en' => 'Responsible Contact'],
        'SETTINGS' => ['LEAD' => 'N', 'CONTACT' => 'Y', 'COMPANY' => 'N', 'DEAL' => 'N'],
    ]]);

    call($serviceBuilder, 'crm.deal.userfield.add', ['fields' => [
        'FIELD_NAME' => 'BIND_MANY',
        'USER_TYPE_ID' => 'crm',
        'MULTIPLE' => 'Y',
        'EDIT_FORM_LABEL' => ['en' => 'Contractors'],
        'SETTINGS' => ['LEAD' => 'N', 'CONTACT' => 'N', 'COMPANY' => 'Y', 'DEAL' => 'N'],
    ]]);

    // 2. Write the values: a string to the single field, an array to the multiple one
    call($serviceBuilder, 'crm.item.update', [
        'entityTypeId' => $entityTypeId,
        'id' => $dealId,
        'fields' => ['ufCrmBindOne' => 'C_1', 'ufCrmBindMany' => ['CO_1', 'CO_2']],
    ]);

    // 3. Read the bindings
    $item = call($serviceBuilder, 'crm.item.get', [
        'entityTypeId' => $entityTypeId,
        'id' => $dealId,
    ])['item'];

    // 4. Resolve the values into elements
    $codes = [];
    foreach (call($serviceBuilder, 'crm.enum.ownertype') as $row) {
        $codes[$row['SYMBOL_CODE_SHORT']] = ['entityTypeId' => (int)$row['ID'], 'name' => $row['NAME']];
    }

    foreach (array_merge([$item['ufCrmBindOne']], $item['ufCrmBindMany']) as $value) {
        $separator = strrpos($value, '_');
        $code = substr($value, 0, $separator);

        if (!isset($codes[$code])) {
            echo 'Unknown type code in the value ' . $value . PHP_EOL;
            continue;
        }

        $linked = call($serviceBuilder, 'crm.item.get', [
            'entityTypeId' => $codes[$code]['entityTypeId'],
            'id' => (int)substr($value, $separator + 1),
        ])['item'];

        $label = $linked['title'] ?? trim(($linked['name'] ?? '') . ' ' . ($linked['lastName'] ?? ''));

        echo $value . ' -> ' . $codes[$code]['name'] . ' ' . $label . PHP_EOL;
    }
    ```
{% endlist %}

## Continue Learning

- [Format of Values for the Binding to CRM Elements Field](../../../api-reference/crm/data-types.md#crm-binding-format)
- [CRM Object Types](../../../api-reference/crm/data-types.md#object_type)
- [{#T}](../../tasks/how-to-connect-task-to-spa.md)
- [{#T}](./how-to-add-user-field-to-spa.md)
- [Add a Deal User Field crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md)
- [Get CRM Object Types crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md)
