# Overview of Methods

Each **SPA** is a new entity within the CRM, for which a new section is created with its own interface, functionality, fields, elements, etc. You can read more in the [documentation](https://helpdesk.bitrix24.com/open/19141012/).

The workflow for working with an SPA is as follows:

1. A new SPA is created using [crm.type.add](./crm-type-add.md).
2. When creating an SPA, a default direction will be created regardless of the settings. If desired, it can be [changed/added](#sales-funnels).
3. When creating a direction, a default set of stages will be created regardless of the settings. If desired, they can be [changed/removed](#stages-of-spas).
4. Custom fields are created for this SPA. [Learn more](../user-defined-fields/index.md)
5. You can work with its [elements](#elements-of-spas).
6. The SPA can be brought into the Digital Workplace. [Learn more](../../automated-solution/index.md)
7. You can configure both personal and shared detail forms for SPA elements. [Learn more](#managing-the-settings-of-the-spa-element-detail-form)
8. There is an option to enhance functionality through events.


## Methods for Working with SPAs

- [{#T}](./crm-type-fields.md) 
- [{#T}](./crm-type-add.md)
- [{#T}](./crm-type-update.md)
- [{#T}](./crm-type-get.md)
- [{#T}](./crm-type-get-by-entity-type-id.md)
- [{#T}](./crm-type-list.md)
- [{#T}](./crm-type-delete.md)


## Elements of SPAs

Methods for working with SPA elements are described in a separate [section](../index.md)

### Methods for Working with SPA Elements

- [{#T}](../crm-item-fields.md)
- [{#T}](../crm-item-add.md)
- [{#T}](../crm-item-update.md)
- [{#T}](../crm-item-get.md)
- [{#T}](../crm-item-list.md)
- [{#T}](../crm-item-delete.md)


## Sales Funnels of SPAs

To work with sales funnels of SPAs, a required parameter `entityTypeId` is necessary, which can be obtained through the methods [`crm.type.add`](crm-type-add.md) or [`crm.type.list`](crm-type-list.md)

### Default Directions

Each entity type can only have one default direction at a time. As a result, there are several restrictions:

- The default direction cannot be deleted;
- When creating a new direction and passing the flag `"isDefault": "Y"`, the old default direction will cease to be the default direction;
- When changing the default direction, it cannot be made a non-default direction;
- When changing a non-default direction and passing the flag `"isDefault": "Y"`, the old default direction will cease to be the default direction.

If the display of directions in the interface is disabled for an existing SPA, working with directions through REST is still possible.

### Methods for Working with Sales Funnels

- [{#T}](../category/crm-category-fields.md)
- [{#T}](../category/crm-category-add.md)
- [{#T}](../category/crm-category-update.md)
- [{#T}](../category/crm-category-get.md)
- [{#T}](../category/crm-category-list.md)
- [{#T}](../category/crm-category-delete.md)


## Stages of SPAs

### ENTITY_ID

The `ENTITY_ID` field for SPA stages has the following format: `DYNAMIC_{entityTypeId}_STAGE_{categoryId}`,

where:
- `{entityTypeId}` - the identifier of the CRM SPA type;
- `{categoryId}` - the identifier of the direction to which the stage belongs.

### STATUS_ID

The `STATUS_ID` field for SPA stages must have the prefix `DT{entityTypeId}_{categoryId}`,

where
- `{entityTypeId}` - the identifier of the CRM SPA type;
- `{categoryId}` - the identifier of the direction to which the stage belongs.

### Example

`crm.status.add(fields)`

To create a new stage of an SPA with `entityTypeId = 135` and `categoryId = 20`, the `fields` parameter should look like this:

```json
{
    "fields": {
        "COLOR": "#1111AA",
        "NAME": "My new stage",
        "SORT": 250,
        "ENTITY_ID": "DYNAMIC_135_STAGE_20",
        "STATUS_ID": "DT135_20:MY_STAGE_REST"
    }
}
```

### Methods for Working with Stages

- [{#T}](./../../status/crm-status-fields.md)
- [{#T}](./../../status/crm-status-add.md)
- [{#T}](./../../status/crm-status-update.md)
- [{#T}](./../../status/crm-status-get.md)
- [{#T}](./../../status/crm-status-list.md)
- [{#T}](./../../status/crm-status-delete.md)

## Managing the Settings of the SPA Element Detail Form

Methods for managing the settings of the SPA element detail form provide the ability to configure the display and behavior of detail forms in the CRM. These methods work similarly to existing methods for managing the settings of deal, contact, and other CRM entity detail forms, such as [`crm.deal.details.configuration.*`](../../deals/custom-form/index.md), [`crm.contact.details.configuration.*`](../../contacts/custom-form/index.md), and so on.

- [{#T}](../item-details-configuration/crm-item-details-configuration-get.md)
- [{#T}](../item-details-configuration/crm-item-details-configuration-set.md)
- [{#T}](../item-details-configuration/crm-item-details-configuration-reset.md)
- [{#T}](../item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md)

## Events on SPAs

- [{#T}](../events/type/on-crm-type-add.md)
- [{#T}](../events/type/on-crm-type-update.md)
- [{#T}](../events/type/on-crm-type-delete.md)

## Continue Learning

- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md)