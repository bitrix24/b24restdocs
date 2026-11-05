# CRM Pipelines and Stages in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These methods belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

A pipeline is a separate set of CRM items within an object type. For deals and SPAs, a pipeline defines the path of an item through stages — from the first stage to success or failure. For example, a requests SPA can have a pipeline for equipment procurement with its own stages: request preparation, contract review, budget approval. The [pipeline methods](#categories-methods) create and update pipelines, and the [stage methods](#stages-methods) do the same for the stages within a pipeline.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Sales Pipelines](https://helpdesk.bitrix24.com/open/20739996/)

## Object Types

The object type is set by the `<type>` segment of the method name: `crm.deal.category.add` creates a deal pipeline, and `crm.smartprocess1038.category.add` creates a pipeline of the SPA with `entityTypeId = 1038`.

#|
|| **Methods** | **Object types** ||
|| `crm.<type>.category.*` | Deal `deal`, contact `contact`, company `company`, SPA `smartprocess<entityTypeId>` ||
|| `crm.<type>.category.stage.*` | Deal `deal`, SPA `smartprocess<entityTypeId>` ||
|#

An SPA has pipeline methods only if pipelines are enabled in the type settings, and stage methods only if both pipelines and stages are enabled. Leads, estimates, invoices, and my company have no pipeline or stage methods.

SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. Method names are written in lowercase: `crm.smartprocess1038.category.list`.

## Getting Started

1. Check which pipelines already exist with the [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) method
2. Create a pipeline with the [crm.&lt;type&gt;.category.add](./crm-entity-category-add-rest-v3.md) method. Bitrix24 adds default stages to it: always for a deal, and for an SPA if stages are enabled in the type
3. Retrieve the pipeline stages with the [crm.&lt;type&gt;.category.stage.list](./crm-entity-category-stage-list-rest-v3.md) method — you will need the `stageId` values
4. Configure stages one by one with the [crm.&lt;type&gt;.category.stage.add](./crm-entity-category-stage-add-rest-v3.md) and [crm.&lt;type&gt;.category.stage.update](./crm-entity-category-stage-update-rest-v3.md) methods. All in-progress stages of the `process` group can be set in a single request with the [crm.&lt;type&gt;.category.stage.replace](./crm-entity-category-stage-replace-rest-v3.md) method
5. Create a deal or SPA item in the pipeline with the [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md) method and the `categoryId` field

## Default Pipeline

One of the pipelines of an object type is the default pipeline, which has `isDefault: true`:

- deal — always the pipeline with `id` `0`
- contact and company — pipeline `0`, until it is changed
- SPA — a regular pipeline with its own ID, for example, `69`

Pipeline `0` cannot be deleted.

For a contact, a company, and an SPA, the default pipeline can be changed with the `isDefault: true` field in the [crm.&lt;type&gt;.category.add](./crm-entity-category-add-rest-v3.md) and [crm.&lt;type&gt;.category.update](./crm-entity-category-update-rest-v3.md) methods. For a deal, `isDefault` is read-only.

{% note warning "" %}

Contact and company pipelines can be created and configured, but an item cannot be placed in them through REST 3.0. The [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md) method creates contacts and companies only in pipeline `0`, and [crm.&lt;type&gt;.update](../items/crm-entity-update-rest-v3.md) does not move them to another pipeline.

{% endnote %}

## Stages and Their Order

Pipeline stages follow the `sort` order and fall into three groups — the `semantics` field:

- `process` — in-progress stages, which come first
- `success` — the successful stage, only one per pipeline
- `failure` — failed stages, which come after the successful stage

Bitrix24 checks this order on every stage change. If a new stage or a new `sort` value breaks the order, the methods return the `OPERATION_NOT_ALLOWED_FOR_STAGE` error.

Bitrix24 creates the first stage, the successful stage, and the main failed stage together with the pipeline, and they have `isSystem: true`. System stages can be renamed with the [crm.&lt;type&gt;.category.stage.update](./crm-entity-category-stage-update-rest-v3.md) method, but they cannot be deleted or moved to another group.

The stage ID includes the pipeline ID:

#|
|| **Pipeline** | **Format** | **Example** ||
|| Default deal pipeline | Stage code without a prefix | `NEW` ||
|| Other deal pipeline | `C<categoryId>:<code>` | `C1:NEW` ||
|| SPA pipeline | `DT<entityTypeId>_<categoryId>:<code>` | `DT1038_127:NEW` ||
|#

## Lists of Pipelines and Stages

The [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) and [crm.&lt;type&gt;.category.stage.list](./crm-entity-category-stage-list-rest-v3.md) methods accept pagination, with up to 100 records per page. The stage method also requires a pipeline filter as a single condition: `"filter": ["categoryId", "=", 127]`.

The methods skip the `order` and `select` parameters without an error, and the pipeline method also skips `filter`. Records are returned in ascending `sort` order, each with all its fields.

## Permissions

A pipeline and its stages can be read by a user with permission to read the items of this pipeline. The list of pipelines includes only the pipelines the user can read. The methods treat a pipeline without read permission as nonexistent.

Pipelines can be created, updated, and deleted by a CRM administrator or a user with the "Manage pipelines" permission in a CRM access role. Stages require the "Manage stages" permission. For an SPA of a digital workplace, Bitrix24 checks permissions in the roles of this digital workplace.

## Pipeline and Stage Errors

The errors from the table below are returned with the `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION` code. Their `field` contains the error code, not a field name:

#|
|| **Code in field** | **When it occurs** ||
|| `DEPENDENT_ITEMS_EXIST` | Deleting a pipeline or a stage that has items ||
|| `OPERATION_NOT_ALLOWED_FOR_CATEGORY` | The pipeline cannot be deleted, for example, pipeline `0` of a deal, contact, or company ||
|| `OPERATION_NOT_ALLOWED_FOR_STAGE` | The change breaks the stage order or affects a system stage ||
|| `STAGE_NOT_FOUND` | There is no stage with the passed ID ||
|| `FIELD_VALUE_NOT_ALLOWED` | The name consists of spaces only ||
|#

If the pipeline does not exist, the methods return the `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` error. If the user has no permission to configure pipelines or stages, the write methods return the `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` error with HTTP status **403**. Common REST 3.0 errors are described in the [REST 3.0 overview](../../../rest-v3.md).

## Relationship with Other Objects

Pipelines are linked to CRM items, the stage group directory, and card views.

**Items.** A deal or SPA item is in one pipeline and at one of its stages — the `categoryId` and `stageId` fields. They are passed to the [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md) and [crm.&lt;type&gt;.update](../items/crm-entity-update-rest-v3.md) methods.

**Stage groups.** Stage group names are returned by the [crm.dictionary.stagesemantic.list](../dictionaries/crm-dictionary-list-rest-v3.md) method. The directory names the groups with the codes `P`, `S`, and `F`, and the stage methods use the values `process`, `success`, and `failure`.

**Card views.** Item card views are stored separately for each pipeline. The pipeline ID is passed in the `categoryId` parameter of the [crm.&lt;type&gt;.detailpagelayout.*](../detail-page-layout/index.md) methods.

## Methods of the Previous Version

At the `/rest/` address, pipelines are handled by the [crm.category.*](../../universal/category/index.md) methods, which take the object type in the `entityTypeId` parameter. In the previous version, stages are directory values, and they are handled by the [crm.status.*](../../status/index.md) methods. The stage directory is set by the `ENTITY_ID` parameter, for example, `DEAL_STAGE_1` for the deal pipeline with ID `1`. In REST 3.0, the object type is part of the method name, and stages have their own `crm.<type>.category.stage.*` methods.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Pipelines {#categories-methods}

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.category.add](./crm-entity-category-add-rest-v3.md) | Creates a pipeline ||
|| [crm.&lt;type&gt;.category.update](./crm-entity-category-update-rest-v3.md) | Updates the name, order, or default pipeline flag of a pipeline ||
|| [crm.&lt;type&gt;.category.get](./crm-entity-category-get-rest-v3.md) | Returns a pipeline by ID ||
|| [crm.&lt;type&gt;.category.list](./crm-entity-category-list-rest-v3.md) | Returns the pipelines of an object type ||
|| [crm.&lt;type&gt;.category.delete](./crm-entity-category-delete-rest-v3.md) | Deletes an empty pipeline together with its stages ||
|| [crm.&lt;type&gt;.category.field.list](#field-methods) | Returns pipeline field descriptions ||
|| [crm.&lt;type&gt;.category.field.get](#field-methods) | Returns a pipeline field description by name ||
|#

### Stages {#stages-methods}

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.category.stage.add](./crm-entity-category-stage-add-rest-v3.md) | Adds a stage to a pipeline ||
|| [crm.&lt;type&gt;.category.stage.update](./crm-entity-category-stage-update-rest-v3.md) | Updates a stage ||
|| [crm.&lt;type&gt;.category.stage.get](./crm-entity-category-stage-get-rest-v3.md) | Returns a stage by ID ||
|| [crm.&lt;type&gt;.category.stage.list](./crm-entity-category-stage-list-rest-v3.md) | Returns the stages of a pipeline ||
|| [crm.&lt;type&gt;.category.stage.delete](./crm-entity-category-stage-delete-rest-v3.md) | Deletes a stage that has no items ||
|| [crm.&lt;type&gt;.category.stage.replace](./crm-entity-category-stage-replace-rest-v3.md) | Sets the entire set of in-progress stages in a single request ||
|| [crm.&lt;type&gt;.category.stage.field.list](#field-methods) | Returns stage field descriptions ||
|| [crm.&lt;type&gt;.category.stage.field.get](#field-methods) | Returns a stage field description by name ||
|#

### Pipeline and Stage Field Descriptions {#field-methods}

Field descriptions with the required and editable flags are returned by the following methods:

- pipeline fields — `crm.<type>.category.field.list` and `crm.<type>.category.field.get`
- stage fields — `crm.<type>.category.stage.field.list` and `crm.<type>.category.stage.field.get`

The response format is the same as for the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) method.
