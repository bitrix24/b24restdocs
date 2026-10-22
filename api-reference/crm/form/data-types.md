# Data Types and Object Structure in the CRM Forms REST API

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

CRM form data types describe the identifiers and object structures in the parameters and responses of the `crm.form.*` methods. This reference shows which fields a form has, which of them can be written, and where to get their values. The basic types `integer`, `string`, `object`, and others are listed in [Data Types in the REST API](../../data-types.md).

## How to Choose an Object

#|
|| **Object** | **When to Use** | **Methods** ||
|| [`crm_form`](#crm_form) | To read, create, and update a CRM form: fields, CRM scheme, appearance, consents, post-submission actions, integrations | [crm.form.*](./index.md) ||
|| [`crm_form_configuration`](#crm_form_configuration) | To retrieve the values required to create a form: CRM schemes, pipelines, field types, languages, payment systems, permissions, and the active forms limit | [crm.form.configuration.get](./setup/crm-form-configuration-get.md) ||
|| [`crm_form_scenario`](#crm_form_scenario) | To choose a ready-made form scenario | [crm.form.scenario.list](./setup/crm-form-scenario-list.md) ||
|| [`crm_form_scenario_category`](#crm_form_scenario_category) | To group scenarios by category | [crm.form.scenariocategory.list](./setup/crm-form-scenariocategory-list.md) ||
|| [`crm_form_entity_field`](#crm_form_entity_field) | To choose the CRM object fields the form is built from | [crm.form.entityfield.list](./setup/crm-form-entityfield-list.md) ||
|| [`crm_form_embedding`](#crm_form_embedding) | To retrieve the embed code, the public link, and the button and auto-display settings | [crm.form.embedding.*](./placement/index.md) ||
|| [`crm_form_embedding_selection`](#crm_form_embedding_selection) | To retrieve the code of a single placement option | [crm.form.embedding.get](./placement/crm-form-embedding-get.md) ||
|| [`crm_form_site_button`](#crm_form_site_button) | To check which website widgets the form is connected to | [crm.form.sitebutton.list](./placement/crm-form-sitebutton-list.md) ||
|| [`crm_form_open_line`](#crm_form_open_line) | To check in which Open Channels the form is the welcome form | [crm.form.openline.list](./placement/crm-form-openline-list.md) ||
|#

## Data Types

#|
|| **Type** | **Descriptions and Values** ||
|| [`crm_form.id`](#crm_form) | Integer identifier of a CRM form, for example `45`. Form identifiers can be retrieved with [crm.form.list](./crm-form-list.md) ||
|| [`crm_form.updatedTime`](#crm_form) | Form version — the date and time of the last change in ISO 8601 format, for example `2026-10-21T17:44:00+02:00`. Can be retrieved with [crm.form.get](./crm-form-get.md). Required in [crm.form.update](./crm-form-update.md) and [crm.form.embedding.update](./placement/crm-form-embedding-update.md) ||
|| [`crm_form_configuration.entitySchemes.id`](#entity-schemes) | Integer identifier of a CRM scheme, for example `1` — lead. Passed in `crm.entityScheme`. Can be retrieved with [crm.form.configuration.get](./setup/crm-form-configuration-get.md) ||
|| [`crm_form_configuration.dealCategories.id`](#crm_form_configuration) | Integer identifier of a deal pipeline, `0` — the general pipeline. Passed in `crm.dealCategoryId` ||
|| [`crm_form_configuration.smartProcesses.categories.id`](#smart-processes) | Integer identifier of an SPA pipeline. Passed in `crm.smartProcessCategoryId` ||
|| [`crm_form_configuration.fieldTypes.id`](#field-types) | String code of a form field type, for example `string` or `phone`. Passed in `fields[].type` ||
|| [`crm_form_configuration.languages.id`](#crm_form_configuration) | String code of the form language, for example `en`. Passed in `languageId` ||
|| [`crm_form_configuration.paySystems.id`](#crm_form_configuration) | Integer identifier of a payment system. Passed in `payment.disabledPaySystemsId` ||
|| [`crm_form_scenario.id`](#crm_form_scenario) | String code of a scenario, for example `contacts` or `callback`. Passed in `templateId`. Can be retrieved with [crm.form.scenario.list](./setup/crm-form-scenario-list.md) ||
|| [`crm_form_scenario_category.id`](#crm_form_scenario_category) | String code of a scenario category, for example `crm`. Can be retrieved with [crm.form.scenariocategory.list](./setup/crm-form-scenariocategory-list.md) ||
|| [`crm_form_entity_field.name`](#crm_form_entity_field) | String code of a CRM object field, for example `LEAD_NAME`. Passed in `fields[].name`. Can be retrieved with [crm.form.entityfield.list](./setup/crm-form-entityfield-list.md) ||
|| [`crm_form_site_button.id`](#crm_form_site_button) | Integer identifier of a website widget. Can be retrieved with [crm.form.sitebutton.list](./placement/crm-form-sitebutton-list.md) ||
|| [`crm_form_open_line.id`](#crm_form_open_line) | Integer identifier of an Open Channel. Can be retrieved with [crm.form.openline.list](./placement/crm-form-openline-list.md) ||
|#

## How to Read and Write Form Fields

- Without `select`, read methods return only the simple top-level fields: `id`, `code`, `name`, `active`, `system`, `languageId`, `templateId`, `createdTime`, `updatedTime`, `activeChangedById`, `activeChangedTime`. The `appearance`, `crm`, `fields`, and other sections must be requested in `select` explicitly
- In `select`, you can pass an entire section, for example `crm`, or a single section field using dot notation, for example `crm.entityScheme`
- When writing, pass only the fields you want to change. Fields missing from the request remain unchanged
- A section cannot be cleared entirely with `null`: clear individual section fields instead
- The `fields`, `dependencies`, `presetFields`, `responsible.userIds`, `agreements.items`, `payment.disabledPaySystemsId`, and `integrations.ads` collections are replaced entirely. An empty array `[]` clears a collection; `null` is not accepted
- A single collection can contain up to 500 elements, and the whole request up to 2000 elements across all collections
- Fields marked as read-only cannot be passed in `fields`: the method returns the ``` `field` is published for reading only ``` error

## Object Structure

### crm_form {#crm_form}

A CRM form. Returned by [crm.form.get](./crm-form-get.md) and [crm.form.list](./crm-form-list.md), passed in `fields` of [crm.form.add](./crm-form-add.md), [crm.form.update](./crm-form-update.md), and [crm.form.validate](./crm-form-validate.md).

[crm.form.field.list](./crm-form-field-list.md) returns the list of form fields with filtering and sorting flags.

#|
|| **Name**
`type` | **Description** | **Write** ||
|| **id**
[`integer`](../../data-types.md) | Form identifier | Read-only ||
|| **code**
[`string`](../../data-types.md) | Form symbolic code | Read-only ||
|| **name**
[`string`](../../data-types.md) | Form name, up to 50 characters | Required on creation ||
|| **active**
[`boolean`](../../data-types.md) | Form activity.

Possible values:

- `true` — the form is active and accepts submissions
- `false` — the form is deactivated

Passed only in [crm.form.add](./crm-form-add.md), `true` by default. For an existing form, the activity is changed by [crm.form.activate](./crm-form-activate.md) and [crm.form.deactivate](./crm-form-deactivate.md) | On creation only ||
|| **system**
[`boolean`](../../data-types.md) | System form flag. A system form cannot be updated or deleted

Possible values:

- `true` — the form is a system form
- `false` — the form was created by a user | Read-only ||
|| **languageId**
[`string`](../../data-types.md) | Form language. [crm.form.configuration.get](./setup/crm-form-configuration-get.md) returns the list of languages in the `languages` key | Yes ||
|| **templateId**
[`string`](../../data-types.md) | Scenario the form was created from, for example `contacts` or `callback`. [crm.form.scenario.list](./setup/crm-form-scenario-list.md) returns the list of scenarios | Yes ||
|| **createdTime**
[`datetime`](../../data-types.md) | Form creation date and time | Read-only ||
|| **updatedTime**
[`datetime`](../../data-types.md) | Form version — the date and time of the last change. Passed in `updatedTime` of the update methods so as not to overwrite someone else's changes | Read-only ||
|| **activeChangedById**
[`integer`](../../data-types.md) | Identifier of the user who last activated or deactivated the form | Read-only ||
|| **activeChangedTime**
[`datetime`](../../data-types.md) | Date and time the form was last activated or deactivated | Read-only ||
|| **appearance**
[`object`](../../data-types.md) | Form title, description, and button. [Section structure](#appearance) | Yes ||
|| **crm**
[`object`](../../data-types.md) | CRM objects the form creates. [Section structure](#crm) | Yes ||
|| **fields**
[`array`](../../data-types.md) | Form fields. The element structure is described [below](#fields) | Yes ||
|| **dependencies**
[`array`](../../data-types.md) | Dependent field rules. [Element structure](#dependencies) | Yes ||
|| **presetFields**
[`array`](../../data-types.md) | CRM field values the form fills in automatically. [Element structure](#presetFields) | Yes ||
|| **responsible**
[`object`](../../data-types.md) | Responsible persons queue. [Section structure](#responsible) | Yes ||
|| **agreements**
[`object`](../../data-types.md) | Consents the customer accepts. [Section structure](#agreements) | Yes ||
|| **submission**
[`object`](../../data-types.md) | What the customer sees after submitting the form. [Section structure](#submission) | Yes ||
|| **payment**
[`object`](../../data-types.md) | Payment after the form is submitted. [Section structure](#payment) | Yes ||
|| **callback**
[`object`](../../data-types.md) | Callback after the form is submitted. [Section structure](#callback) | Yes ||
|| **booking**
[`object`](../../data-types.md) | Online booking. Contains the `enabled` field of the `boolean` type: `true` if the form has a booking field | Read-only ||
|| **analytics**
[`object`](../../data-types.md) | Web analytics counters. [Section structure](#analytics) | Yes ||
|| **spamProtection**
[`object`](../../data-types.md) | Spam protection. Contains the `enabled` field of the `boolean` type: `true` — protection is enabled, `false` — disabled | Yes ||
|| **integrations**
[`object`](../../data-types.md) | Integrations with messengers and advertising services. [Section structure](#integrations) | Yes ||
|#

#### Appearance Section {#appearance}

#|
|| **Name**
`type` | **Description** | **Write** ||
|| **title**
[`string`](../../data-types.md) | Form title | Yes ||
|| **description**
[`string`](../../data-types.md) | Description below the title | Yes ||
|| **buttonCaption**
[`string`](../../data-types.md) | Submit button text | Yes ||
|| **buttonTextColor**
[`string`](../../data-types.md) | Button text color | Yes ||
|| **buttonBackgroundColor**
[`string`](../../data-types.md) | Button background color | Yes ||
|| **backgroundImageId**
[`integer`](../../data-types.md) | Background image identifier | Read-only ||
|| **copyrightRemoved**
[`boolean`](../../data-types.md) | Whether the "Powered by Bitrix24" label is hidden.

Possible values:

- `true` — the label is hidden
- `false` — the label is shown

If the plan does not allow removing the label, the value `true` is saved as `false` without an error | Yes ||
|#

The form theme cannot be changed via REST yet.

#### Crm Section {#crm}

#|
|| **Name**
`type` | **Description** | **Write** ||
|| **entityScheme**
[`integer`](../../data-types.md) | CRM scheme — the objects created by submitting the form, for example a lead or a deal with a contact. [crm.form.configuration.get](./setup/crm-form-configuration-get.md) returns the scheme identifiers in the `entitySchemes` key.

The scheme determines which CRM fields can be added to the form | Required on creation ||
|| **duplicateMode**
[`string`](../../data-types.md) | What to do if the customer already exists in CRM.

Possible values from the `duplicateModes` key of [crm.form.configuration.get](./setup/crm-form-configuration-get.md):

- `MERGE` — merge data
- `REPLACE` — replace data | Yes ||
|| **dealCategoryId**
[`integer`](../../data-types.md) | Deal pipeline. `0` — the general pipeline, `null` — no pipeline specified. [crm.form.configuration.get](./setup/crm-form-configuration-get.md) returns the pipeline identifiers in the `dealCategories` key | Yes ||
|| **dealDuplicateControlEnabled**
[`boolean`](../../data-types.md) | Duplicate control for deals.

Possible values:

- `true` — control is enabled
- `false` — control is disabled | Yes ||
|| **smartProcessCategoryId**
[`integer`](../../data-types.md) | SPA pipeline. [crm.form.configuration.get](./setup/crm-form-configuration-get.md) returns the pipeline identifiers in the `smartProcesses[].categories` key | Yes ||
|| **smartProcessDuplicateControlEnabled**
[`boolean`](../../data-types.md) | Duplicate control for SPA items.

Possible values:

- `true` — control is enabled
- `false` — control is disabled | Yes ||
|#

#### Fields Element {#fields}

#|
|| **Name**
`type` | **Description** | **Write** ||
|| **name**
[`string`](../../data-types.md) | Field code, for example `LEAD_NAME`. [crm.form.entityfield.list](./setup/crm-form-entityfield-list.md) returns the CRM field codes. The field must belong to the objects of the `crm.entityScheme` scheme. A code can appear in the form only once | Yes ||
|| **type**
[`string`](../../data-types.md) | Field type, for example `string`, `phone`, `email`. [crm.form.configuration.get](./setup/crm-form-configuration-get.md) returns the list of types in the `fieldTypes` key.

Fields with choice options and products cannot be added via REST yet: `list`, `checkbox`, `radio`, `product`, `booking`. Such fields created in the interface are retained on write if their type is not changed | Yes ||
|| **label**
[`string`](../../data-types.md) | Field label | Yes ||
|| **sort**
[`integer`](../../data-types.md) | Field order. Calculated from the element's position in the `fields` array | Read-only ||
|| **required**
[`boolean`](../../data-types.md) | Whether the field is required.

Possible values:

- `true` — the field is required
- `false` — the field is optional | Yes ||
|| **multiple**
[`boolean`](../../data-types.md) | Whether multiple values can be entered.

Possible values:

- `true` — multiple values
- `false` — a single value | Yes ||
|| **placeholder**
[`string`](../../data-types.md) | Hint shown inside an empty field | Yes ||
|| **valueType**
[`string`](../../data-types.md) | Value type for contact fields, for example a work or mobile phone | Yes ||
|| **value**
[`string`](../../data-types.md) | Default value | Yes ||
|#

#### Dependencies Element {#dependencies}

Each element is a group of rules that show or hide fields depending on the customer's answers.

#|
|| **Name**
`type` | **Description** ||
|| **logic**
[`string`](../../data-types.md) | How the group's rules are combined.

Possible values:

- `and` — all rules must be met
- `or` — one rule is enough ||
|| **rules**
[`array`](../../data-types.md) | Group rules. A group must contain at least one rule. Each rule contains the `condition` and `action` objects ||
|| **rules[].condition.fieldName**
[`string`](../../data-types.md) | Code of the form field the rule tracks. The field must be in `fields` of this form ||
|| **rules[].condition.event**
[`string`](../../data-types.md) | Field event. Possible value: `change` — the value has changed ||
|| **rules[].condition.operation**
[`string`](../../data-types.md) | Comparison.

Possible values:

- `=` — equal to
- `!=` or `<>` — not equal to
- `>`, `>=`, `<`, `<=` — greater than, greater than or equal to, less than, less than or equal to — for numeric fields
- `empty` — not filled in
- `any` — filled in
- `contain` — contains — for string fields
- `!contain` — does not contain — for string fields ||
|| **rules[].condition.value**
[`string`](../../data-types.md) | Value to compare with ||
|| **rules[].action.fieldName**
[`string`](../../data-types.md) | Code of the form field the rule acts on ||
|| **rules[].action.type**
[`string`](../../data-types.md) | Action.

Possible values:

- `show` — show the field
- `hide` — hide the field ||
|| **rules[].action.value**
[`string`](../../data-types.md) | Action value ||
|#

#### PresetFields Element {#presetFields}

#|
|| **Name**
`type` | **Description** ||
|| **entityName**
[`string`](../../data-types.md) | CRM object, for example `LEAD` ||
|| **fieldName**
[`string`](../../data-types.md) | Object field code ||
|| **value**
[`string`](../../data-types.md) | Value the form writes to the field ||
|#

#### Responsible Section {#responsible}

#|
|| **Name**
`type` | **Description** ||
|| **userIds**
[`integer[]`](../../data-types.md) | Identifiers of the responsible persons in queue order. An identifier cannot be repeated. User identifiers can be retrieved with [user.get](../../user/user-get.md) ||
|| **checkWorkTime**
[`boolean`](../../data-types.md) | Whether to take working hours into account when choosing the next responsible person.

Possible values:

- `true` — take into account
- `false` — do not take into account ||
|#

#### Agreements Section {#agreements}

#|
|| **Name**
`type` | **Description** ||
|| **enabled**
[`boolean`](../../data-types.md) | Whether to show consents in the form.

Possible values:

- `true` — show
- `false` — do not show ||
|| **items**
[`array`](../../data-types.md) | Consents. Each element contains:

- `agreementId` — consent identifier
- `checked` — whether the consent is pre-selected
- `required` — whether the consent must be accepted ||
|#

#### Submission Section {#submission}

#|
|| **Name**
`type` | **Description** ||
|| **success**
[`object`](../../data-types.md) | Result of a successful submission: `url` — redirect URL, `text` — message text. If both `url` and the `refill` option are set, the URL takes priority ||
|| **failure**
[`object`](../../data-types.md) | Result of a failed submission: `url` — redirect URL, `text` — message text ||
|| **redirectDelay**
[`integer`](../../data-types.md) | Delay before the redirect after a successful submission, from 0 to 10 seconds ||
|| **refill**
[`object`](../../data-types.md) | Offer to fill in the form again: `enabled` — whether the offer is enabled, `caption` — button text ||
|#

#### Payment Section {#payment}

#|
|| **Name**
`type` | **Description** ||
|| **enabled**
[`boolean`](../../data-types.md) | Whether payment is enabled. Payment can be enabled if orders are available in Bitrix24 ||
|| **disabledPaySystemsId**
[`integer[]`](../../data-types.md) | Payment systems the form does not offer. All other Bitrix24 payment systems are available, including those connected later. An empty array enables all payment systems. [crm.form.configuration.get](./setup/crm-form-configuration-get.md) returns the identifiers in the `paySystems` key ||
|#

#### Callback Section {#callback}

#|
|| **Name**
`type` | **Description** ||
|| **enabled**
[`boolean`](../../data-types.md) | Whether callback is enabled. It can be enabled if callback is available in Bitrix24 ||
|| **from**
[`string`](../../data-types.md) | Number Bitrix24 uses to call the customer back. The number must be among the Bitrix24 telephony numbers ||
|| **text**
[`string`](../../data-types.md) | Text the customer hears during the call ||
|#

#### Analytics Section {#analytics}

#|
|| **Name**
`type` | **Description** ||
|| **googleAnalyticsId**
[`string`](../../data-types.md) | Google Analytics identifier ||
|| **googleAnalyticsPageViewEnabled**
[`boolean`](../../data-types.md) | Whether to send page views to Google Analytics ||
|| **yandexMetricaId**
[`string`](../../data-types.md) | Web analytics counter identifier ||
|#

#### Integrations Section {#integrations}

#|
|| **Name**
`type` | **Description** ||
|| **whatsAppEnabled**
[`boolean`](../../data-types.md) | Whether the form is enabled in the messenger. It can be enabled if the feature is available in Bitrix24 ||
|| **ads**
[`array`](../../data-types.md) | Links to advertising service forms. Each element contains:

- `direction` — exchange direction
- `providerCode` — advertising service code
- `accountId` and `accountName` — advertising account identifier and name
- `formId` and `formName` — identifier and name of the form in the advertising service
- `fieldMappings` — field mapping: `crmFieldName` — CRM field, `adsFieldName` — advertising form field ||
|#

### crm_form_configuration {#crm_form_configuration}

Form builder directories. Returned by [crm.form.configuration.get](./setup/crm-form-configuration-get.md) in the `item` key.

#|
|| **Name**
`type` | **Description** ||
|| **entitySchemes**
[`object[]`](../../data-types.md) | CRM object creation schemes: which objects a submitted form creates [(detailed description)](#entity-schemes).

The scheme identifier is passed in the `crm.entityScheme` field when creating and updating a form ||
|| **duplicateModes**
[`object[]`](../../data-types.md) | Modes for handling repeated form submissions: elements with the `id` and `name` fields.

The `id` value is passed in the `crm.duplicateMode` field. On a test Bitrix24, `MERGE` — merge and `REPLACE` — replace are returned ||
|| **lead**
[`object`](../../data-types.md) | Object with the `enabled` field of the `boolean` type:
- `true` — leads are enabled in CRM
- `false` — CRM works in simple mode without leads ||
|| **dealCategories**
[`object[]`](../../data-types.md) | Deal pipelines: elements with the `id` and `name` fields.

The `id` value is passed in the `crm.dealCategoryId` field. The general pipeline has the identifier `0` ||
|| **smartProcesses**
[`object[]`](../../data-types.md) | SPAs whose items the form can create [(detailed description)](#smart-processes) ||
|| **languages**
[`object[]`](../../data-types.md) | Form languages: elements with the `id` and `name` fields.

The `id` value is passed in the `languageId` field ||
|| **currencies**
[`object[]`](../../data-types.md) | Bitrix24 currencies [(detailed description)](#currencies) ||
|| **fieldTypes**
[`object[]`](../../data-types.md) | Field types a form can be built from [(detailed description)](#field-types).

The `id` value is passed in the `fields[].type` field ||
|| **paySystems**
[`object[]`](../../data-types.md) | Payment systems that can be used to pay for a submitted form: elements with the `id` and `name` fields.

The identifiers of the systems the form must not offer are passed in the `payment.disabledPaySystemsId` field. If the `sale` module is not installed in Bitrix24, the array is empty ||
|| **permissions**
[`object`](../../data-types.md) | Current user's permissions for CRM forms:
- `canRead` — can view forms
- `canEdit` — can create, update, and delete forms ||
|| **limits**
[`object`](../../data-types.md) | Object with the `activeForms` field — the active forms limit:
- `current` — how many forms are currently active
- `maximum` — how many forms can be kept active on the plan

If the number of active forms in Bitrix24 is unlimited, `activeForms` is `null` ||
|| **product**
[`object`](../../data-types.md) | Product information:
- `isCloud` — cloud Bitrix24 ||
|| **region**
[`string`](../../data-types.md) | Bitrix24 region code, for example `en` ||
|#

#### EntitySchemes Element {#entity-schemes}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Scheme identifier ||
|| **name**
[`string`](../../data-types.md) | Scheme name ||
|| **description**
[`string`](../../data-types.md) | Scheme description: which objects it creates ||
|| **entityNames**
[`string[]`](../../data-types.md) | Codes of the CRM objects the scheme creates, for example `LEAD`, `CONTACT`, `COMPANY`, `DYNAMIC_174`.

The codes are passed in the filter of [crm.form.entityfield.list](./setup/crm-form-entityfield-list.md) to retrieve the fields for the form ||
|| **mainEntityName**
[`string`](../../data-types.md) | Code of the scheme's main object. For the "Client" scheme — `null` ||
|| **hasInvoice**
[`boolean`](../../data-types.md) | Flag indicating that the scheme creates an invoice ||
|| **dynamic**
[`boolean`](../../data-types.md) | SPA scheme flag ||
|#

#### SmartProcesses Element {#smart-processes}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`](../../data-types.md) | SPA type identifier ||
|| **entityName**
[`string`](../../data-types.md) | SPA code, for example `DYNAMIC_177` ||
|| **name**
[`string`](../../data-types.md) | SPA name ||
|| **categories**
[`object[]`](../../data-types.md) | SPA pipelines: elements with the `id` and `name` fields.

The `id` value is passed in the `crm.smartProcessCategoryId` field ||
|#

#### Currencies Element {#currencies}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Currency code, for example `EUR` ||
|| **fullName**
[`string`](../../data-types.md) | Currency name ||
|| **formatString**
[`string`](../../data-types.md) | Amount output template, `#` is replaced with the amount ||
|#

#### FieldTypes Element {#field-types}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Form field type code, for example `string`, `phone`, `email`, `section`, `hr` ||
|| **name**
[`string`](../../data-types.md) | Field type name ||
|| **parameters**
[`string[]`](../../data-types.md) | Parameters that can be set for a field of this type in the `fields[]` element, for example `label`, `required`, `multiple`, `placeholder`, `valueType`, `value`. For the `hr` and `br` separators, the array is empty ||
|#

{% note info "" %}

The `fieldTypes` directory does not contain field types with choice options: list, checkboxes, radio buttons. Such fields, as well as products and booking, cannot be added to a form via REST yet.

{% endnote %}

### crm_form_scenario {#crm_form_scenario}

Form creation scenario. Returned by [crm.form.scenario.list](./setup/crm-form-scenario-list.md) in the `items` array. The scenario code `id` is passed in the form's `templateId` field.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Scenario code, for example `callback`, `contacts`, `event_registration` ||
|| **title**
[`string`](../../data-types.md) | Scenario name ||
|| **description**
[`string`](../../data-types.md) | Scenario description or `null` ||
|| **categoryId**
[`string`](../../data-types.md) | Scenario category code. [crm.form.scenariocategory.list](./setup/crm-form-scenariocategory-list.md) returns the category names ||
|| **iconUrl**
[`string`](../../data-types.md) | Path to the scenario icon in Bitrix24, or `null` if there is no icon ||
|| **sections**
[`string[]`](../../data-types.md) | Builder sections the scenario opens for setup, for example `fields`, `agreements`, `crm`, `design` ||
|| **expertSections**
[`string[]`](../../data-types.md) | Builder sections the scenario opens in advanced mode. Can be empty ||
|| **defaultSection**
[`string`](../../data-types.md) | Section the builder opens with, or `null` ||
|| **openable**
[`boolean`](../../data-types.md) | Flag indicating that the scenario opens the form builder.

Possible values:

- `true` — the scenario opens the builder
- `false` — the scenario does not open the builder but performs an action from `actions` ||
|| **soon**
[`boolean`](../../data-types.md) | Announced scenario flag.

Possible values:

- `true` — the scenario is announced but not available yet
- `false` — the scenario is available ||
|| **actions**
[`string[]`](../../data-types.md) | Additional scenario actions, for example `showHelp` or `check`. For most scenarios, the array is empty ||
|#

### crm_form_scenario_category {#crm_form_scenario_category}

Scenario category. Returned by [crm.form.scenariocategory.list](./setup/crm-form-scenariocategory-list.md) in the `items` array.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | Category code. Matches the `categoryId` field of a scenario from [crm.form.scenario.list](./setup/crm-form-scenario-list.md) ||
|| **title**
[`string`](../../data-types.md) | Category name ||
|#

### crm_form_entity_field {#crm_form_entity_field}

A CRM object field that can be added to a form. Returned by [crm.form.entityfield.list](./setup/crm-form-entityfield-list.md) in the `items` array.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field code, for example `CONTACT_NAME`. Passed in `fields[].name` when creating or updating a form ||
|| **label**
[`string`](../../data-types.md) | Field name ||
|| **type**
[`string`](../../data-types.md) | Field type, for example `string`, `typed_string`, `list`, `date`, `file`, `address`. Fields of the `list` and `checkbox` types cannot be added to a form via REST ||
|| **required**
[`boolean`](../../data-types.md) | Required field flag in CRM ||
|| **multiple**
[`boolean`](../../data-types.md) | Multiple field flag ||
|| **entityName**
[`string`](../../data-types.md) | Code of the CRM object the field belongs to ||
|| **entityLabel**
[`string`](../../data-types.md) | CRM object name ||
|| **items**
[`object[]`](../../data-types.md) | Value options of a list field: elements with the fields `id` — the value saved in CRM, and `label` — the option name. For other fields, the array is empty ||
|#

### crm_form_embedding {#crm_form_embedding}

Form placement options. Returned by [crm.form.embedding.get](./placement/crm-form-embedding-get.md) without the `variant` parameter in the `item` key. The `click.settings` and `auto.settings` settings are changed by [crm.form.embedding.update](./placement/crm-form-embedding-update.md).

#|
|| **Name**
`type` | **Description** ||
|| **formActive**
[`boolean`](../../data-types.md) | Whether the form is active.

Possible values:

- `true` — the form is active and accepts submissions
- `false` — the form is deactivated. It can be activated with [crm.form.activate](./crm-form-activate.md) ||
|| **resourcesReady**
[`boolean`](../../data-types.md) | Whether the form files are built for publishing.

Possible values:

- `true` — the files are built, the embed codes are ready
- `false` — the files are not built yet, `code` is `null` for all options ||
|| **publicUrl**
[`string`](../../data-types.md) | Public link to the form ||
|| **inline**
[`object`](../../data-types.md) | Option for embedding the form in a website page. The option has no settings, so `settings` and `options` are `null`, [detailed description](#embedding-variant) ||
|| **click**
[`object`](../../data-types.md) | Option for showing the form on a button or link click, [detailed description](#embedding-variant) ||
|| **auto**
[`object`](../../data-types.md) | Option for showing the form automatically after a specified time, [detailed description](#embedding-variant) ||
|#

#### Inline, Click, Auto Objects {#embedding-variant}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../data-types.md) | HTML code for embedding on a website. `null` until the form files are built ||
|| **previewUrl**
[`string`](../../data-types.md) | Link to the form preview in this placement option ||
|| **settings**
[`object`](../../data-types.md) | Current display settings. Available only for the `click` and `auto` options, [detailed description](#embedding-settings) ||
|| **options**
[`object`](../../data-types.md) | Allowed values of the display settings. Available only for the `click` and `auto` options, [detailed description](#embedding-options) ||
|#

#### Settings Object {#embedding-settings}

If a setting value is not saved in the form, `null` is returned. In this case, the default value is applied on the website.

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Form window type.

Possible values:

- `popup` — pop-up window
- `panel` — side panel ||
|| **position**
[`string`](../../data-types.md) | Horizontal window position.

Possible values:

- `left` — left
- `center` — center
- `right` — right ||
|| **vertical**
[`string`](../../data-types.md) | Vertical window position.

Possible values:

- `bottom` — bottom
- `top` — top ||
|| **delay**
[`integer`](../../data-types.md) | Auto-display delay in seconds. Available only for the `auto` option ||
|| **button**
[`object`](../../data-types.md) | Settings of the button that opens the form. Available only for the `click` option, [detailed description](#embedding-button) ||
|#

#### Button Object {#embedding-button}

#|
|| **Name**
`type` | **Description** ||
|| **use**
[`boolean`](../../data-types.md) | Whether to show the Bitrix24 button on the website.

Possible values:

- `true` — show the button with the settings from this object
- `false` — do not show the button ||
|| **text**
[`string`](../../data-types.md) | Button text ||
|| **font**
[`string`](../../data-types.md) | Button font. The allowed values are listed in `click.options.button.font`, for example `modern`, `classic`, `elegant` ||
|| **align**
[`string`](../../data-types.md) | Button alignment. The allowed values are listed in `click.options.button.align`, for example `inline`, `left`, `center`, `right` ||
|| **plain**
[`boolean`](../../data-types.md) | Flag for a button without a background.

Possible values:

- `true` — the button has no background
- `false` — the button has a background ||
|| **rounded**
[`boolean`](../../data-types.md) | Rounded corners flag.

Possible values:

- `true` — the corners are rounded
- `false` — the corners are square ||
|| **outlined**
[`boolean`](../../data-types.md) | Button outline flag.

Possible values:

- `true` — the button has an outline
- `false` — no outline ||
|| **decoration**
[`string`](../../data-types.md) | Button text underline. The interface offers the values `dotted`, `solid`, or an empty value ||
|| **color**
[`object`](../../data-types.md) | Button colors in HEX format, for example `#3bc8f5` or `#3eddffa6` with transparency:

- `text` — text color
- `textHover` — text color on hover
- `background` — background color
- `backgroundHover` — background color on hover ||
|#

#### Options Object {#embedding-options}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string[]`](../../data-types.md) | Allowed values of `settings.type` ||
|| **position**
[`string[]`](../../data-types.md) | Allowed values of `settings.position` ||
|| **vertical**
[`string[]`](../../data-types.md) | Allowed values of `settings.vertical` ||
|| **delay**
[`integer[]`](../../data-types.md) | Allowed values of `settings.delay`. Available only for the `auto` option ||
|| **button**
[`object`](../../data-types.md) | Allowed values of the button settings: `font` and `align`. Available only for the `click` option ||
|#

### crm_form_embedding_selection {#crm_form_embedding_selection}

A single placement option. Returned by [crm.form.embedding.get](./placement/crm-form-embedding-get.md) with the `variant` parameter in the `item` key.

#|
|| **Name**
`type` | **Description** ||
|| **variant**
[`string`](../../data-types.md) | Selected placement option: `inline`, `click`, `auto`, or `link` ||
|| **code**
[`string`](../../data-types.md) | HTML code for embedding on a website. Returned for the `inline`, `click`, and `auto` options ||
|| **url**
[`string`](../../data-types.md) | Public link to the form. Returned for the `link` option ||
|#

### crm_form_site_button {#crm_form_site_button}

Website widget. Returned by [crm.form.sitebutton.list](./placement/crm-form-sitebutton-list.md) in the `items` array. A form cannot be linked to a widget via REST.

#|
|| **Name**
`type` | **Description** ||
|| **formId**
[`integer`](../../data-types.md) | CRM form identifier from the request filter ||
|| **id**
[`integer`](../../data-types.md) | Widget identifier ||
|| **name**
[`string`](../../data-types.md) | Widget name ||
|| **system**
[`boolean`](../../data-types.md) | System widget flag.

Possible values:

- `true` — the widget was created by Bitrix24 automatically
- `false` — the widget was created by a user ||
|| **bound**
[`boolean`](../../data-types.md) | Whether the form from the filter is connected to this widget.

Possible values:

- `true` — the form is in an active widget slot
- `false` — the form is not connected to the widget ||
|| **formsId**
[`integer[]`](../../data-types.md) | Identifiers of all CRM forms connected to the widget ||
|| **previewUrl**
[`string`](../../data-types.md) | Link to the widget preview. The form from the filter is visible in the preview only if `bound` is `true` ||
|#

### crm_form_open_line {#crm_form_open_line}

Open Channel. Returned by [crm.form.openline.list](./placement/crm-form-openline-list.md) in the `items` array. A form cannot be set as the welcome form via REST.

#|
|| **Name**
`type` | **Description** ||
|| **formId**
[`integer`](../../data-types.md) | CRM form identifier from the request filter ||
|| **id**
[`integer`](../../data-types.md) | Open Channel identifier ||
|| **name**
[`string`](../../data-types.md) | Open Channel name ||
|| **bound**
[`boolean`](../../data-types.md) | Whether the form from the filter is the welcome form of this Open Channel.

Possible values:

- `true` — the form is set as the welcome form
- `false` — the form is not set as the welcome form ||
|| **welcomeFormEnabled**
[`boolean`](../../data-types.md) | Whether a welcome form is enabled in the Open Channel: this one or another.

Possible values:

- `true` — the welcome form is enabled
- `false` — the welcome form is disabled ||
|| **welcomeFormId**
[`integer`](../../data-types.md) | Identifier of the Open Channel welcome form. `null` if no form is selected ||
|| **welcomeFormDelayed**
[`boolean`](../../data-types.md) | When the welcome form is shown.

Possible values:

- `true` — after the visitor's first message
- `false` — before the visitor's first message ||
|| **previewUrl**
[`string`](../../data-types.md) | Link to the preview of the website widget with this Open Channel. `null` if the Open Channel is not connected to any active widget. The form from the filter is visible in the preview only if `bound` is `true` ||
|#

## User Documentation

- [Create and Configure CRM Forms](https://helpdesk.bitrix24.com/open/24977144/)
- [Change CRM Form Title, Name, and Page Name](https://helpdesk.bitrix24.com/open/25196454/)
- [Collect Customer Consent via a CRM Form](https://helpdesk.bitrix24.com/open/25821939/)
- [Configure Field Show Rules in CRM Forms](https://helpdesk.bitrix24.com/open/24300716/)
- [CRM Forms: How to Fix Errors in Field Display Rules Settings](https://helpdesk.bitrix24.com/open/24323658/)
- [Autocomplete Fields in CRM Forms](https://helpdesk.bitrix24.com/open/25071358/)
- [Default Values for Hidden Fields in CRM Forms](https://helpdesk.bitrix24.com/open/25769127/)
- [Create Personalized CRM Forms](https://helpdesk.bitrix24.com/open/25764959/)
- [Configure reCAPTCHA V2 for CRM Forms](https://helpdesk.bitrix24.com/open/25085158/)

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-form-add.md)
- [{#T}](./crm-form-update.md)
- [{#T}](./setup/crm-form-configuration-get.md)
- [{#T}](./placement/crm-form-embedding-get.md)
