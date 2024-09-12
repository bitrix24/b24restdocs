# Set of Additional Content Blocks

The structure **RestAppLayoutDto** describes a set of additional content blocks.

## Parameters

{% include [Footnote on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **blocks***
[`ContentBlockDto[]`](./body.md#contentblockdto) | An array of additional content blocks ||
|#

There are also quantitative limits on `blocks`:
- Minimum quantity — `1`
- Maximum quantity — `20`

## Code Example

```js
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

