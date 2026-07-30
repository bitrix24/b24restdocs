# Example Application with Additional Content Blocks

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An additional content block is an interface element that an application adds to an activity or a timeline record. A block can display text, a link, a heading, an expiration date, and other application data. The user will see the block in the CRM card timeline.

The example works as a test page: it allows you to verify the structure of blocks before using them in your own application.

## How the Scenario Works

The scenario involves a user, an application, and Bitrix24:

- the user selects a CRM item, an activity, or a timeline record and assembles a set of blocks
- the application calls methods and passes the selected identifiers, and during installation, also passes a JSON containing the blocks
- Bitrix24 retains this application's set and displays it in the CRM card timeline

A CRM item is a specific lead, deal, contact, company, estimate, invoice, or SPA card. Two values are required for requests:

- `entityTypeId` — a numeric code for the CRM object type, for example `2` for a deal
- `entityId` — the identifier of the specific item, for example a deal identifier

Blocks can be added to two locations:

- **an activity** — an action in the timeline, such as a call or an e-mail. The application retrieves related activities using the [crm.activity.list](../activities/activity-base/crm-activity-list.md) method and uses the `ID` field of the selected activity as the `activityId`
- **a timeline record** — a separate event in the CRM item history. There is no public method to search for suitable records. To test this, create a comment in the same CRM item using the [crm.timeline.comment.add](../comments/crm-timeline-comment-add.md) method. The method will return its identifier in the `result` field. Pass this value as the [timelineId](./crm-timeline-layout-blocks-set.md)

After selecting a location, the application performs three operations:

1. Retrieves the set it installed using the [crm.activity.layout.blocks.get](../activities/layout-blocks/crm-activity-layout-blocks-get.md) or [crm.timeline.layout.blocks.get](./crm-timeline-layout-blocks-get.md) method
2. Sets a new set using the [crm.activity.layout.blocks.set](../activities/layout-blocks/crm-activity-layout-blocks-set.md) or [crm.timeline.layout.blocks.set](./crm-timeline-layout-blocks-set.md) method
3. Deletes the set using the [crm.activity.layout.blocks.delete](../activities/layout-blocks/crm-activity-layout-blocks-delete.md) or [crm.timeline.layout.blocks.delete](./crm-timeline-layout-blocks-delete.md) method

## Preparing the Application and Data

