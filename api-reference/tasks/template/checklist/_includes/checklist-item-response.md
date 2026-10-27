#### Object checkListItem {#checklistitem}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](/api-reference/data-types.html) | Identifier of the checklist item ||
|| **copiedId**
[`integer`](/api-reference/data-types.html) | Identifier of the original item if the item was created by copying. For items created via REST, `null` is returned ||
|| **userId**
[`integer`](/api-reference/data-types.html) | Identifier of the user who made the request ||
|| **createdBy**
[`integer`](/api-reference/data-types.html) | Identifier of the item creator. Populated in the response of the `tasks.template.checklist.add` method; other methods return `null` ||
|| **parentId**
[`integer`](/api-reference/data-types.html) | Identifier of the parent item. A value of `0` indicates a root item, which is the checklist title ||
|| **title**
[`string`](/api-reference/data-types.html) | Text of the checklist item ||
|| **sortIndex**
[`integer`](/api-reference/data-types.html) | Sorting index among items at the same level. The lower the value, the higher the item ||
|| **displaySortIndex**
[`string`](/api-reference/data-types.html) | Service field for display order. For template items, an empty string is returned ||
|| **isComplete**
[`boolean`](/api-reference/data-types.html) | Status of the item completion: `true` — completed, `false` — not completed ||
|| **isImportant**
[`boolean`](/api-reference/data-types.html) | Importance flag of the item: `true` — important, `false` — regular ||
|| **completedCount**
[`integer`](/api-reference/data-types.html) | Service counter of completed subitems. For template items, `0` is returned ||
|| **members**
[`array`](/api-reference/data-types.html) | Array of objects with [description of participants](#members).

If there are no participants, an empty array `[]` is returned ||
|| **attachments**
[`object`](/api-reference/data-types.html) | Object with [description of attachments](#attachments).

If there are no attachments, an empty array `[]` is returned ||
|| **nodeId**
[`integer`](/api-reference/data-types.html) | Service field. For template items, `null` is returned ||
|| **templateId**
[`integer`](/api-reference/data-types.html) | Identifier of the task template ||
|#

#### Object members {#members}

The object structure depends on the method:

- the `tasks.template.checklist.add` and `tasks.template.checklist.update` methods return participants in short form if `fields.MEMBERS` is passed in the request: only `id` of type `integer` and `type`
- in all other cases, the full set of fields from the table is returned, and `id` is of type `string`

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](/api-reference/data-types.html) \| [`integer`](/api-reference/data-types.html) | Identifier of the user ||
|| **type**
[`string`](/api-reference/data-types.html) | Role of the user in the checklist item. Possible values:
- `A` — Participant
- `U` — Observer ||
|| **name**
[`string`](/api-reference/data-types.html) | User's first and last name ||
|| **personalPhoto**
[`string`](/api-reference/data-types.html) | Identifier of the user's avatar file ||
|| **personalGender**
[`string`](/api-reference/data-types.html) | Gender of the user. Possible values:
- `M` — Male
- `F` — Female
- empty string — not specified ||
|| **image**
[`string`](/api-reference/data-types.html) | Link to the user's avatar ||
|| **isCollaber**
[`boolean`](/api-reference/data-types.html) | Indicator that the user is an external participant from a collab ||
|#

#### Object attachments {#attachments}

An object of the form `{ "attachment identifier": "n<file identifier>" }`, for example `{"621": "n10921"}`.

#|
|| **Name**
`type` | **Description** ||
|| **key**
[`string`](/api-reference/data-types.html) | Identifier of the file attachment to the item. Pass it in the `attachmentsIds` parameter of the [tasks.template.checklist.removeAttachments](/api-reference/tasks/template/checklist/tasks-template-checklist-remove-attachments.html) method to remove the attachment ||
|| **value**
[`string`](/api-reference/data-types.html) | Identifier of the file on Drive with the `n` prefix. The `filesIds` parameter of the [tasks.template.checklist.addAttachmentsFromDisk](/api-reference/tasks/template/checklist/tasks-template-checklist-add-attachments-from-disk.html) method accepts the same format ||
|#