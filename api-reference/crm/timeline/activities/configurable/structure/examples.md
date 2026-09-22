# Activity Configuration Examples

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Ready-made examples of the [`LayoutDto`](./layout.md) object — the structure that describes the appearance of a timeline entry. This object is passed to the `layout` field of the [crm.activity.configurable.add](../crm-activity-configurable-add.md) and [crm.activity.configurable.update](../crm-activity-configurable-update.md) methods.

Each example shows a complete ready-made configuration and the resulting view the user will see in the timeline:

- [Entry with a Set of Fields](#fields-card) — name-value pairs and a deadline
- [Entry with Different Action Types](#actions-card) — link navigation, opening the app, and an event on a button click
- [Multi-language Entry](#multilang-card) — texts with translations

Examples of individual [content blocks](./content-block.md) are collected on their description page. All the configurations below comply with the [structure restrictions](./layout.md#limits).

Both methods work only within the context of an app: calling them via an inbound webhook returns the `ERROR_WRONG_CONTEXT` error. Permissions and calling conditions are described on the [structure page](./layout.md).

Icon and logo codes in the examples are taken from the general timeline lists. You can retrieve the full lists using the [crm.timeline.icon.list](../../../logmessage/icons/crm-timeline-icon-list.md) and [crm.timeline.logo.list](../../../logmessage/logo/crm-timeline-logo-list.md) methods.

## Entry with a Set of Fields {#fields-card}

An "Information Message" entry with four name-value pairs: deadline, customer, manager, and additional information. Each pair is a `withTitle` block, which outputs a signature and a nested block with a value. A nested block can be of type `text`, `link`, or `deadline`.

The `inline` parameter controls the layout: with `true`, the signature and value are on the same line; with `false`, the value moves below the signature.

The `deadline` block inserts the deadline of the activity itself — the conditions under which it is not displayed and cannot be edited are listed in the [block description](./content-block.md#vybor-krajnego-sroka).

The application generates the keys in the `blocks` array itself — they are not linked to block types. In the example, the key `deadline` matches the type name, but this is a coincidence rather than a requirement.

```json
{
    "icon": {
        "code": "info"
    },
    "header": {
        "title": "Information message"
    },
    "body": {
        "logo": {
            "code": "document"
        },
        "blocks": {
            "deadline": {
                "type": "withTitle",
                "properties": {
                    "title": "Deadline",
                    "inline": true,
                    "block": {
                        "type": "deadline"
                    }
                }
            },
            "client": {
                "type": "withTitle",
                "properties": {
                    "title": "Client",
                    "inline": true,
                    "block": {
                        "type": "text",
                        "properties": {
                            "value": "Müller GmbH"
                        }
                    }
                }
            },
            "manager": {
                "type": "withTitle",
                "properties": {
                    "title": "Manager",
                    "inline": true,
                    "block": {
                        "type": "link",
                        "properties": {
                            "text": "Klaus Weber",
                            "bold": true,
                            "action": {
                                "type": "redirect",
                                "uri": "/company/personal/user/1/"
                            }
                        }
                    }
                }
            },
            "description": {
                "type": "withTitle",
                "properties": {
                    "title": "Additional information in large quantities",
                    "inline": false,
                    "block": {
                        "type": "text",
                        "properties": {
                            "multiline": true,
                            "value": "Arrive no earlier than lunchtime. Entrance from the courtyard, gate password 555. Go up to the 5th floor, ask for Klaus Weber. Payment in cash, change from 5000 EUR."
                        }
                    }
                }
            }
        }
    }
}
```

![Entry with a set of fields](./_images/ContentBlockDto_11.png)

## Entry with Different Action Types {#actions-card}

A configuration that gathers all types of [actions](./action.md): navigating via internal and external links, opening an app from a tag, and sending an event to an app upon a button click.

Both `link` blocks use the `redirect` action but behave differently. A relative link to a deal opens it in a slider. An external link with a domain opens in a new browser tab.

Both tags open an app and differ in styling: `warning` provides a yellow background, while `primary` provides a blue one. Their `actionParams` sets are also different.

Both buttons send the app the `onCrmTimelineItemAction` [event](./action.md#sobytie) with `id = confirm` and differ only by the `animationType` value, so they look the same in the timeline — the difference is visible when clicked.

The `blockId` key in `actionParams` is created by the app and does not reference anything within the configuration itself: the app will decide how to use it.

```json
{
    "icon": {
        "code": "document"
    },
    "header": {
        "title": "Example of different types of actions",
        "tags": {
            "tag1": {
                "type": "warning",
                "title": "open application",
                "action": {
                    "type": "openRestApp",
                    "actionParams": {
                        "myId": 123
                    }
                }
            },
            "tag2": {
                "type": "primary",
                "title": "open application",
                "action": {
                    "type": "openRestApp",
                    "actionParams": {
                        "someImportant": "qwerty"
                    }
                }
            }
        }
    },
    "body": {
        "logo": {
            "code": "document"
        },
        "blocks": {
            "link1": {
                "type": "link",
                "properties": {
                    "text": "Open internal link",
                    "action": {
                        "type": "redirect",
                        "uri": "/crm/deal/details/1/"
                    }
                }
            },
            "link2": {
                "type": "link",
                "properties": {
                    "text": "Open external link",
                    "action": {
                        "type": "redirect",
                        "uri": "https://bitrix24.com"
                    }
                }
            }
        }
    },
    "footer": {
        "buttons": {
            "button1": {
                "title": "rest event",
                "action": {
                    "type": "restEvent",
                    "id": "confirm",
                    "animationType": "loader",
                    "actionParams": {
                        "blockId": "time"
                    }
                },
                "type": "primary"
            },
            "button2": {
                "title": "rest event",
                "action": {
                    "type": "restEvent",
                    "id": "confirm",
                    "animationType": "disable",
                    "actionParams": {
                        "blockId": "time"
                    }
                },
                "type": "primary"
            }
        }
    }
}
```

![Entry with different types of actions](./_images/ContentBlockDto_12.png)

## Multi-language Entry {#multilang-card}

A configuration for an app used in different languages. Instead of a string, an object with translations is passed to the record heading, tag text, block content, and button labels: these fields have the [`textWithTranslation`](./field-types.md#textwithtranslation) type. The same page describes the rules for such an object and the order in which Bitrix24 picks the language.

```json
{
    "icon": {
        "code": "info"
    },
    "header": {
        "title": {"de": "Information", "en": "Information"},
        "tags": {
            "tag": {
                "type": "warning",
                "title": {
                    "de": "Achtung",
                    "en": "Warning"
                }
            }
        }
    },
    "body": {
        "logo": {
            "code": "notification"
        },
        "blocks": {
            "text": {
                "type": "text",
                "properties": {
                    "value": {"de": "Dieser Text wird in verschiedenen Sprachen unterschiedlich angezeigt", "en": "A text"}
                }
            }
        }
    },
    "footer": {
        "buttons": {
            "button1": {
                "title": {"de": "Klick mich", "en": "Push me"},
                "type": "primary",
                "action": {
                    "type": "redirect",
                    "uri": "https://bitrix24.com"
                }
            }
        }
    }
}
```

Result in German:

![Entry in German](./_images/ContentBlockDto_13.png)

Result in English:

![Entry in English](./_images/ContentBlockDto_14.png)

## Continue Learning

- [{#T}](./layout.md)
- [{#T}](./icon.md)
- [{#T}](./header.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./action.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](../crm-activity-configurable-add.md)
- [{#T}](../crm-activity-configurable-update.md)
- [{#T}](../index.md)
