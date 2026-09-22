# Structure of Configurable Activity

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

`LayoutDto` is the top-level object that describes the appearance of a [timeline entry](../index.md). The application builds the whole entry from it: the icon, the heading with tags, the content area, and the bottom part with buttons and a menu.

The object is passed in the `layout` parameter of the [crm.activity.configurable.add](../crm-activity-configurable-add.md) and [crm.activity.configurable.update](../crm-activity-configurable-update.md) methods. The [crm.activity.configurable.get](../crm-activity-configurable-get.md) method returns the same structure in the `layout` field of the response.

On update, the structure is replaced as a whole, the fields are not merged. Pass `layout` in full even if only one block has changed.

The structure is hierarchical: each `LayoutDto` field is a standalone object with its own set of fields, described on a separate page. Such objects are called DTO, Data Transfer Object.

`LayoutDto` describes the entire entry and works only for activities created by the application itself. To add your own blocks to someone else's timeline entry, use a different object — [`RestAppLayoutDto`](./rest-app-layout-dto.md).

![Top-level object of the timeline entry](./_images/LayoutDto.png)

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: a user with edit access to the CRM object the activity is linked to. Without such access the method returns the `ACCESS_DENIED` error

{% note info "" %}

The methods that accept `LayoutDto` work only within the context of an [application](../../../../../../settings/app-installation/index.md). Calling them via an inbound webhook returns the `ERROR_WRONG_CONTEXT` error.

{% endnote %}

## Parameters of the `LayoutDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **icon^*^**
[`IconDto`](./icon.md) | Icon to the left of the entry ||
|| **header^*^**
[`HeaderDto`](./header.md) | Title of the record ||
|| **body^*^**
[`BodyDto`](./body.md) | Main content area of the entry ||
|| **footer**
[`FooterDto`](./footer.md) | Bottom part of the entry with action block ||
|#

## How to Build the Structure

1. Fill in the required `icon`, `header`, and `body` fields, and if the entry has actions — the optional `footer` with buttons and [menu items](./menu-item.md).
2. Assemble the entry content from [content blocks](./content-block.md) — they live in `body.blocks`.
3. Describe the reaction to clicks — [`ActionDto`](./action.md). This object is accepted by the heading, tags, logo, links, buttons, and menu items.
4. Pass the finished structure in the `layout` parameter of the [crm.activity.configurable.add](../crm-activity-configurable-add.md) method.

Texts the user sees accept the [`textWithTranslation`](./field-types.md#textwithtranslation) type — they can be passed in several languages at once.

The [`scope`](./field-types.md#scope) field of blocks, buttons, and menu items hides the element in the browser or in the mobile app. It is not related to the application scope.

## Structure Restrictions {#limits}

#|
|| **Restriction** | **Error Code** ||
|| No more than two [tags](./header.md#obuekt) in the heading | `TOO_MANY_ITEMS` ||
|| No more than two [buttons](./footer.md) in the bottom part | `TOO_MANY_ITEMS` ||
|| No more than 20 [content blocks](./content-block.md) in the main area | `TOO_MANY_ITEMS` ||
|| No more than ten [menu items](./menu-item.md) and no more than ten menu sections | `TOO_MANY_ITEMS` ||
|| Keys of the `blocks`, `tags`, `buttons`, `items`, `sections`, and `actionParams` associative arrays — Latin letters, digits, hyphens, and underscores only | `KEY_CONTAIN_WRONG_SYMBOLS` ||
|| A required field of the object is not passed | `FIELD_IS_REQUIRED` ||
|| A field not present in the object description is passed | `FIELD_IS_REDUNDANT` ||
|| The field value is not in the list of allowed values, for example an unknown tag type | `ENUM_FIELD` ||
|| A multi-language value contains a language code not installed in Bitrix24 | `WRONG_LANG` ||
|#

The full list of errors is on the [crm.activity.configurable.add](../crm-activity-configurable-add.md#errors) and [crm.activity.configurable.update](../crm-activity-configurable-update.md#errors) pages.

## Object Example {#primer}

An entry about an incoming call: an icon, a heading with a tag, the customer and the assignee in the content area, a button that opens the app, and two menu items.

```json
{
    "icon": {
        "code": "call-completed"
    },
    "header": {
        "title": "Incoming call",
        "tags": {
            "status2": {
                "type": "warning",
                "title": "not transcribed"
            }
        }
    },
    "body": {
        "logo": {
            "code": "call-incoming",
            "action": {
                "type": "redirect",
                "uri": "/crm/deal/details/123/"
            }
        },
        "blocks": {
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
            "responsible": {
                "type": "lineOfBlocks",
                "properties": {
                    "blocks": {
                        "name": {
                            "type": "link",
                            "properties": {
                                "text": "Klaus Weber",
                                "bold": true,
                                "action": {
                                    "type": "redirect",
                                    "uri": "/crm/lead/details/789/"
                                }
                            }
                        },
                        "phone": {
                            "type": "text",
                            "properties": {
                                "value": "+49 999 888 7777"
                            }
                        }
                    }
                }
            }
        }
    },
    "footer": {
        "buttons": {
            "aboutClient": {
                "title": "About client",
                "action": {
                    "type": "openRestApp",
                    "actionParams": {
                        "clientId": 456
                    }
                },
                "type": "primary"
            }
        },
        "menu": {
            "showPostponeItem": false,
            "items": {
                "confirm": {
                    "title": "Confirm request",
                    "action": {
                        "type": "restEvent",
                        "id": "confirm",
                        "animationType": "loader"
                    }
                },
                "decline": {
                    "title": "Reject request",
                    "action": {
                        "type": "restEvent",
                        "id": "decline",
                        "animationType": "loader"
                    }
                }
            }
        }
    }
}
```

## Continue Learning

- [{#T}](./icon.md)
- [{#T}](./header.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./action.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)