1. Save the [full code](#full-code) to the file `index.html`
2. In the three presets containing links, replace `123` in the path `/crm/deal/details/123/` with an existing deal identifier or specify another relative path within Bitrix24
3. Host the file on a server with a public HTTPS address
4. Create a [server-side local application with a user interface](../../../../local-integrations/serverside-local-app-with-ui.md), specify the file address as the main page, and add the [`crm`](../../../scopes/permissions.md) scope
5. Install and open the application in Bitrix24

To verify, prepare the following:

- A CRM item to which the user has read and write access
- An activity in the selected CRM item to test the first mode
- A deal where a test comment can be created to test the second mode
- Browser access to the jsDelivr CDN and the address `api.bitrix24.com`

../activities/layout-blocks/index.md ([Additional Activity Block Methods]) and ./index.md ([Additional Timeline Block Methods]) work only within the context of an installed application. An incoming webhook is not suitable for this example.

The application supplements the list of CRM types using the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. This method requires administrative access to the CRM. If permissions are insufficient, the application will display an error but will retain the standard types in the list: leads, deals, contacts, companies, estimates, and invoices.

### Retrieving the timelineId for a Deal

This step is only required for the **Timeline Record** mode. Open the installed application and wait for it to load. Then, open the developer tools, select the application iframe context in the **Console** tab, and execute the following code.

In the example, `ENTITY_ID = 4` is the deal identifier that must be replaced. The `deal` string type corresponds to the `entityTypeId = 2` numeric type in the test application.

{% include [Note on examples](../../../../_includes/examples.md) %}

```js
BX24.callMethod(
    'crm.timeline.comment.add',
    {
        fields: {
            ENTITY_TYPE: 'deal',
            ENTITY_ID: 4,
            COMMENT: 'Comment for checking content blocks',
        },
    },
    (result) => {
        if (result.error())
        {
            console.error(result.error());
            return;
        }

        const timelineId = result.data();
        console.log('timelineId:', timelineId);
    }
);
```

The [crm.timeline.comment.add](../comments/crm-timeline-comment-add.md) method returns an integer comment identifier. A string will appear in the console, for example, `timelineId: 123`. Save this value: it will be needed when verifying the timeline recording mode.

Each request execution creates a new comment. After verification, you can delete the unnecessary comment using the [crm.timeline.comment.delete](../comments/crm-timeline-comment-delete.md) method.

Next, we will examine the main parts of the application. The fragments show how the interface, parameters, and methods are linked, but they cannot be run separately. They are already assembled in the correct order in the [full code](#full-code).

The core logic is located in the `ConfigurableTimelineBlocks` class. It stores the editor, fields, and buttons. Records of the `this.#itemIdNode` type access the private fields of the class. The methods from the following steps belong to this class.

## 1. Including Libraries

At the beginning of the file, we will set the UTF-8 encoding and include three external resources:

- Bootstrap styles the fields and buttons
- JSONEditor displays and edits the `layout` object as JSON
- [BX24.js](../../../../sdk/bx24-js-sdk/index.md) provides functions for initializing the page and calling Bitrix24 methods

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Additional content blocks</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/jsoneditor@9.9.2/dist/jsoneditor.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/jsoneditor@9.9.2/dist/jsoneditor.min.js"></script>
    <script src="https://api.bitrix24.com/api/v1/"></script>
</head>
```

Without JSONEditor, the page will not be able to display the block editor. Without BX24.js, [BX24.init](../../../../sdk/bx24-js-sdk/system-functions/bx24-init.md) and [BX24.callMethod](../../../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md) will not be available.

## 2. Adding the Interface

Divide the page into two parts. On the left will be the JSON editor, and on the right will be the request parameters.

```html
<body>
    <div class="container-fluid">
        <form id="form" class="mt-3 mb-3">
            <div class="row">
                <div class="col-8">
                    <div class="mb-3">
                        <div class="d-flex flex-row gap-3">
                            <label class="form-label h3">JSON of blocks</label>
                            <div id="content_block_presets" class="d-flex flex-row gap-2"></div>
                        </div>
                        <div id="json_editor" style="height: 510px"></div>
                    </div>
                </div>
                <div class="col-4" id="parameters">
                    <label class="form-label h3">Parameters</label>
                    <hr class="mt-0">
                    <div class="vstack gap-3">
                        <div class="form-group">
                            <label for="entity_type_id">CRM object</label>
                            <select id="entity_type_id" name="entityTypeId" class="form-select">
                                <option value="2" selected>[2] Deal</option>
                                <option value="1">[1] Lead</option>
                                <option value="3">[3] Contact</option>
                                <option value="4">[4] Company</option>
                                <option value="7">[7] Offer</option>
                                <option value="31">[31] Invoice</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label for="entity_id">CRM object ID</label>
                            <input id="entity_id" name="entityId" type="number" min="1" class="form-control">
                        </div>
                        <div class="form-group">
                            <label for="item_type_id" class="text-truncate">Where to add blocks</label>
                            <select name="itemTypeId" id="item_type_id" class="form-select" required>
                                <option value="1" selected>Activity</option>
                                <option value="2">Timeline entry</option>
                            </select>
                        </div>
                        <button id="get_items_button" type="button" class="btn btn-outline-dark btn-sm">Find</button>
                        <div class="form-group">
                            <label id="item_id_label" for="item_id">Activity</label>
                            <select name="itemId" id="item_id" class="form-select"></select>
                        </div>
                        <button id="get_button" type="button" class="btn btn-outline-dark btn-sm">Get</button>
                        <button id="set_button" type="button" class="btn btn-outline-dark btn-sm">Set</button>
                        <button id="delete_button" type="button" class="btn btn-outline-danger btn-sm">Delete</button>
                    </div>
                </div>
            </div>
        </form>
    </div>
    <div class="container-fluid" id="alert_container"></div>
```

By default, the **Deal** type with `entityTypeId = 2` and the **Activity** mode are selected. The user enters `entityId` — the identifier of a specific deal — and clicks **Find**. To record a timeline entry, the application will replace the activity list with the manual input field `timelineId`.

## 3. Selecting Methods for an Activity or a Timeline Entry

Both modes use the same operations but have different method names and identifier parameters. The `METHODS_MAP` object stores this mapping in one place.

```js
    const ITEM_ACTIVITY = 1;
    const ITEM_TIMELINE = 2;
    const ALLOWED_ITEM_TYPES = [
        ITEM_ACTIVITY,
        ITEM_TIMELINE,
    ];
    const METHODS_MAP = {
        [ITEM_ACTIVITY]: {
            get: 'crm.activity.layout.blocks.get',
            set: 'crm.activity.layout.blocks.set',
            delete: 'crm.activity.layout.blocks.delete',
            itemField: 'activityId',
        },
        [ITEM_TIMELINE]: {
            get: 'crm.timeline.layout.blocks.get',
            set: 'crm.timeline.layout.blocks.set',
            delete: 'crm.timeline.layout.blocks.delete',
            itemField: 'timelineId',
        },
    };
```

For example, for an activity, the application will select the [crm.activity.layout.blocks.set](../activities/layout-blocks/crm-activity-layout-blocks-set.md) method and pass the identifier in the `activityId` parameter. For a timeline entry, it will select [crm.timeline.layout.blocks.set](./crm-timeline-layout-blocks-set.md) and parameter `timelineId`.

When the mode changes, the `renderItemIdControl` method changes the interface item:

```js
        renderItemIdControl()
        {
            const isActivity = this.getItemTypeId() === ITEM_ACTIVITY;
            const itemIdNode = document.createElement(isActivity ? 'select' : 'input');
            itemIdNode.id = 'item_id';
            itemIdNode.name = 'itemId';
            itemIdNode.className = isActivity ? 'form-select' : 'form-control';
            if (!isActivity)
            {
                itemIdNode.type = 'number';
                itemIdNode.min = '1';
            }
            this.#itemIdNode.replaceWith(itemIdNode);
            this.#itemIdNode = itemIdNode;
            this.#getItemsButton.hidden = !isActivity;
            document.getElementById('item_id_label').textContent = isActivity
                ? 'Activity'
                : 'Timeline entry ID';
        }
```

For an activity, a list and a **Find** button are displayed; for a timeline entry, a manual numeric input field is displayed.

## 4. Loading CRM Types and Activities

When the page opens, the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method supplements the CRM object list with smart processes. The [BX24.callMethod](../../../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md) function passes the `result` object to a callback — a function that runs after the response. [`result.error()`](../../../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md#ajax-result) returns an error, while [`result.data()`](../../../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md#ajax-result) returns the successful response data.

```js
        loadDynamicTypes()
        {
            BX24.callMethod('crm.type.list', {}, (result) => {
                if (result.error())
                {
                    this.renderRequestError(result, 'Failed to load smart processes');
                    return;
                }
                const types = result.data()?.types ?? [];
                types.forEach((item) => {
                    const option = document.createElement('option');
                    option.value = item.entityTypeId;
                    option.innerText = `[${item.entityTypeId}] ${item.title}`;
                    this.#entityTypeIdNode.append(option);
                });
            });
        }
```

The value of `item.entityTypeId` becomes the `value` of the new item. The application will pass this exact code in the `entityTypeId` parameter of subsequent requests.

After clicking **Find**, the [crm.activity.list](../activities/activity-base/crm-activity-list.md) method retrieves activities associated with the selected CRM item. Before the request, the `loading()` function disables the fields and buttons, and `stopLoading()` enables them after the response.

```js
        getItemsAction()
        {
            if (this.getItemTypeId() !== ITEM_ACTIVITY)
            {
                return;
            }
            if (!this.validateEntityTypeIdAndEntityId())
            {
                return;
            }
            const data = {
                select: ['*'],
                filter: {
                    'OWNER_TYPE_ID': this.getEntityTypeId(),
                    'OWNER_ID': this.getEntityId(),
                },
            };
            const callback = (result) => {
                this.stopLoading();
                if (result.error())
                {
                    this.renderRequestError(result);
                    return;
                }
                const activities = result.data();
                this.#itemIdNode.innerHTML = '';
                activities.forEach((activity) => {
                    const option = document.createElement('option');
                    option.innerText = `[${activity.ID}] ${activity.SUBJECT} | ${activity.PROVIDER_ID}`;
                    option.value = activity.ID;
                    this.#itemIdNode.append(option);
                });
            };
            this.loading();
            BX24.callMethod('crm.activity.list', data, callback);
        }
```

In the `OWNER_TYPE_ID` filter, the object type code is used, and `OWNER_ID` is the identifier of the specific item. From each found activity, the application saves the `ID` field: then it will become parameter `activityId`.

## 5. Verifying and Collecting Parameters

Before each request, the application verifies the CRM item identifier and the selected activity or entry. The values must be positive integers.

```js
        validateFieldsWithAlerts()
        {
            if (!this.validateEntityTypeIdAndEntityId())
            {
                return false;
            }
            const itemId = this.getItemId();
            if (!Number.isInteger(itemId) || itemId < 1)
            {
                alert(this.getItemTypeId() === ITEM_ACTIVITY
                    ? 'Select activity'
                    : 'Enter timeline entry ID');
                this.#itemIdNode.focus();
                return false;
            }
            return true;
        }
        validateEntityTypeIdAndEntityId()
        {
            const entityId = this.getEntityId();
            if (!Number.isInteger(entityId) || entityId < 1)
            {
                alert('Enter CRM object ID');
                this.#entityIdNode.focus();
                return false;
            }
            if (!ALLOWED_ITEM_TYPES.includes(this.getItemTypeId()))
            {
                alert('Select where to add blocks');
                this.#itemTypeIdNode.focus();
                return false;
            }
            return true;
        }
```

The `getData` method collects the common part of the parameters. The calculated field name in square brackets will be populated by `activityId` or `timelineId` from `METHODS_MAP`.

```js
        getData()
        {
            return {
                entityTypeId: this.getEntityTypeId(),
                entityId: this.getEntityId(),
                [this.getItemFieldName()]: this.getItemId(),
            };
        }
        getMethod(method)
        {
            return METHODS_MAP[this.getItemTypeId()][method];
        }
```

For a deal with `entityId = 4` and an activity with `ID = 8`, an `{ entityTypeId: 2, entityId: 4, activityId: 8 }` object will be produced.

## 6. Retrieving the Set Configuration

The **Get** button calls the `get` method of the selected group.

```js
        getAction()
        {
            const isValid = this.validateFieldsWithAlerts();
            if (!isValid)
            {
                return;
            }
            const method = this.getMethod('get');
            const data = this.getData();
            const callback = (result) => {
                this.stopLoading();
                if (result.error())
                {
                    this.renderRequestError(result);
                    return;
                }
                this.#jsonEditor.set(result.data().layout ?? {});
                this.renderSuccessAlert('Block set received');
            };
            this.loading();
            BX24.callMethod(method, data, callback);
        }
```

In a successful response, [`result.data()`](../../../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md#ajax-result) returns an object with the `layout` field. The application passes its value to the JSON editor, and in its absence `layout` — an empty object.

## 7. Setting and Removing a Set

The `presets` array stores button names and ready-made blocks. The `renderJSONLayoutActions` method creates buttons and adds the selected block to the first available numeric key of the `blocks` object. Existing blocks are not overwritten during this process.

The `layout` object must comply with the [RestAppLayoutDto](../activities/configurable/structure/rest-app-layout-dto.md) structure and contain between 1 and 20 blocks.

For example, a set with a single text block looks like this:

```json
{
  "blocks": {
    "1": {
      "type": "text",
      "properties": {
        "value": "Hello!",
        "multiline": true
      }
    }
  }
}
```

The **Set** button reads the JSON, verifies the `blocks` field and the number of blocks, and then adds `layout` to the `set` method parameters.

```js
        setAction()
        {
            const isValid = this.validateFieldsWithAlerts();
            if (!isValid)
            {
                return;
            }
            let layout;
            try
            {
                layout = this.#jsonEditor.get();
            }
            catch
            {
                this.renderDangerAlert('Fix errors in JSON');
                return;
            }
            if (!layout || typeof layout !== 'object' || Array.isArray(layout))
            {
                this.renderDangerAlert('The root JSON value must be an object');
                return;
            }
            if (!layout.blocks || typeof layout.blocks !== 'object' || Array.isArray(layout.blocks))
            {
                this.renderDangerAlert('The blocks field must be an object');
                return;
            }
            const blocksCount = Object.keys(layout.blocks).length;
            if (blocksCount < 1 || blocksCount > 20)
            {
                this.renderDangerAlert('The number of blocks must be between 1 and 20');
                return;
            }
            const method = this.getMethod('set');
            const data = {
                ...this.getData(),
                layout,
            };
            const callback = (result) => {
                this.stopLoading();
                if (result.error())
                {
                    this.renderRequestError(result);
                    return;
                }
                this.renderSuccessAlert('Block set set');
            };
            this.loading();
            BX24.callMethod(method, data, callback);
        }
```

A subsequent call will replace the previous set for the same application. Sets belonging to other applications will remain unchanged.

The **Delete** button calls the `delete` method using the same identifiers. Upon a successful response, the application clears the editor.

```js
        deleteAction()
        {
            const isValid = this.validateFieldsWithAlerts();
            if (!isValid)
            {
                return;
            }
            const method = this.getMethod('delete');
            const data = this.getData();
            const callback = (result) => {
                this.stopLoading();
                if (result.error())
                {
                    this.renderRequestError(result);
                    return;
                }
                this.renderSuccessAlert('Block set deleted');
                this.#jsonEditor.set({});
            };
            this.loading();
            BX24.callMethod(method, data, callback);
        }
```

## 8. Handling Errors and Launching the Application

Each handler checks [`result.error()`](../../../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md#ajax-result). The `renderRequestError` function attempts to retrieve the error description and displays it below the form.

```js
        renderRequestError(result, prefix = 'Request error')
        {
            const error = result.error();
            const description = error?.ex?.error_description
                ?? error?.ex?.error
                ?? error?.status
                ?? 'Unknown error';
            this.renderDangerAlert(`${prefix}: ${description}`);
        }
```

After the HTML loads, we wait for `BX24.js` to be ready via [BX24.init](../../../../sdk/bx24-js-sdk/system-functions/bx24-init.md). Then, we create a `JSONEditor` and an instance of the application class.

```js
    document.addEventListener('DOMContentLoaded', () => {
        BX24.init(() => {
            const alertContainer = document.getElementById('alert_container');
            const jsonEditor = new JSONEditor(document.getElementById('json_editor'), {
                mode: 'code',
            });
            new ConfigurableTimelineBlocks(
                jsonEditor,
                alertContainer,
                presets,
            );
        });
    });
```

The class constructor binds buttons to handlers, creates buttons for ready-made blocks, and loads SPAs. After this, the application is ready to accept user actions.

## Full Application Code {#full-code}

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Additional content blocks</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/jsoneditor@9.9.2/dist/jsoneditor.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/jsoneditor@9.9.2/dist/jsoneditor.min.js"></script>
    <script src="https://api.bitrix24.com/api/v1/"></script>
</head>
<body>
    <div class="container-fluid">
        <form id="form" class="mt-3 mb-3">
            <div class="row">
                <div class="col-8">
                    <div class="mb-3">
                        <div class="d-flex flex-row gap-3">
                            <label class="form-label h3">JSON of blocks</label>
                            <div id="content_block_presets" class="d-flex flex-row gap-2"></div>
                        </div>
                        <div id="json_editor" style="height: 510px"></div>
                    </div>
                </div>
                <div class="col-4" id="parameters">
                    <label class="form-label h3">Parameters</label>
                    <hr class="mt-0">
                    <div class="vstack gap-3">
                        <div class="form-group">
                            <label for="entity_type_id">CRM object</label>
                            <select id="entity_type_id" name="entityTypeId" class="form-select">
                                <option value="2" selected>[2] Deal</option>
                                <option value="1">[1] Lead</option>
                                <option value="3">[3] Contact</option>
                                <option value="4">[4] Company</option>
                                <option value="7">[7] Offer</option>
                                <option value="31">[31] Invoice</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label for="entity_id">CRM object ID</label>
                            <input id="entity_id" name="entityId" type="number" min="1" class="form-control">
                        </div>
                        <div class="form-group">
                            <label for="item_type_id" class="text-truncate">Where to add blocks</label>
                            <select name="itemTypeId" id="item_type_id" class="form-select" required>
                                <option value="1" selected>Activity</option>
                                <option value="2">Timeline entry</option>
                            </select>
                        </div>
                        <button id="get_items_button" type="button" class="btn btn-outline-dark btn-sm">Find</button>
                        <div class="form-group">
                            <label id="item_id_label" for="item_id">Activity</label>
                            <select name="itemId" id="item_id" class="form-select"></select>
                        </div>
                        <button id="get_button" type="button" class="btn btn-outline-dark btn-sm">Get</button>
                        <button id="set_button" type="button" class="btn btn-outline-dark btn-sm">Set</button>
                        <button id="delete_button" type="button" class="btn btn-outline-danger btn-sm">Delete</button>
                    </div>
                </div>
            </div>
        </form>
    </div>
    <div class="container-fluid" id="alert_container"></div>
<script>
    const ITEM_ACTIVITY = 1;
    const ITEM_TIMELINE = 2;
    const ALLOWED_ITEM_TYPES = [
        ITEM_ACTIVITY,
        ITEM_TIMELINE,
    ];
    const METHODS_MAP = {
        [ITEM_ACTIVITY]: {
            get: 'crm.activity.layout.blocks.get',
            set: 'crm.activity.layout.blocks.set',
            delete: 'crm.activity.layout.blocks.delete',
            itemField: 'activityId',
        },
        [ITEM_TIMELINE]: {
            get: 'crm.timeline.layout.blocks.get',
            set: 'crm.timeline.layout.blocks.set',
            delete: 'crm.timeline.layout.blocks.delete',
            itemField: 'timelineId',
        },
    };
    class ConfigurableTimelineBlocks {
        #jsonEditor;
        #statusContainer;
        #contentBlockPresets;
        // fields
        #entityTypeIdNode;
        #entityIdNode;
        #itemTypeIdNode;
        #itemIdNode;
        // buttons
        #getButton;
        #setButton;
        #deleteButton;
        #getItemsButton;
        constructor(
            jsonEditor,
            statusContainer,
            contentBlockPresets,
        ) {
            this.#jsonEditor = jsonEditor;
            this.#statusContainer = statusContainer;
            this.#contentBlockPresets = contentBlockPresets;
            this.renderJSONLayoutActions();
            this.fetchProperties();
            this.bindEvents();
            this.loadDynamicTypes();
        }
        fetchProperties() {
            this.#entityTypeIdNode = document.getElementById('entity_type_id');
            this.#entityIdNode = document.getElementById('entity_id');
            this.#itemTypeIdNode = document.getElementById('item_type_id');
            this.#itemIdNode = document.getElementById('item_id');
            this.#getButton = document.getElementById('get_button');
            this.#setButton = document.getElementById('set_button');
            this.#deleteButton = document.getElementById('delete_button');
            this.#getItemsButton = document.getElementById('get_items_button');
        }
        bindEvents() {
            this.#getButton.onclick = this.getAction.bind(this);
            this.#setButton.onclick = this.setAction.bind(this);
            this.#deleteButton.onclick = this.deleteAction.bind(this);
            this.#getItemsButton.onclick = this.getItemsAction.bind(this);
            this.#itemTypeIdNode.onchange = this.renderItemIdControl.bind(this);
        }
        renderItemIdControl()
        {
            const isActivity = this.getItemTypeId() === ITEM_ACTIVITY;
            const itemIdNode = document.createElement(isActivity ? 'select' : 'input');
            itemIdNode.id = 'item_id';
            itemIdNode.name = 'itemId';
            itemIdNode.className = isActivity ? 'form-select' : 'form-control';
            if (!isActivity)
            {
                itemIdNode.type = 'number';
                itemIdNode.min = '1';
            }
            this.#itemIdNode.replaceWith(itemIdNode);
            this.#itemIdNode = itemIdNode;
            this.#getItemsButton.hidden = !isActivity;
            document.getElementById('item_id_label').textContent = isActivity
                ? 'Activity'
                : 'Timeline entry ID';
        }
        renderJSONLayoutActions() {
            const contentBlockPresetsContainer = document.getElementById('content_block_presets');
            if (!contentBlockPresetsContainer) {
                return;
            }
            contentBlockPresetsContainer.innerHTML = '';
            this.#contentBlockPresets.forEach((contentBlockPreset) => {
                const button = document.createElement('button');
                button.classList = 'btn btn-link btn-sm text-secondary';
                button.innerText = contentBlockPreset.getTitle();
                button.type = 'button';
                button.onclick = () => {
                    let json;
                    try
                    {
                        json = this.#jsonEditor.get();
                    }
                    catch
                    {
                        this.renderDangerAlert('Fix errors in JSON');
                        return false;
                    }
                    if (!json || typeof json !== 'object' || Array.isArray(json))
                    {
                        this.renderDangerAlert('The root JSON value must be an object');
                        return false;
                    }
                    if (!json.blocks) {
                        json.blocks = {};
                    }
                    if (typeof json.blocks !== 'object' || Array.isArray(json.blocks))
                    {
                        this.renderDangerAlert('The blocks field must be an object');
                        return false;
                    }
                    let blockId = 1;
                    while (Object.hasOwn(json.blocks, `${blockId}`))
                    {
                        blockId += 1;
                    }
                    json.blocks[`${blockId}`] = contentBlockPreset.getValue();
                    this.#jsonEditor.set(json);
                    return false;
                };
                contentBlockPresetsContainer.append(button);
            });
            const clearButton = document.createElement('button');
            clearButton.innerText = 'Clear';
            clearButton.classList = 'btn btn-link btn-sm text-danger';
            clearButton.type = 'button';
            clearButton.onclick = () => {
                this.#jsonEditor.set({});
            };
            contentBlockPresetsContainer.append(clearButton);
        }
        loadDynamicTypes()
        {
            BX24.callMethod('crm.type.list', {}, (result) => {
                if (result.error())
                {
                    this.renderRequestError(result, 'Failed to load smart processes');
                    return;
                }
                const types = result.data()?.types ?? [];
                types.forEach((item) => {
                    const option = document.createElement('option');
                    option.value = item.entityTypeId;
                    option.innerText = `[${item.entityTypeId}] ${item.title}`;
                    this.#entityTypeIdNode.append(option);
                });
            });
        }
        loading()
        {
            this.#entityTypeIdNode.disabled = true;
            this.#entityIdNode.disabled = true;
            this.#itemTypeIdNode.disabled = true;
            this.#itemIdNode.disabled = true;
            this.#getItemsButton.disabled = true;
            this.#getButton.disabled = true;
            this.#setButton.disabled = true;
            this.#deleteButton.disabled = true;
        }
        stopLoading()
        {
            this.#entityTypeIdNode.disabled = false;
            this.#entityIdNode.disabled = false;
            this.#itemTypeIdNode.disabled = false;
            this.#itemIdNode.disabled = false;
            this.#getItemsButton.disabled = false;
            this.#getButton.disabled = false;
            this.#setButton.disabled = false;
            this.#deleteButton.disabled = false;
        }
        getItemsAction()
        {
            if (this.getItemTypeId() !== ITEM_ACTIVITY)
            {
                return;
            }
            if (!this.validateEntityTypeIdAndEntityId())
            {
                return;
            }
            const data = {
                select: ['*'],
                filter: {
                    'OWNER_TYPE_ID': this.getEntityTypeId(),
                    'OWNER_ID': this.getEntityId(),
                },
            };
            const callback = (result) => {
                this.stopLoading();
                if (result.error())
                {
                    this.renderRequestError(result);
                    return;
                }
                const activities = result.data();
                this.#itemIdNode.innerHTML = '';
                activities.forEach((activity) => {
                    const option = document.createElement('option');
                    option.innerText = `[${activity.ID}] ${activity.SUBJECT} | ${activity.PROVIDER_ID}`;
                    option.value = activity.ID;
                    this.#itemIdNode.append(option);
                });
            };
            this.loading();
            BX24.callMethod('crm.activity.list', data, callback);
        }
        getAction()
        {
            const isValid = this.validateFieldsWithAlerts();
            if (!isValid)
            {
                return;
            }
            const method = this.getMethod('get');
            const data = this.getData();
            const callback = (result) => {
                this.stopLoading();
                if (result.error())
                {
                    this.renderRequestError(result);
                    return;
                }
                this.#jsonEditor.set(result.data().layout ?? {});
                this.renderSuccessAlert('Block set received');
            };
            this.loading();
            BX24.callMethod(method, data, callback);
        }
        setAction()
        {
            const isValid = this.validateFieldsWithAlerts();
            if (!isValid)
            {
                return;
            }
            let layout;
            try
            {
                layout = this.#jsonEditor.get();
            }
            catch
            {
                this.renderDangerAlert('Fix errors in JSON');
                return;
            }
            if (!layout || typeof layout !== 'object' || Array.isArray(layout))
            {
                this.renderDangerAlert('The root JSON value must be an object');
                return;
            }
            if (!layout.blocks || typeof layout.blocks !== 'object' || Array.isArray(layout.blocks))
            {
                this.renderDangerAlert('The blocks field must be an object');
                return;
            }
            const blocksCount = Object.keys(layout.blocks).length;
            if (blocksCount < 1 || blocksCount > 20)
            {
                this.renderDangerAlert('The number of blocks must be between 1 and 20');
                return;
            }
            const method = this.getMethod('set');
            const data = {
                ...this.getData(),
                layout,
            };
            const callback = (result) => {
                this.stopLoading();
                if (result.error())
                {
                    this.renderRequestError(result);
                    return;
                }
                this.renderSuccessAlert('Block set set');
            };
            this.loading();
            BX24.callMethod(method, data, callback);
        }
        deleteAction()
        {
            const isValid = this.validateFieldsWithAlerts();
            if (!isValid)
            {
                return;
            }
            const method = this.getMethod('delete');
            const data = this.getData();
            const callback = (result) => {
                this.stopLoading();
                if (result.error())
                {
                    this.renderRequestError(result);
                    return;
                }
                this.renderSuccessAlert('Block set deleted');
                this.#jsonEditor.set({});
            };
            this.loading();
            BX24.callMethod(method, data, callback);
        }
        validateFieldsWithAlerts()
        {
            if (!this.validateEntityTypeIdAndEntityId())
            {
                return false;
            }
            const itemId = this.getItemId();
            if (!Number.isInteger(itemId) || itemId < 1)
            {
                alert(this.getItemTypeId() === ITEM_ACTIVITY
                    ? 'Select activity'
                    : 'Enter timeline entry ID');
                this.#itemIdNode.focus();
                return false;
            }
            return true;
        }
        validateEntityTypeIdAndEntityId()
        {
            const entityId = this.getEntityId();
            if (!Number.isInteger(entityId) || entityId < 1)
            {
                alert('Enter CRM object ID');
                this.#entityIdNode.focus();
                return false;
            }
            if (!ALLOWED_ITEM_TYPES.includes(this.getItemTypeId()))
            {
                alert('Select where to add blocks');
                this.#itemTypeIdNode.focus();
                return false;
            }
            return true;
        }
        getData()
        {
            return {
                entityTypeId: this.getEntityTypeId(),
                entityId: this.getEntityId(),
                [this.getItemFieldName()]: this.getItemId(),
            };
        }
        getMethod(method)
        {
            return METHODS_MAP[this.getItemTypeId()][method];
        }
        getEntityTypeId()
        {
            return Number.parseInt(this.#entityTypeIdNode.value, 10);
        }
        getEntityId()
        {
            return Number.parseInt(this.#entityIdNode.value, 10);
        }
        getItemTypeId()
        {
            return Number.parseInt(this.#itemTypeIdNode.value, 10);
        }
        getItemId()
        {
            return Number.parseInt(this.#itemIdNode.value, 10);
        }
        getItemFieldName()
        {
            return METHODS_MAP[this.getItemTypeId()].itemField;
        }
        renderAlert(message, classList)
        {
            const alert = document.createElement('div');
            alert.className = classList;
            alert.setAttribute('role', 'alert');
            const time = (new Date()).toLocaleTimeString();
            alert.innerText = `[${time}] ${message}`;
            this.#statusContainer.innerHTML = '';
            this.#statusContainer.append(alert);
        }
        renderDangerAlert(message)
        {
            this.renderAlert(message, 'alert alert-danger');
        }
        renderRequestError(result, prefix = 'Request error')
        {
            const error = result.error();
            const description = error?.ex?.error_description
                ?? error?.ex?.error
                ?? error?.status
                ?? 'Unknown error';
            this.renderDangerAlert(`${prefix}: ${description}`);
        }
        renderSuccessAlert(message)
        {
            this.renderAlert(message, 'alert alert-success');
        }
    }
    class ContentBlockPreset
    {
        #title;
        #value;
        constructor(title, value) {
            this.#title = title;
            this.#value = value;
        }
        getTitle(){ return this.#title; }
        getValue(){ return this.#value; }
    }
    const presets = [
        new ContentBlockPreset('Text', {
            type: "text",
            properties: {
                value: "Hello!\nWe are starting.",
                multiline: true,
                bold: true,
                color: "base_90"
            }
        }),
        new ContentBlockPreset('Large text', {
            type: "largeText",
            properties: {
                value: "Hello!\nWe are starting.\nWe are continuing.\nWe are still working on it.\nWe are continuing.\nWe are close to the result.\nGoodbye."
            }
        }),
        new ContentBlockPreset('Link', {
            type: "link",
            properties: {
                text: "Open deal",
                action: {
                    type: "redirect",
                    uri: "/crm/deal/details/123/"
                },
                bold: true
            }
        }),
        new ContentBlockPreset('Block with heading', {
            type: "withTitle",
            properties: {
                title: "Heading",
                block: {
                    type: "text",
                    properties: {
                        value: "Some value"
                    }
                }
            }
        }),
        new ContentBlockPreset('Heading and link in a line', {
            type: "withTitle",
            properties: {
                title: "Heading 2",
                block: {
                    type: "link",
                    properties: {
                        text: "Open deal",
                        action: {
                            type: "redirect",
                            uri: "/crm/deal/details/123/"
                        }
                    }
                },
                inline: true
            }
        }),
        new ContentBlockPreset('Block row', {
            type: "lineOfBlocks",
            properties: {
                blocks: {
                    text: {
                        type: "text",
                        properties: {
                            value: "Some text"
                        }
                    },
                    link: {
                        type: "link",
                        properties: {
                            text: "link",
                            action: {
                                type: "redirect",
                                uri: "/crm/deal/details/123/"
                            }
                        }
                    },
                    boldText: {
                        type: "text",
                        properties: {
                            value: "bold text",
                            bold: true
                        }
                    }
                }
            }
        }),
        new ContentBlockPreset('Deadline', {
            type: "deadline",
            properties: {
                readonly: false
            }
        }),
    ];
    document.addEventListener('DOMContentLoaded', () => {
        BX24.init(() => {
            const alertContainer = document.getElementById('alert_container');
            const jsonEditor = new JSONEditor(document.getElementById('json_editor'), {
                mode: 'code',
            });
            new ConfigurableTimelineBlocks(
                jsonEditor,
                alertContainer,
                presets,
            );
        });
    });
</script>
</body>
</html>
```

## Key Considerations

When working, keep the following limitations in mind:

- To record a timeline entry, select a configurable record — a record that supports additional application blocks. It is not possible to set blocks in a deal, a [timeline log entry](../logmessage/index.md) for a secondary event, or a legacy record using [crm.timeline.layout.blocks.* methods](./index.md).
- For an activity, the [crm.activity.layout.blocks.set](../activities/layout-blocks/crm-activity-layout-blocks-set.md) method cannot be applied to a [configurable application activity](../activities/configurable/index.md) or a legacy type activity.
- The **Delete** button removes the set immediately without additional confirmation.

## Verify the Result {#check-result}

1. Select the type and specify the CRM object identifier.
2. Choose where to add the blocks:
   - **Activity** — click **Find** and select an activity from the list.
   - **Timeline Record** — enter the saved `timelineId` of the created comment.
3. Add one or more ready-made blocks to the editor.
4. Click **Set**. The application will display the Block set set message.
5. Open the CRM item card. The added blocks should appear on the selected activity or timeline record.
6. Return to the application and click **Get**. The installed JSON should appear in the editor.
7. Click **Delete**, then refresh the CRM card. The set should disappear from the timeline.

## If the Application Returns an Error

- If SPAs do not appear in the **CRM Object** field, check the administrative access to the CRM. Standard types can be used without loading SPAs
- If the list is empty after clicking **Find**, check the CRM item identifier and the presence of related activities
- If the request returns an access error, ensure that the page is opened within an installed application with the [`crm`](../../../scopes/permissions.md) scope and that the user has permissions for the selected CRM item
- If the timeline record methods return an error, check `entityTypeId`, `entityId`, `timelineId`, and the record type. Blocks can only be set in a suitable configurable record
- If the editor reports a JSON error, fix the structure and ensure that the `blocks` field contains between 1 and 20 blocks

## Continue Learning

- [Additional Timeline Content Blocks](./index.md)
- [Additional Activity Content Blocks](../activities/layout-blocks/index.md)
- [RestAppLayoutDto Additional Content Structure](../activities/configurable/structure/rest-app-layout-dto.md)
