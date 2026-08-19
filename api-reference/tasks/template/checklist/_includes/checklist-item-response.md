#### Object checkListItem {#checklistitem}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](/api-reference/data-types.html) | Identifier of the checklist item ||
|| **copiedId**
[`integer`](/api-reference/data-types.html) | Identifier of the original item when copied, if applicable ||
|| **userId**
[`integer`](/api-reference/data-types.html) | Identifier of the user in the context of which the object is created ||
|| **createdBy**
[`integer`](/api-reference/data-types.html) | Identifier of the item creator ||
|| **parentId**
[`integer`](/api-reference/data-types.html) | Identifier of the parent item. A value of `0` indicates a root item ||
|| **title**
[`string`](/api-reference/data-types.html) | Text of the checklist item ||
|| **sortIndex**
[`integer`](/api-reference/data-types.html) | Sorting index. The lower the value, the higher the item in the list or sublist ||
|| **displaySortIndex**
[`string`](/api-reference/data-types.html) | Auxiliary value for display order ||
|| **isComplete**
[`boolean`](/api-reference/data-types.html) | Status of the item completion ||
|| **isImportant**
[`boolean`](/api-reference/data-types.html) | Importance flag of the item ||
|| **completedCount**
[`integer`](/api-reference/data-types.html) | Number of completions of the item ||
|| **members**
[`array`](/api-reference/data-types.html) | Array of objects with [description of participants](#members).

If there is no data, an empty array `[]` is returned ||
|| **attachments**
[`object`](/api-reference/data-types.html) | Object with [description of attachments](#attachments).

If there is no data, an empty array `[]` is returned ||
|| **nodeId**
[`integer`](/api-reference/data-types.html) | Service identifier of the node, if used ||
|| **templateId**
[`integer`](/api-reference/data-types.html) | Identifier of the task template ||
|#

#### Object members {#members}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](/api-reference/data-types.html) | Identifier of the user ||
|| **type**
[`string`](/api-reference/data-types.html) | Role of the user in the checklist item. Possible values:
- `A` — Participant
- `U` — Observer ||
|| **name**
[`string`](/api-reference/data-types.html) | User's name ||
|| **personalPhoto**
[`string`](/api-reference/data-types.html) | Identifier of the user's avatar file on Drive ||
|| **personalGender**
[`string`](/api-reference/data-types.html) | Gender of the user. Possible values:
- `M` — Male
- `F` — Female ||
|| **image**
[`string`](/api-reference/data-types.html) | Link to the user's avatar ||
|| **isCollaber**
[`boolean`](/api-reference/data-types.html) | Indicator that the user is an external participant ||
|#

#### Object attachments {#attachments}

#|
|| **Name**
`type` | **Description** ||
|| **attachmentId**
[`string`](/api-reference/data-types.html) | Identifier of the file on Drive in the format `n<fileId>`, where the key is the identifier of the attachment ||
|#