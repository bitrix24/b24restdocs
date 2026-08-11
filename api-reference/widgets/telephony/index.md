# Widgets in Telephony: Overview of Placements

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Placements of the section add the application interface to telephony: a tab in the call card during a conversation and a report in the call analytics menu. Both placements need the `telephony` scope in addition to `placement`.

To register a widget, use the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter.

An external WebRTC client is embedded differently. It has no placement code of its own: the client is loaded into the [PAGE_BACKGROUND_WORKER](../universal/background-worker.md) placement, and telephony is connected with the methods of the [{#T}](../../telephony/index.md) section. The scenario is described on the [{#T}](./webrtc.md) page.

> Quick navigation: [all placements](#all-placements)

## How to Choose a Placement

Choose a placement by the task your application solves:

- show client data right during a conversation — [CALL_CARD](./call-card.md)
- add your own report to the built-in call analytics — [TELEPHONY_ANALYTICS_MENU](./analytics-menu.md)

The placements differ in the condition that invokes them. The `CALL_CARD` handler is invoked when the user opens the tab in the call card, and it receives the conversation context. This placement exists only during a call: without an active call there is no card and nothing to check the widget against.

The `TELEPHONY_ANALYTICS_MENU` handler is invoked when the user selects an item in the call statistics menu. The placement has no context of its own, but it is available at any moment.

## How to Get Started

1. Choose a placement for your scenario.
2. Register the handler with the [placement.bind](../placement-bind.md) method and pass the code in the `PLACEMENT` parameter. The method is available to an administrator only and requires the application context: a placement cannot be bound with a webhook. On successful registration, the method returns `result: true` — the response breakdown and the error codes are on its page.
3. Complete the application installation. Until then, the widget is not displayed in the interface.
4. Reload the Bitrix24 page. The lists of call card tabs and analytics menu items are built when the page loads.
5. Call the widget. For `TELEPHONY_ANALYTICS_MENU`, open the `/report/telephony/` page and select the application item. For `CALL_CARD`, you need a call: register an external call with the [telephony.externalCall.register](../../telephony/telephony-external-call-register.md) method and the `SHOW` = 1 parameter.
6. Parse `PLACEMENT_OPTIONS` in the handler — it carries the call context.

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

Both placements pass the same set of standard parameters to the handler. The example is shown for a tab in the call card: for the analytics menu, the `PLACEMENT` value and the call context in `PLACEMENT_OPTIONS` change.

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 588b8a98e848778a4ffb38fbcf70f2b9
    [AUTH_ID] => 4172bb6600705a0700005a4b00000001f0f107c42ca5bd5f61030c5d9c3e4d60d11b5a
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 31f1e26600705a0700005a4b00000001f0f107b1918506d8a2ed9ecf76e8fdac962471
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => 5b2f8c1d7e3a9046b8c5d2f1a7e3b904
    [APPLICATION_SCOPE] => telephony,crm,placement
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => CALL_CARD
    [PLACEMENT_OPTIONS] => {"CALL_ID":"externalCall.c3ee67f1a63f6e6117c230ab59cc49ea.1723556778","PHONE_NUMBER":"+4930123456","LINE_NUMBER":"+49 30 000-00-00","LINE_NAME":"+49 30 000-00-00","CRM_ENTITY_TYPE":"COMPANY","CRM_ENTITY_ID":"17","CRM_ACTIVITY_ID":"undefined","CRM_BINDINGS":[{"ENTITY_TYPE":"DEAL","ENTITY_ID":"25"},{"ENTITY_TYPE":"COMPANY","ENTITY_ID":"17"}],"CALL_DIRECTION":"incoming","CALL_STATE":"connected","CALL_LIST_MODE":"false","URI":"\/crm\/company\/details\/17\/"}
)
```

After parsing, the `PLACEMENT_OPTIONS` string from this example looks like this:

```json
{
    "CALL_ID": "externalCall.c3ee67f1a63f6e6117c230ab59cc49ea.1723556778",
    "PHONE_NUMBER": "+4930123456",
    "LINE_NUMBER": "+49 30 000-00-00",
    "LINE_NAME": "+49 30 000-00-00",
    "CRM_ENTITY_TYPE": "COMPANY",
    "CRM_ENTITY_ID": "17",
    "CRM_ACTIVITY_ID": "undefined",
    "CRM_BINDINGS": [
        { "ENTITY_TYPE": "DEAL", "ENTITY_ID": "25" },
        { "ENTITY_TYPE": "COMPANY", "ENTITY_ID": "17" }
    ],
    "CALL_DIRECTION": "incoming",
    "CALL_STATE": "connected",
    "CALL_LIST_MODE": "false",
    "URI": "/crm/company/details/17/"
}
```

The values arrive as strings, including `CALL_LIST_MODE` and `CRM_ACTIVITY_ID`: compare them with the strings `"true"`, `"false"`, and `"undefined"`, not with a boolean type.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context. The universal `URI` key arrives for both placements on the general terms described above, while own keys exist only for the call card.

#|
|| **Placement** | **Own Keys** | **What Is Passed** ||
|| [CALL_CARD](./call-card.md) | Conversation: `CALL_ID`, `CALL_DIRECTION`, `CALL_STATE`, `CALL_LIST_MODE`

Line and number: `PHONE_NUMBER`, `LINE_NUMBER`, `LINE_NAME`

CRM links: `CRM_ENTITY_TYPE`, `CRM_ENTITY_ID`, `CRM_BINDINGS`, `CRM_ACTIVITY_ID` | The context of the call during which the widget is opened. The description of each key, the default values, and the cases when a key does not arrive are on the [placement page](./call-card.md) ||
|| [TELEPHONY_ANALYTICS_MENU](./analytics-menu.md) | none | Only the address of the analytics page in `URI` ||
|#

## OPTIONS When Registering via placement.bind

Neither placement of the section supports the `OPTIONS` parameter of the [placement.bind](../placement-bind.md) method: the values passed are not retained, and [placement.get](../placement-get.md) returns an empty array for such a registration.

This rule does not apply to the WebRTC scenario. It works not on a telephony placement but on [PAGE_BACKGROUND_WORKER](../universal/background-worker.md), and there `OPTIONS[errorHandlerUrl]` is required: without it, `placement.bind` returns the `EMPTY_ERROR_HANDLER_URL` error.

## Relationship With Other Objects

**Call.** The `CALL_ID` identifier from the `CALL_CARD` context is the same one returned by [telephony.externalCall.register](../../telephony/telephony-external-call-register.md). The handler uses it to finish the call with the [telephony.externalCall.finish](../../telephony/telephony-external-call-finish.md) method and to attach a call recording.

**CRM items.** The `CRM_ENTITY_TYPE` and `CRM_ENTITY_ID` keys point to the item the call is linked to, and `CRM_BINDINGS` points to all linked items. The data is returned by the methods of the corresponding CRM sections.

**CRM activity.** The `CRM_ACTIVITY_ID` identifier points to the activity created for the call. The activity data is returned by the [crm.activity.get](../../crm/timeline/activities/activity-base/crm-activity-get.md) method.

**Phone lines.** The line name in `LINE_NAME` refers to a line added with the [telephony.externalLine.add](../../telephony/telephony-external-line-add.md) method.

## Common Mistakes

#|
|| **Mistake** | **How to Resolve** ||
|| `placement.bind` returns `WRONG_AUTH_TYPE` with the description `Application context required` | Register the placement on behalf of the application. A placement cannot be bound with a webhook ||
|| `placement.bind` returns `ERROR_PLACEMENT_NOT_FOUND` | The placement code is specified incorrectly or the application has not been granted the `telephony` scope ||
|| `placement.bind` returns `ERROR_ARGUMENT` | Pass the required `PLACEMENT` and `HANDLER` parameters. The code of the empty field arrives in `argument` ||
|| The widget is registered but does not appear in the interface | Complete the [application installation](../../../settings/app-installation/installation-finish.md) and reload the Bitrix24 page: the lists of call card tabs and analytics menu items are assembled when the page loads ||
|| The handler received call data other than expected | The description of each context key and its specifics are on the [CALL_CARD](./call-card.md) page: it also describes `CRM_ACTIVITY_ID` with the string `undefined`, the symbolic code in `CRM_ENTITY_TYPE`, and the full list of links in `CRM_BINDINGS` ||
|#

The error arrives in the response body — the code in the `error` field, the text in `error_description`:

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

Other registration error codes are listed in the "Possible Error Codes" section of the [placement.bind](../placement-bind.md) page.

## Overview of Placements {#all-placements}

> Scope: [`placement, telephony`](../../scopes/permissions.md)

#|
|| **Placement** | **When to Use** ||
|| [CALL_CARD](./call-card.md) | Show client data from an external system right during a conversation: the tab opens in the call card and receives the conversation context ||
|| [TELEPHONY_ANALYTICS_MENU](./analytics-menu.md) | Add your own call report to the built-in analytics: the item opens in the call statistics menu and is available at any moment ||
|#

The third material of the section — [{#T}](./webrtc.md) — is not listed in the table: it has no placement code of its own, and the client runs on top of the [PAGE_BACKGROUND_WORKER](../universal/background-worker.md) placement.

## Continue Your Exploration

- [{#T}](../index.md)
- [{#T}](../placements.md)
- [{#T}](./webrtc.md)
- [{#T}](../placement-bind.md)
- [{#T}](../placement-get.md)
- [{#T}](../placement-list.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../telephony/index.md)
- [{#T}](../../../settings/interactivity/index.md)
