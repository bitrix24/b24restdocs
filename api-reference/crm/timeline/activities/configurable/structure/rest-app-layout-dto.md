# Set of Additional Content Blocks

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

`RestAppLayoutDto` is a set of additional [content blocks](./content-block.md) that an application adds to someone else's timeline entry.

A typical scenario: a delivery app shows the waybill number and a tracking link right inside the entry about a call to the customer, and a telephony app adds a link to the call recording to someone else's activity. An entry of your own is not needed for this — a few blocks in an existing one are enough. If, however, the application creates the entire entry and controls its icon, heading, and buttons, you need [`LayoutDto`](./layout.md) and a configurable activity.

The object is passed in the `layout` parameter. Where exactly the blocks are added is set by the remaining parameters of the method:

#|
|| **Method** | **What It Does** | **What Sets the Link** ||
|| [crm.activity.layout.blocks.set](../../layout-blocks/crm-activity-layout-blocks-set.md) | Adds blocks to a CRM activity | `entityTypeId`, `entityId`, `activityId` ||
|| [crm.timeline.layout.blocks.set](../../../layout-blocks/crm-timeline-layout-blocks-set.md) | Adds blocks to a timeline entry | `entityTypeId`, `entityId`, `timelineId` ||
|| [crm.activity.layout.blocks.get](../../layout-blocks/crm-activity-layout-blocks-get.md) | Returns the set installed in an activity | `entityTypeId`, `entityId`, `activityId` ||
|| [crm.timeline.layout.blocks.get](../../../layout-blocks/crm-timeline-layout-blocks-get.md) | Returns the set installed in an entry | `entityTypeId`, `entityId`, `timelineId` ||
|| [crm.activity.layout.blocks.delete](../../layout-blocks/crm-activity-layout-blocks-delete.md) | Deletes the set from an activity | `entityTypeId`, `entityId`, `activityId` ||
|| [crm.timeline.layout.blocks.delete](../../../layout-blocks/crm-timeline-layout-blocks-delete.md) | Deletes the set from an entry | `entityTypeId`, `entityId`, `timelineId` ||
|#

The blocks are displayed below the main content of the entry. If several applications have installed their sets, they are shown in the order they were added.

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: for `set` and `delete` — a user with edit access to the CRM object the activity or timeline entry is linked to, for `get` read access is enough

{% note info "" %}

The methods that accept `RestAppLayoutDto` work only within the context of an [application](../../../../../../settings/app-installation/index.md). Calling them via an inbound webhook returns the `ERROR_WRONG_CONTEXT` error.

{% endnote %}

## Parameters of the `RestAppLayoutDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **blocks^*^**
[`object`](../../../../../data-types.md) | Additional content blocks: the key is the block identifier that the application sets itself, the value is a [ContentBlockDto](./content-block.md) object ||
|#

## Restrictions {#limits}

- No more than 20 blocks, otherwise the method returns the `TOO_MANY_ITEMS` error.
- Block keys consist only of Latin letters, digits, hyphens, and underscores, otherwise the method returns the `KEY_CONTAIN_WRONG_SYMBOLS` error.
- The `blocks` field is required: without it the method returns the `FIELD_IS_REQUIRED` error. An empty object passes validation, but the set of blocks ends up empty.
- A new set replaces the previous one as a whole within a single application, the blocks are not merged.
- The set cannot be installed in a [configurable activity](../index.md) whose appearance is entirely defined by `LayoutDto`, or in an activity of a deprecated type. For such an activity [crm.activity.layout.blocks.set](../../layout-blocks/crm-activity-layout-blocks-set.md) returns the `UNSUITABLE_ACTIVITY_TYPE_ERROR` error, and for an unsuitable timeline entry [crm.timeline.layout.blocks.set](../../../layout-blocks/crm-timeline-layout-blocks-set.md) returns `UNAVAILABLE_TIMELINE_ITEM`.
- There is no way to tell in advance whether an object is suitable — only a trial call shows it.

## Example Object

A set of seven blocks: text, long text, a link, two name-value pairs, a line of several blocks, and a deadline.

```json
{
    "blocks": {
        "block_1": {
            "type": "text",
            "properties": {
                "value": "Hello!\nWe are starting.",
                "multiline": true,
                "bold": true,
                "color": "base_90"
            }
        },
        "block_2": {
            "type": "largeText",
            "properties": {
                "value": "Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye."
            }
        },
        "block_3": {
            "type": "link",
            "properties": {
                "text": "Open deal",
                "action": {
                    "type": "redirect",
                    "uri": "/crm/deal/details/123/"
                },
                "bold": true
            }
        },
        "block_4": {
            "type": "withTitle",
            "properties": {
                "title": "Title",
                "block": {
                    "type": "text",
                    "properties": {
                        "value": "Some value"
                    }
                }
            }
        },
        "block_5": {
            "type": "withTitle",
            "properties": {
                "title": "Title 2",
                "block": {
                    "type": "link",
                    "properties": {
                        "text": "Open deal",
                        "action": {
                            "type": "redirect",
                            "uri": "/crm/deal/details/123/"
                        }
                    }
                },
                "inline": true
            }
        },
        "block_6": {
            "type": "lineOfBlocks",
            "properties": {
                "blocks": {
                    "text": {
                        "type": "text",
                        "properties": {
                            "value": "Some text"
                        }
                    },
                    "link": {
                        "type": "link",
                        "properties": {
                            "text": "link",
                            "action": {
                                "type": "redirect",
                                "uri": "/crm/deal/details/123/"
                            }
                        }
                    },
                    "boldText": {
                        "type": "text",
                        "properties": {
                            "value": "bold text",
                            "bold": true
                        }
                    }
                }
            }
        },
        "block_7": {
            "type": "withTitle",
            "properties": {
                "title": "Title with deadline",
                "block": {
                    "type": "deadline",
                    "properties": {
                        "readonly": false
                    }
                }
            }
        }
    }
}
```


## Continue Exploring

- [{#T}](../../layout-blocks/index.md)
- [{#T}](../../../layout-blocks/index.md)
- [{#T}](../../../layout-blocks/content-blocks-test-app.md)
- [{#T}](./layout.md)
- [{#T}](./icon.md)
- [{#T}](./header.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./action.md)
- [{#T}](./field-types.md)
- [{#T}](./examples.md)
