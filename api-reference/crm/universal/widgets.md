# Widgets Embedded in the Interface of Custom CRM Object Types

Widgets allow you to integrate custom functionality directly into the Bitrix24 interface. We have previously described this mechanism in the section [{#T}](../../widgets/index.md).

Unlike standard embedding locations for widgets (such as in deals), for custom CRM object types, it is important to note that these locations become available only after specific custom object types are added to Bitrix24.

The identifiers of these types are used in the symbolic codes of the widget embedding locations. Suppose a custom type "Vendors" has been added to Bitrix24, and the identifier for this custom object type is `131`. In this case, the following locations will be available for embedding widgets:

- Tab in the detail form of the entity with the code `CRM_SMART_131_DETAIL_TAB`
- `timeline` button in the entity card with the code `CRM_SMART_131_DETAIL_ACTIVITY`

## CRM Widgets

- [{#T}](../../widgets/crm/index.md)
- [{#T}](../../widgets/crm/list-toolbar.md)
- [{#T}](../../widgets/crm/detail-tab.md)
- [{#T}](../../widgets/crm/detail-activity.md)
- [{#T}](../../widgets/crm/detail-toolbar.md)
- [{#T}](../../widgets/crm/activity-timeline-menu.md)
- [{#T}](../../widgets/crm/document-generator-button.md)
- [{#T}](../../widgets/crm/automation-rule-designer-toolbar.md)
- [{#T}](../../widgets/crm/funnels-toolbar.md)
- [{#T}](../../widgets/crm/analytics-menu.md)
- [{#T}](../../widgets/crm/analytics-toolbar.md)