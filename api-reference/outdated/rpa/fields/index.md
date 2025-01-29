# Field Visibility Settings: Overview of Methods

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

A set of methods for managing field visibility settings

In addition to custom fields, you can manage the visibility of system fields in the kanban card.

System fields have the following codes:

- `id` — element identifier
- `createdBy` — who created it
- `updatedBy` — who modified it
- `movedBy` — who changed the stage
- `createdTime` — creation time
- `updatedTime` — modification time
- `movedTime` — stage change time

#|
|| **Method** | **Description** ||
|| [rpa.fields.getSettings](./rpa-fields-get-settings.md) | Retrieves the complete set of field visibility settings for the stage with identifier `stageId` of the process with identifier `typeId` ||
|| [rpa.fields.setSettings](./rpa-fields-set-settings.md) | Sets the complete set of field visibility settings for the stage with identifier `stageId` of the process with identifier `typeId` ||
|| [rpa.fields.setVisibilitySettings](./rpa-fields-set-visibility-settings.md) | Changes the visibility settings `visibility` of fields `fields` for the process with identifier `typeId` at the stage with identifier `stageId` ||
|#