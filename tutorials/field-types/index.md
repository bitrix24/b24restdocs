# Field Types: Typical Scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields extend the cards of deals and other CRM objects, tasks, employees, universal list items, and inventory accounting documents. The value format is set by the field type, not by the object the field belongs to: methods differ between modules, but the rules for working with the value stay the same.

The most common mistake when working with fields is passing to the API what is visible in the interface. In the card, the field displays text: the name of a list option, a product name, a section name. In the API, the same field stores an identifier.

> Quick links: [all scenarios](#choose-tutorial)
>
> User documentation: [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## How a Field Type Works

**Type code**. The type is set by the `USER_TYPE_ID` parameter when the field is created. The full list of available codes is returned by the [crm.userfield.types](../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md) and [userfieldconfig.getTypes](../../api-reference/crm/universal/userfieldconfig/userfieldconfig-get-types.md) methods.

**Parameter casing**. Names depend on the method family: `crm.*.userfield.*` methods accept `USER_TYPE_ID`, `MULTIPLE`, and `SETTINGS`, while `userfieldconfig.*` methods accept `userTypeId`, `multiple`, and `settings`. An unsupported key is ignored by the method without an error, so names are never carried over from one family to the other. A breakdown with examples is available in the [How to Configure Rounding for a Number Custom Field](./how-to-add-precision-to-user-field.md#case) tutorial.

**Type settings**. Every type has its own set of settings in the `SETTINGS` parameter: rounding precision for a number field, the information block identifier for binding fields, display style and height for a list. The list options themselves are not part of `SETTINGS` — they are passed in the `LIST` or `enum` parameter. The sets of settings are described in the [Parameter SETTINGS](../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md#settings) section of the field creation method.

**Single and multiple values**. The `MULTIPLE` flag is set together with the type at creation. The value of a multiple field is received and passed as an array, even when only one element is filled in.

**Silent failures**. The method returns no error in any of these cases:

- text instead of an identifier in `enumeration`, `iblock_element`, or `iblock_section` clears the field, the response contains `0`, and filtering by text returns unrelated records
- text, a nonexistent code, or an arbitrary prefix such as `XX_1` in `crm` and `crm_status` is retained verbatim
- the [crm.deal.userfield.update](../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-update.md) method does not accept the `USER_TYPE_ID` and `MULTIPLE` parameters and silently ignores them: neither the type nor the multiplicity of an existing field changes, so the field is deleted and created again

This is why the result of a write is verified by reading it back, not by the response code.

### What the Value Stores by Type {#value-by-type}

Four of the five types are binding fields: they store the identifier of a record kept elsewhere in Bitrix24, not the value itself. The fifth one is `enumeration`. None of them returns a readable name — it is retrieved by a separate call.

#|
|| **Field type** | **What the value stores** ||
|| `enumeration` | The identifier of the selected option, not its text ||
|| `crm` | A link to a CRM element in the `C_1` format: the object type code and the element number ||
|| `crm_status` | The string `STATUS_ID` code of an option from a system CRM directory, for example `IT` ||
|| `iblock_element` | Identifiers of information block elements: universal list items or catalog products ||
|| `iblock_section` | Identifiers of information block sections. Nested sections are not included in the value ||
|#

**Choosing between `enumeration` and `crm_status`**. Both types present the user with a list of options, but the options are stored in different places. With `enumeration`, the list belongs to the field itself: it is created together with the field and changed by the field update method. The `crm_status` type refers to a shared CRM directory, such as `INDUSTRY`. The same set of values is used in other CRM sections, and the directory is edited with the [crm.status.*](../../api-reference/crm/status/index.md) methods. Create an `enumeration` field when the list is needed only for that field, and a `crm_status` field when the directory is shared.

The remaining types — `string`, `integer`, `double`, `boolean`, `date`, `datetime`, `file`, `employee`, `money`, `address`, `url` — have no dedicated scenario. Where to look up their value formats:

- the structure is returned by the field description in the [crm.deal.fields](../../api-reference/crm/deals/crm-deal-fields.md) response, and composite values are covered in the CRM data types: [address](../../api-reference/crm/data-types.md#crm_user_field_address) and [binding to CRM elements](../../api-reference/crm/data-types.md#crm-binding-format)
- `money` is passed together with the currency code, separated by a vertical bar: `300|EUR`
- `file` is filled in following the rules from the [{#T}](../../api-reference/files/index.md) section

## What You Need

**Scope**. Depends on the method family that creates the field. For `crm.*.userfield.*` methods, the [`crm`](../../api-reference/scopes/permissions.md) scope is enough. The `userfieldconfig.*` methods require the [`userfieldconfig`](../../api-reference/scopes/permissions.md) scope and, in addition, the scope of the module from `moduleId`. Scenarios for information block binding fields also require [`lists`](../../api-reference/scopes/permissions.md) and [`catalog`](../../api-reference/scopes/permissions.md) — these are used to retrieve element and section identifiers.

**Permissions**. Depend on the method family. `crm.deal.userfield.add` requires CRM administrator permissions. `userfieldconfig.add` requires permission to change the settings of the module from `moduleId`, which in CRM is "Allow changing settings". To write a value into a card, permission to modify the item itself is enough.

**Call method**. Most scenarios run through an [inbound webhook](../../local-integrations/local-webhooks.md#incoming-webhook). The exception is filling a field automatically by event: this requires a [local application](../../local-integrations/local-apps.md) with OAuth authorization and a handler at a public HTTPS address. A webhook does not receive the event.

## How to Choose a Scenario {#choose-tutorial}

#|
|| **If necessary** | **Open** ||
|| Create a custom field in a smart process and configure its labels | [How to Create a Custom Field in a Smart Process](./how-to-add-user-field-to-spa.md) ||
|| Set the rounding precision for a number field | [How to Configure Rounding for a Number Custom Field](./how-to-add-precision-to-user-field.md) ||
|| Write a value into a field with a list of options, filter records by it, and change the set of options | [How to Work with the List Field Type](./how-to-use-enumeration-field.md) ||
|| Link a card to another CRM element and expand the binding into a record | [How to Work with the Binding to CRM Elements Field](./how-to-use-crm-binding-field.md) ||
|| Select an option from a shared CRM directory in a field | [How to Work with the Binding to CRM Directories Field](./how-to-use-crm-status-field.md) ||
|| Link a card to a universal list item or a catalog product | [How to Work with the Binding to Information Block Elements Field](./how-to-use-iblock-binding-field.md) ||
|| Link a card to a list or catalog section | [How to Work with the Binding to Information Block Sections Field](./how-to-use-iblock-section-binding-field.md) ||
|| Fill one field automatically based on the value of another after the card is saved | [How to Automatically Fill a Dependent CRM Field After the Main Field Changes](./how-to-autofill-dependent-field.md) ||
|#

## Field Methods by Module

The method family is chosen by the object the field belongs to. In CRM there are three families, and they solve different tasks.

#|
|| **Methods** | **Purpose** ||
|| [{#T}](../../api-reference/crm/deals/user-defined-fields/index.md) | Create, update, and delete a field of a specific CRM object. The same families exist for [leads](../../api-reference/crm/leads/userfield/index.md), [contacts](../../api-reference/crm/contacts/userfield/index.md), [companies](../../api-reference/crm/companies/userfields/index.md), [estimates](../../api-reference/crm/quote/user-field/index.md), and [company details](../../api-reference/crm/requisites/user-fields/index.md) ||
|| [{#T}](../../api-reference/crm/universal/userfieldconfig/index.md) | Create and configure a field of any CRM object through a single interface. The only way to do it for smart processes and new invoices: they have no `userfield` methods of their own. It also works in other modules through `moduleId`, for example `rpa` and `catalog` ||
|| [{#T}](../../api-reference/crm/universal/user-defined-fields/index.md) | Retrieve reference information: available types, characteristics, and settings. These methods do not create a field in a card ||
|#

Outside CRM, a field is created with the methods of its own module: [tasks](../../api-reference/tasks/user-field/index.md), [employees](../../api-reference/user/userfields/index.md), [universal list items](../../api-reference/lists/fields/index.md). For [inventory accounting documents](../../api-reference/catalog/userfield-document/index.md), the methods only read and update values — the field itself is created through `userfieldconfig.*` with `moduleId = catalog`.

## Continue Learning

- [{#T}](../../api-reference/crm/universal/user-defined-fields/userfield-type.md) — how an application registers its own field type
- [{#T}](../catalog/how-to-change-product-custom-field-values.md) — information block properties rather than custom fields
- [{#T}](../index.md) — all tutorial categories
