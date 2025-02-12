# Click Reaction

`ActionDto` — the action defines the reaction to a click on a specific timeline record element. There are several different types of actions, each with its own format.

## Link Navigation

If a relative link to standard Bitrix24 objects that support opening in a slider is used, the slider will be displayed. Otherwise, it will be a regular link navigation.

### Parameters

{% include [Note on parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **type^*^**
[`const`](../../../../data-types.md) | Value `redirect` ||
|| **uri**
[`string`](../../../../data-types.md) | Valid URI link, for example `https://ya.com` or `/crm/deal/details/1/` ||
|#

### Example

```json
{
    "type": "redirect",
    "uri": "/crm/deal/details/1/"
}
```

## Event

Calling the action will generate the event **onCrmTimelineItemAction**. When the event occurs, handlers will be triggered only for the application that created this timeline record. The context will always be passed to the handler:

- **id** - event identifier,
- **entityTypeId** - identifier of the object type to which the deal is linked,
- **entityId** - identifier of the element of this object,
- **activityId** - identifier of the deal,
- **userId** - identifier of the user who triggered the action.

### Parameters

{% include [Note on parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **type^*^**
[`const`](../../../../data-types.md) | Value `restEvent` ||
|| **id^*^**
[`string`](../../../../data-types.md) | Event identifier. Any value can be specified, for example `resetButtonClick` ||
|| **actionParams**
[`array`](../../../../data-types.md) | Array of arbitrary format, data from which will be passed to the event handler ||
|| **animationType**
[`string`](../../../../data-types.md) | Animation that will be shown during event processing, for example `disable` ||
|#

In some cases, sending the event implies that the handler of this event should change the appearance of the record in the timeline. For example, adding new blocks or changing the set of buttons.

To ensure the user sees the update, the `animationType` parameter can be used. If `animationType` is set to **loader** — the timeline record will be blocked and a loader will appear on top of it. The blocking will last until the record is updated via [crm.activity.configurable.update](../crm-activity-configurable-update.md). 
If the action is triggered by clicking a button at the bottom of the timeline and `animationType` is set to `disable` — this button will be blocked until the record is updated via [crm.activity.configurable.update](../crm-activity-configurable-update.md).

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

The action is assigned to a button. Clicking it will trigger the event handler `onCrmTimelineItemAction`, registered by the application that created the timeline record. 
The handler will receive standard parameters `id=resetButtonClick`, `entityTypeId`, `entityId`, `activityId`, `userId`, and parameters defined by the developer — `myId=123` and `someImportant=qwerty`. The button will be blocked from the moment it is clicked until the timeline record is updated via `crm.activity.configurable.update`.

## Opening the Application Slider

{% note warning %}

The action is not supported in the mobile application.

{% endnote %}

Calling the action will open the slider of the application that created the timeline record. The context will be passed to the slider:

- **entityTypeId** - identifier of the object type to which the deal is linked,
- **entityId** - identifier of the element of this object,
- **activityId** - identifier of the deal.

### Parameters

{% include [Note on parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **type^*^**
[`const`](../../../../data-types.md) | Value `openRestApp` ||
|| **actionParams**
[`array`](../../../../data-types.md) | Array of arbitrary format, data from which will be passed to the event handler ||
|| **sliderParams**
[ActionSliderParamsDto](#actionsliderparamsdto) | Options with which the slider is opened ||
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
        "labelText": "this application",
        "labelColor": "#c3b3ff",
        "labelBgColor": "violet",
        "title": "This is the application slider title",
        "width": 700
    }
}
```

### ActionSliderParamsDto

#### Parameters for Opening the Application Slider

#|
|| **Field** | **Description** | **Additional** ||
|| **width**
[`int`](../../../../data-types.md) | Width of the slider, `px` | Cannot be used simultaneously with `leftBoundary` ||
|| **leftBoundary**
[`int`](../../../../data-types.md) | Slider spans the full width of the browser window with a left margin, `px` | Cannot be used simultaneously with `width` ||
|| **labelBgColor**
[`string`](../../../../data-types.md) | Background color code of the slider close button | Can only take values `aqua`, `green`, `orange`, `brown`, `pink`, `blue`, `grey`, `violet` ||
|| **labelColor**
[`string`](../../../../data-types.md) | Color code of the slider close button | In `hex` format ||
|| **labelText**
[`string`](../../../../data-types.md) | Text of the slider close button | ||
|| **title**
[`string`](../../../../data-types.md) | Text of the browser window title when opening the slider | ||
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