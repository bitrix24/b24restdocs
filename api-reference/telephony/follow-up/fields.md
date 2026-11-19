# Call Follow-up Fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A call follow-up contains call metadata, attendees, recording, and AI blocks: transcription, overview, summary, insights, and efficiency rating.

Fields can be passed in the `select` parameter of the [call.followup.list](./call-followup-list.md), [call.followup.get](./call-followup-get.md), [syncconnector.followup.list](./syncconnector-followup-list.md), and [syncconnector.followup.get](./syncconnector-followup-get.md) methods. All four methods return the same fields.

For Bitrix24 Sync calls, some fields are filled in differently, whichever set of methods returns the Follow-up: [call.followup.*](./index.md#all-methods) or [syncconnector.followup.*](./index.md#sync-methods). The differences are described in the [Field Differences in Bitrix24 Sync](#sync) section.

## Which Fields Can Be Passed in Select {#select-paths}

In `select`, you can pass [root object](#root-object) fields and available nested paths for AI blocks:

- `transcription`, `transcription.language`, `transcription.segments`
- `overview`, `overview.topic`, `overview.detailedTakeaways`, `overview.meetingType`, `overview.agenda`, `overview.agreements`, `overview.actionItems`, `overview.meetings`
- `summary`
- `insights`, `insights.speakerEvaluationAvailable`, `insights.speakerAnalysis`, `insights.meetingStrengths`, `insights.meetingWeaknesses`, `insights.speechStyleInfluence`, `insights.engagementLevel`, `insights.areasOfResponsibility`, `insights.finalRecommendations`
- `evaluation`, `evaluation.efficiencyValue`, `evaluation.calendar`, `evaluation.criteria`

You cannot pass fields inside `items` arrays in `select`. For example, instead of `participants.name`, pass `participants`, and instead of `overview.actionItems.actionItem`, pass `overview.actionItems`. The method will return the entire array with the available fields.

## Root Object {#root-object}

#|
|| **Name**
`type` | **Description** ||
|| **callId**
[`integer`](../../data-types.md) | Follow-up identifier:

- for a Bitrix24 call — the call identifier
- for a Bitrix24 Sync call — `10000000000` plus the identifier of the detailed analysis page. For example, page `128` corresponds to `callId` `10000000128` ||
|| **source**
[`string`](../../data-types.md) | Follow-up source:

- `call` — a Bitrix24 call
- `sync` — a Bitrix24 Sync call

The field is returned only if you pass it in `select` ||
|| **callType**
[`integer`](../../data-types.md) | Call type:

- `1` — instant call
- `2` — persistent conference
- `3` — large room ||
|| **chatId**
[`integer`](../../data-types.md) | Identifier of the chat the call belongs to. If there is no chat, the value is `null` ||
|| **initiatorId**
[`integer`](../../data-types.md) | Identifier of the user who started the call ||
|| **startDate**
[`string`](../../data-types.md) | Call start date in ISO 8601 format ||
|| **endDate**
[`string`](../../data-types.md) | Call end date in ISO 8601 format. If the call is still ongoing, the value is `null` ||
|| **durationSeconds**
[`integer`](../../data-types.md) | Duration of the call in seconds ||
|| **uuid**
[`string`](../../data-types.md) | Call session UUID ||
|| **language**
[`string`](../../data-types.md) | Transcription language code, e.g., `ru` or `en` ||
|| **version**
[`integer`](../../data-types.md) | Maximum schema version among saved AI blocks ||
|| **participants**
[`array`](../../data-types.md) | Call participants [(detailed description)](#participant) ||
|| **outcomes**
[`array`](../../data-types.md) | List of ready AI blocks:

- `transcription` — call transcription
- `overview` — meeting overview
- `summary` — call summary
- `insights` — insights and speaker analysis
- `evaluation` — meeting effectiveness assessment ||
|| **createdAt**
[`string`](../../data-types.md) | Date when the last AI block for the call was retained, in ISO 8601 format ||
|| **tracks**
[`array`](../../data-types.md) | Call recordings [(detailed description)](#track) ||
|| **transcription**
[`object`](../../data-types.md) | Call transcription [(detailed description)](#transcription) ||
|| **overview**
[`object`](../../data-types.md) | Meeting overview [(detailed description)](#overview) ||
|| **summary**
[`object`](../../data-types.md) | Call summary by time segments [(detailed description)](#summary) ||
|| **insights**
[`object`](../../data-types.md) | Insights and speaker analysis [(detailed description)](#insights) ||
|| **evaluation**
[`object`](../../data-types.md) | Meeting effectiveness assessment [(detailed description)](#evaluation) ||
|#

## Field Differences in Bitrix24 Sync {#sync}

For Bitrix24 Sync call Follow-ups, the field keys and types are the same as for Bitrix24 calls, but some fields have different values. This applies to both [call.followup.*](./index.md#all-methods) and [syncconnector.followup.*](./index.md#sync-methods):

#|
|| **Name**
`type` | **Value in Bitrix24 Sync** ||
|| **callId**
[`integer`](../../data-types.md) | Built from the identifier of the detailed analysis page. One call can produce several pages, one for each call part.

- [syncconnector.followup.*](./index.md#sync-methods) return the page identifier
- [call.followup.*](./index.md#all-methods) return `10000000000` plus the page identifier ||
|| **source**
[`string`](../../data-types.md) | Always `sync`. The field is returned only if you pass it in `select` ||
|| **callType**
[`integer`](../../data-types.md) | Always `1` ||
|| **initiatorId**
[`integer`](../../data-types.md) | Always `null`: Sync does not pass to Bitrix24 who started the call ||
|| **startDate**
[`string`](../../data-types.md) | Start of the call part. If the page does not specify the start and end times of the call part, this field contains the time when the detailed analysis page became available ||
|| **endDate**
[`string`](../../data-types.md) | End of the call part. If the page does not specify the start and end times of the call part, this field is `null` ||
|| **durationSeconds**
[`integer`](../../data-types.md) | Duration of the call part in seconds. If the page does not specify the start and end times of the call part, this field is `null` ||
|| **uuid**
[`string`](../../data-types.md) | Sync call key shared by all its parts: use it to group the pages of one call. The value is not in UUID format ||
|| **participants**
[`array`](../../data-types.md) | Bitrix24 users who spoke in this call part. Guests are not listed ||
|| **participants[].talkedSeconds**
[`integer`](../../data-types.md) | How many seconds the user spoke — the sum of the durations of their transcription segments. Without a transcription — `null` ||
|| **createdAt**
[`string`](../../data-types.md) | The time the detailed analysis page became available ||
|| **tracks**
[`array`](../../data-types.md) | Audio recording of the Follow-up on Drive. The `type` field is always `record`, and `callId` has the same value as the root `callId` field. The array is empty if there is no recording or the user has no access to the file ||
|#

## Participants[] Object {#participant}

#|
|| **Name**
`type` | **Description** ||
|| **userId**
[`integer`](../../data-types.md) | User identifier ||
|| **talkedSeconds**
[`integer`](../../data-types.md) | User participation time in the call in seconds. For a Bitrix24 Sync call, the value means something else — see [Field Differences in Bitrix24 Sync](#sync) ||
|| **name**
[`string`](../../data-types.md) | User's first name ||
|| **avatar**
[`string`](../../data-types.md) | URL of the user's avatar ||
|| **workPosition**
[`string`](../../data-types.md) | Position of the user ||
|#

## Tracks[] Object {#track}

#|
|| **Name**
`type` | **Description** ||
|| **trackId**
[`integer`](../../data-types.md) | Call recording identifier ||
|| **type**
[`string`](../../data-types.md) | Recording type ||
|| **fileId**
[`integer`](../../data-types.md) | Identifier of the file ||
|| **diskFileId**
[`integer`](../../data-types.md) | Identifier of the file on Drive ||
|| **duration**
[`integer`](../../data-types.md) | Recording duration in seconds ||
|| **fileSize**
[`integer`](../../data-types.md) | File size in bytes ||
|| **fileName**
[`string`](../../data-types.md) | File name ||
|| **mimeType**
[`string`](../../data-types.md) | File MIME type ||
|| **callId**
[`integer`](../../data-types.md) | Follow-up identifier, the same value as in the root `callId` field ||
|| **relUrl**
[`string`](../../data-types.md) | Relative file URL ||
|| **url**
[`string`](../../data-types.md) | Absolute file URL ||
|| **dateCreate**
[`string`](../../data-types.md) | Recording registration date in ISO 8601 format ||
|#

## Transcription Object {#transcription}

#|
|| **Name**
`type` | **Description** ||
|| **language**
[`string`](../../data-types.md) | Transcription language code ||
|| **segments**
[`array`](../../data-types.md) | Utterances in chronological order [(detailed description)](#transcription-segment) ||
|#

### transcription.segments[] Object {#transcription-segment}

#|
|| **Name**
`type` | **Description** ||
|| **userId**
[`integer`](../../data-types.md) | Identifier of the user who spoke the utterance ||
|| **userName**
[`string`](../../data-types.md) | User's first name ||
|| **start**
[`string`](../../data-types.md) | Utterance start time relative to the beginning of the call in `HH:MM:SS` format ||
|| **end**
[`string`](../../data-types.md) | Utterance end time relative to the beginning of the call in `HH:MM:SS` format ||
|| **text**
[`string`](../../data-types.md) | Utterance text. Mention format depends on the `mentionFormat` parameter ||
|#

## Overview Object {#overview}

#|
|| **Name**
`type` | **Description** ||
|| **topic**
[`string`](../../data-types.md) | Brief meeting topic ||
|| **detailedTakeaways**
[`string`](../../data-types.md) | Detailed meeting outcomes ||
|| **meetingType**
[`object`](../../data-types.md) | Meeting type:

- `explanation` — explanation of the meeting type
- `typeTag` — meeting type code
- `title` — meeting type name ||
|| **agenda**
[`object`](../../data-types.md) | Meeting agenda:

- `explanation` — agenda description
- `quote` — quote from the call ||
|| **agreements**
[`array`](../../data-types.md) | Agreements. Each item may contain:

- `agreement` — agreement text
- `quote` — quote from the call ||
|| **actionItems**
[`array`](../../data-types.md) | Action items based on the meeting results. Each item may contain:

- `actionItem` — action with participant mentions
- `actionItemMentionLess` — action without participant mentions
- `quote` — quote from the call ||
|| **meetings**
[`array`](../../data-types.md) | Scheduled follow-up meetings. Each item may contain:

- `meeting` — meeting description with mentions of participants
- `meetingMentionLess` — meeting description without mentions of participants
- `quote` — a quote from the call ||
|#

## Summary Object {#summary}

#|
|| **Name**
`type` | **Description** ||
|| **segments**
[`array`](../../data-types.md) | Summary segments [(detailed description)](#summary-segment) ||
|#

### summary.segments[] Object {#summary-segment}

#|
|| **Name**
`type` | **Description** ||
|| **start**
[`string`](../../data-types.md) | Segment start time relative to the start of the call in `HH:MM:SS` format ||
|| **end**
[`string`](../../data-types.md) | Segment end time relative to the start of the call in `HH:MM:SS` format ||
|| **title**
[`string`](../../data-types.md) | Segment title ||
|| **summary**
[`string`](../../data-types.md) | Segment summary ||
|#

## Insights Object {#insights}

#|
|| **Name**
`type` | **Description** ||
|| **speakerEvaluationAvailable**
[`boolean`](../../data-types.md) | Whether speaker ratings are available. For regions outside the CIS, the value is `false` ||
|| **speakerAnalysis**
[`array`](../../data-types.md) | Speaker analysis [(detailed description)](#speaker-analysis). Items are sorted by share of speech and efficiency rating ||
|| **meetingStrengths**
[`array`](../../data-types.md) | Meeting strengths. Each item contains:

- `strengthTitle` — strength name
- `strengthExplanation` — strength explanation ||
|| **meetingWeaknesses**
[`array`](../../data-types.md) | Meeting weaknesses. Each item contains:

- `weaknessTitle` — weakness name
- `weaknessExplanation` — weakness explanation ||
|| **speechStyleInfluence**
[`string`](../../data-types.md) | Assessment of the impact of communication style on the meeting outcome ||
|| **engagementLevel**
[`string`](../../data-types.md) | Assessment of participant engagement ||
|| **areasOfResponsibility**
[`string`](../../data-types.md) | Areas of responsibility defined based on the meeting results ||
|| **finalRecommendations**
[`string`](../../data-types.md) | Recommendations for future meetings ||
|#

### insights.speakerAnalysis[] Object {#speaker-analysis}

#|
|| **Name**
`type` | **Description** ||
|| **userId**
[`integer`](../../data-types.md) | User identifier ||
|| **detailedInsight**
[`string`](../../data-types.md) | Detailed analysis of speaker participation ||
|| **efficiencyValue**
[`integer`](../../data-types.md) | Speaker efficiency rating from `0` to `100` ||
|| **evaluationCriteria**
[`object`](../../data-types.md) | Assessment criteria map. Key — criterion code, value — object with fields:

- `value` — criterion check result, boolean value
- `criteria` — criterion description
- `title` — criterion name ||
|| **talkPercentage**
[`integer`](../../data-types.md) | Speaker's share of speech in percent. Field is added based on transcription data ||
|| **duration**
[`integer`](../../data-types.md) | Speaker's speech duration in seconds. Field is added based on transcription data ||
|| **durationFormat**
[`string`](../../data-types.md) | Formatted speaker speech duration. Field is added based on transcription data ||
|#

## Evaluation Object {#evaluation}

#|
|| **Name**
`type` | **Description** ||
|| **efficiencyValue**
[`integer`](../../data-types.md) | Overall meeting efficiency rating from `0` to `100` ||
|| **calendar**
[`object`](../../data-types.md) | Calendar planning assessment. Contains field:

- `overhead` — meeting exceeded scheduled time, boolean value ||
|| **criteria**
[`object`](../../data-types.md) | Assessment criteria map. Key — criterion code, value — object with fields:

- `value` — criterion check result, boolean value
- `criteria` — criterion description
- `thoughts` — rating explanation
- `title` — criterion name ||
|#
