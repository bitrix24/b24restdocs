#### Object checkListItem {#checklistitem}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the checklist item ||
|| **copiedId**
[`integer`](../../../../data-types.md) | Identifier of the original item when copied, if applicable ||
|| **userId**
[`integer`](../../../../data-types.md) | Identifier of the user in the context of which the object was created ||
|| **createdBy**
[`integer`](../../../../data-types.md) | Identifier of the item creator ||
|| **parentId**
[`integer`](../../../../data-types.md) | Identifier of the parent item. A value of `0` indicates a root item ||
|| **title**
[`string`](../../../../data-types.md) | Text of the checklist item ||
|| **sortIndex**
[`integer`](../../../../data-types.md) | Sorting index. The lower the value, the higher the item appears in the list or sublist ||
|| **displaySortIndex**
[`string`](../../../../data-types.md) | Auxiliary value for display order ||
|| **isComplete**
[`boolean`](../../../../data-types.md) | Status of item completion ||
|| **isImportant**
[`boolean`](../../../../data-types.md) | Importance flag for the item ||
|| **completedCount**
[`integer`](../../../../data-types.md) | Number of completions for the item ||
|| **members**
[`array`](../../../../data-types.md) | Array of objects with [description of participants](#members).

If there is no data, an empty array `[]` is returned ||
|| **attachments**
[`object`](../../../../data-types.md) | Object with [description of attachments](#attachments).

If there is no data, an empty array `[]` is returned ||
|| **nodeId**
[`integer`](../../../../data-types.md) | Service identifier of the node, if used ||
|| **templateId**
[`integer`](../../../../data-types.md) | Identifier of the task template ||
|#

#### Object members {#members}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../../data-types.md) | Identifier of the user ||
|| **type**
[`string`](../../../../data-types.md) | User's role in the checklist item. Possible values:
- `A` — Participant
- `U` — Observer ||
|| **name**
[`string`](../../../../data-types.md) | User's name ||
|| **personalPhoto**
[`string`](../../../../data-types.md) | Identifier of the user's avatar file on Drive ||
|| **personalGender**
[`string`](../../../../data-types.md) | User's gender. Possible values:
- `M` — Male
- `F` — Female ||
|| **image**
[`string`](../../../../data-types.md) | Link to the user's avatar ||
|| **isCollaber**
[`boolean`](../../../../data-types.md) | Indicator that the user is an external participant ||
|#

#### Object attachments {#attachments}

#|
|| **Name**
`type` | **Description** ||
|| **<attachmentId>**
[`string`](../../../../data-types.md) | Identifier of the file on Drive in the format `n<fileId>`, where the key is the identifier of the attachment ||
|#