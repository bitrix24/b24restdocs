# Field Visibility Settings

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

A set of methods for managing field visibility settings.

In addition to custom fields, you can manage the visibility of system fields in the kanban card.

System fields have the following codes:

- `id` - id of the entity
- `createdBy` - who created it
- `updatedBy` - who modified it
- `movedBy` - who changed the stage
- `createdTime` - creation time
- `updatedTime` - modification time
- `movedTime` - stage change time

#|
|| **Method** | **Description** ||
|| [rpa.fields.getSettings](./rpa-fields-get-settings.md) | This method returns the complete set of field visibility settings for the stage with the identifier stageId of the process with the identifier typeId. ||
|| [rpa.fields.setSettings](./rpa-fields-set-settings.md) | This method sets the complete set of field visibility settings for the stage with the identifier stageId of the process with the identifier typeId. ||
|| [rpa.fields.setVisibilitySettings](./rpa-fields-set-visibility-settings.md) | This method modifies the visibility settings of fields for the process with the identifier typeId at the stage with the identifier stageId. ||
|#