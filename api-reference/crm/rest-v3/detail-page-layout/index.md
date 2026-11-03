# CRM Card Views in REST 3.0: Overview of Methods

A card view defines which sections and fields a CRM item card shows and in what order. The `crm.<type>.detailpagelayout.*` methods create and update card views, select the view a user sees, and return the common view to all users.

{% note info "" %}

These methods belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

> Quick navigation: [all methods](#all-methods)

## Personal, Common, and Additional Views

In each pipeline, a card has views of three kinds.

#|
|| **View** | **Who sees it** | **ID** | **How to change it** ||
|| Personal | A single user. Each user has their own personal view | `C69:personal`; for a lead and an estimate, `personal` | A user changes their own view, and a CRM administrator can change the view of any user ||
|| Common | All users who have not selected another view | `C69:common`; for a lead and an estimate, `common` | A user with permission to edit the common card view, for example, a CRM administrator ||
|| Additional | Members of the audience: employees, departments, groups, company structure nodes | A number in a string, for example, `"17"` | A user with permission to edit the common card view ||
|#

The personal and common views always exist and cannot be created or deleted. If they have not been configured, the methods return the default view. A pipeline can have several additional views, each with its own name and audience. They are created by the [crm.&lt;type&gt;.detailpagelayout.add](./crm-entity-detail-page-layout-add-rest-v3.md) method.

## View ID and Pipeline

Card views are stored separately for each pipeline, so all methods accept the `categoryId` parameter:

- leads and estimates have no pipelines — pass `0`
- for deals, contacts, and companies, pass the pipeline ID; `0` is the default pipeline
- for invoices, documents, and SPAs, pass the pipeline ID. The method replaces the value `0` with the ID of the default pipeline

Pipeline IDs of deals, contacts, companies, and SPAs are returned by the [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) method.

The ID of the personal and common view contains the pipeline number: `C<categoryId>:personal` and `C<categoryId>:common`. For example, for an SPA with the default pipeline `69`, these are `C69:personal` and `C69:common`. Pass the ID together with the `categoryId` of the same pipeline; otherwise, the methods return the `INVALID_LAYOUT_ID` error. Ready-made IDs are returned by the [crm.&lt;type&gt;.detailpagelayout.getpersonallayoutid](./crm-entity-detail-page-layout-get-personal-layout-id-rest-v3.md) and [crm.&lt;type&gt;.detailpagelayout.getcommonlayoutid](./crm-entity-detail-page-layout-get-common-layout-id-rest-v3.md) methods.

Leads have separate views for repeat leads: `personal_returning_customer` and `common_returning_customer`. Pass them in the view ID parameter with `categoryId` set to `0`.

## How a View Is Structured

A view is an array of card sections `configuration`. A section has a code `name`, a title `title`, and fields `elements` in display order. A field is specified by its name in the card format: `TITLE`, `STAGE_ID`, `OPPORTUNITY_WITH_CURRENCY`, `UF_CRM_1687529546`. These names are written in uppercase and differ from the item field names in REST 3.0, for example, `title` and `stageId`.

The methods do not validate field names: a view with a nonexistent field is saved. Take a ready-made set of field names and settings from the common view with the [crm.&lt;type&gt;.detailpagelayout.get](./crm-entity-detail-page-layout-get-rest-v3.md) method, modify it, and pass it to [crm.&lt;type&gt;.detailpagelayout.update](./crm-entity-detail-page-layout-update-rest-v3.md) or [crm.&lt;type&gt;.detailpagelayout.add](./crm-entity-detail-page-layout-add-rest-v3.md).

## Current User View

The current view is the one the user sees in the card. If the user has not selected anything, the common view is considered current.

1. Find out the current view with the [crm.&lt;type&gt;.detailpagelayout.getcurrent](./crm-entity-detail-page-layout-get-current-rest-v3.md) method.
2. Select another view with the [crm.&lt;type&gt;.detailpagelayout.setcurrent](./crm-entity-detail-page-layout-set-current-rest-v3.md) method. An additional view can be selected only if the user is in its audience.
3. To make all users see the common view again, call [crm.&lt;type&gt;.detailpagelayout.setcommonforall](./crm-entity-detail-page-layout-set-common-for-all-rest-v3.md). The method resets the selection but does not delete personal and additional views.

A new additional view does not become current on its own, even for members of its audience. It is selected in the card or with the `setcurrent` method.

## Permissions

Any user can view card views and select their own current view.

A user with permission to edit the common card view of this object type, for example, a CRM administrator, can:

- edit the common view
- create, edit, and delete additional views
- apply the common view to all users

The `userId` parameter lets you act on behalf of another user: retrieve their current view, select a view for them, edit their personal view. Only a CRM administrator can pass the `userId` of another user.

## Card View Errors

Card view errors are returned with short codes without the `BITRIX_REST_V3_` prefix and with English messages:

#|
|| **Code** | **HTTP status** | **When it occurs** ||
|| `INVALID_LAYOUT_ID` | 400 | Invalid view ID format, or the view belongs to another pipeline ||
|| `UNSUPPORTED_CONTEXT` | 400 | This object type has no pipeline with the `categoryId` passed ||
|| `INVALID_LAYOUT_CONFIGURATION` | 400 | Empty `configuration` array, invalid view name ||
|| `INVALID_LAYOUT_AUDIENCE` | 400 | Invalid or empty audience of an additional view ||
|| `PERMISSION_DENIED` | 403 | No permission for the action with the view ||
|| `LAYOUT_NOT_FOUND` | 404 | The additional view does not exist or is not available to the user ||
|| `STORAGE_FAILED` | 500 | Failed to save or read the view ||
|| `CONSISTENCY_FAILURE` | 500 | The changes were applied only partially ||
|#

Request validation errors are returned in the common REST 3.0 format with `BITRIX_REST_V3_*` codes.

## Relationship with Other Objects

Card views are linked to object types, pipelines, and employees.

**Object type.** The methods are available for deals, leads, contacts, companies, estimates, invoices, CRM documents, B2E documents, and SPAs. The object type is set by the `<type>` segment of the method name; for an SPA, it is `smartprocess<entityTypeId>`, for example, `crm.smartprocess1038.detailpagelayout.get`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. My company has no card view methods.

**Pipeline.** Each pipeline stores its own views. The pipeline ID is passed in `categoryId`, and the list of pipelines is returned by the [crm.&lt;type&gt;.category.list](../categories/crm-entity-category-list-rest-v3.md) method.

**Employees and departments.** The audience of an additional view is set by the `audience` array: employees from [user.get](../../../user/user-get.md), departments from [department.get](../../../departments/department-get.md), groups and projects from [sonet_group.get](../../../sonet-group/sonet-group-get.md), and company structure nodes.

**Item fields.** A view contains fields of the object type, including custom `UF_CRM_*` fields. The list of custom fields is returned by the [crm.&lt;type&gt;.field.custom.list](../fields/crm-entity-field-custom-list-rest-v3.md) method.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Card Views

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.detailpagelayout.add](./crm-entity-detail-page-layout-add-rest-v3.md) | Creates an additional card view ||
|| [crm.&lt;type&gt;.detailpagelayout.update](./crm-entity-detail-page-layout-update-rest-v3.md) | Updates a personal, common, or additional view ||
|| [crm.&lt;type&gt;.detailpagelayout.get](./crm-entity-detail-page-layout-get-rest-v3.md) | Returns a card view by ID ||
|| [crm.&lt;type&gt;.detailpagelayout.list](./crm-entity-detail-page-layout-list-rest-v3.md) | Returns additional card views in a pipeline ||
|| [crm.&lt;type&gt;.detailpagelayout.delete](./crm-entity-detail-page-layout-delete-rest-v3.md) | Deletes an additional card view ||
|#

### Current View

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.detailpagelayout.getcurrent](./crm-entity-detail-page-layout-get-current-rest-v3.md) | Returns the view the user sees in the card ||
|| [crm.&lt;type&gt;.detailpagelayout.setcurrent](./crm-entity-detail-page-layout-set-current-rest-v3.md) | Selects the current view for a user ||
|| [crm.&lt;type&gt;.detailpagelayout.setcommonforall](./crm-entity-detail-page-layout-set-common-for-all-rest-v3.md) | Makes the common view current for all users ||
|#

### Personal and Common View IDs

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.detailpagelayout.getpersonallayoutid](./crm-entity-detail-page-layout-get-personal-layout-id-rest-v3.md) | Returns the personal view ID in a pipeline ||
|| [crm.&lt;type&gt;.detailpagelayout.getcommonlayoutid](./crm-entity-detail-page-layout-get-common-layout-id-rest-v3.md) | Returns the common view ID in a pipeline ||
|#

### View Field Descriptions

The group has the `crm.<type>.detailpagelayout.field.list` and `crm.<type>.detailpagelayout.field.get` methods. They return descriptions of the card view fields — `id`, `configuration`, `name`, `audience` — with required and editable attributes. The response format is the same as for the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) method.
