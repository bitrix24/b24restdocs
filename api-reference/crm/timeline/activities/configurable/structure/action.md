# Click Reaction

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

`ActionDto` is the reaction to a click on an element of a [timeline record](../index.md). There are three types of actions, and each is described by its own set of fields:

#|
|| **`type` Value** | **What Happens on Click** | **When to Use** ||
|| [`redirect`](#perehod-po-ssylke) | A link opens: a Bitrix24 object slider, a page, or an external site | You need to take the user to a deal, a lead, or an external service ||
|| [`restEvent`](#sobytie) | The application receives the `onCrmTimelineItemAction` event | The click must run logic on the application side ||
|| [`openRestApp`](#otkrytie-slajdera-prilozheniya) | The slider of the application that set the action opens | You need the application's own interface on top of the timeline ||
|#

The action is set in the following fields:

- `titleAction` of the [heading](./header.md) — optional
- `action` of a [tag](./header.md#tagdto) — optional
- `action` of the logo in the [content area](./body.md) — optional
- `action` of a `link` block in [content blocks](./content-block.md) — required
- `action` of a [button in the bottom part](./footer.md) — required
- `action` of a [menu item](./menu-item.md) — required

The `type` field is required. A value outside the list is rejected by the method with the `ENUM_FIELD` error, and a field that belongs to none of the three sets — with the `FIELD_IS_REDUNDANT` error. Bitrix24 does not reject fields of a different action type, but does not use them either.

## Link Navigation redirect {#perehod-po-ssylke}

A relative link to a standard Bitrix24 object that supports a slider opens the slider. Other relative links open as a regular redirect. A link specifying a domain is considered external and opens in a new browser tab.

### Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **type^*^**
[`string`](../../../../../data-types.md) | Value `redirect` ||
|| **uri^*^**
[`string`](../../../../../data-types.md) | Valid link URI, for example `https://example.com` or `/crm/deal/details/1/` ||
|#

### Example

```json
{
    "type": "redirect",
    "uri": "/crm/deal/details/1/"
}
```

## Event restEvent {#sobytie}

To receive the `onCrmTimelineItemAction` event, the application subscribes to it with the [event.bind](../../../../../events/event-bind.md) method or declares a handler during installation.

The event is received only by the application that set the action: for a configurable activity — the application that created the activity, for [additional content blocks](./rest-app-layout-dto.md) — the application that installed the blocks. Other applications do not receive the event.

The context is always passed to the handler:

#|
|| **Field** | **Description** ||
|| **id** | Event identifier — the value of the `id` field from the action ||
|| **entityTypeId** | Identifier of the CRM object type the activity is linked to ||
|| **entityId** | Identifier of the element of this object ||
|| **activityId** | Identifier of the activity ||
|#

The values from `actionParams` are passed through the user's browser, so secrets and tokens must not be put there.

The user who clicked is identified by the standard `auth[user_id]` field of the event — it does not get into the action context.

### Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **type^*^**
[`string`](../../../../../data-types.md) | Value `restEvent` ||
|| **id^*^**
[`string`](../../../../../data-types.md) | Event identifier. Any value can be specified, for example `resetButtonClick` ||
|| **actionParams**
[`object`](../../../../../data-types.md) | Application data that reaches the event handler: the key is the parameter name, the value is a scalar. No more than 20 values, all of them are cast to strings ||
|| **animationType**
[`string`](../../../../../data-types.md) | Animation shown while the event is being processed: `loader` or `disable`. Any other value is rejected by the method with the `ENUM_FIELD` error ||
|#

The event handler often changes the record itself: it adds blocks or replaces the set of buttons. While the event is being processed, `animationType` shows the user that the click has been accepted:

#|
|| **Value** | **What Is Blocked** ||
|| `loader` | The entire timeline record, with a loader on top of it ||
|| `disable` | Only the button that was clicked ||
|#

The block is not released automatically: it persists until the application updates the activity with the [crm.activity.configurable.update](../crm-activity-configurable-update.md) method.

### Example

```json
{
    "type": "restEvent",
    "id": "resetButtonClick",
    "actionParams": {
        "myId": 123,
        "someImportant": "qwerty"
    },
    "animationType": "disable"
}
```

Such an action is assigned to a button or a menu item. Besides the context fields, the handler receives `myId` and `someImportant` set by the application.

## Opening the Application Slider openRestApp {#otkrytie-slajdera-prilozheniya}

{% note warning %}

The action is not supported in the mobile application. If the scenario must work on mobile devices, choose `redirect` or `restEvent`.

{% endnote %}

The slider opens on top of the timeline, and the application interface runs inside it. The context is passed to the slider:

#|
|| **Field** | **Description** ||
|| **entityTypeId** | Identifier of the CRM object type the activity is linked to ||
|| **entityId** | Identifier of the element of this object ||
|| **activityId** | Identifier of the activity ||
|#

### Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **type^*^**
[`string`](../../../../../data-types.md) | Value `openRestApp` ||
|| **actionParams**
[`object`](../../../../../data-types.md) | Application data that reaches the slider together with the context: the key is the parameter name, the value is a scalar. No more than 20 values, all of them are cast to strings ||
|| **sliderParams**
[`ActionSliderParamsDto`](#actionsliderparamsdto) | Options with which the slider is opened ||
|#

### Example

```json
{
    "type": "openRestApp",
    "actionParams": {
        "myId": 123,
        "someImportant": "qwerty"
    },
    "sliderParams": {
        "title": "This is the application slider header",
        "width": 700
    }
}
```

## `ActionSliderParamsDto` Object {#actionsliderparamsdto}

The object sets the slider size, the browser window title, and the label in the header. All of its fields are optional: without `sliderParams` the slider opens with the default settings.

### Parameters of the `ActionSliderParamsDto` Object

#|
|| **Field** | **Description** | **Additional** ||
|| **width**
[`integer`](../../../../../data-types.md) | Slider width, `px` | Set either `width` or `leftBoundary` ||
|| **leftBoundary**
[`integer`](../../../../../data-types.md) | Full-width slider for the browser window with a left margin, `px` | Set either `width` or `leftBoundary` ||
|| **title**
[`string`](../../../../../data-types.md) | Browser window title text when opening the slider | ||
|| **labelText**
[`string`](../../../../../data-types.md) | Label text in the slider header | For example, `Request` ||
|| **labelBgColor**
[`string`](../../../../../data-types.md) | Label background color | Allowed values: `aqua`, `green`, `orange`, `brown`, `pink`, `blue`, `grey`, `violet`. Any other value is rejected by the method with the `ENUM_FIELD` error ||
|| **labelColor**
[`string`](../../../../../data-types.md) | Label text color | A six-digit HEX code with a hash, for example `#ffffff`. Any other format is rejected by the method with the `WRONG_FIELD_VALUE` error ||
|#

## Continue Learning

- [{#T}](./layout.md)
- [{#T}](./icon.md)
- [{#T}](./header.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)
