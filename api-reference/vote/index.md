# Polls, Votes: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In the news feed and messenger, you can conduct employee polls and organize votes for response options. The poll creator can configure the anonymity of responses and the ability to revote.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Polls in Bitrix24 chats: how to create and configure](https://helpdesk.bitrix24.com/open/25418811/), [Poll in the News Feed](https://helpdesk.bitrix24.com/open/25743835/)

## How to Get Started

1. Create a poll. In a messenger chat, a poll is created by the [vote.Integration.Im.send](./vote.integration.im.send.md) method. A poll in the news feed is created only in the Bitrix24 interface — through the REST API, you can manage an already created poll.
2. Retrieve the poll identifier. The [vote.Integration.Im.send](./vote.integration.im.send.md) method returns the message identifier `messageId` and the voting identifier `voteId`. For a poll in the news feed, retrieve the post `ID` using the [log.blogpost.get](../log/log-blogpost-get.md) method.
3. Work with the voting using the `vote.AttachedVote.*` methods. Each of them accepts the poll in one of three ways: by the attachment identifier `attachId`, by the combination `moduleId` + `entityType` + `entityId`, or by the signed identifier `signedAttachId`.
4. Retrieve the results using the [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getWithVoted](./vote.attachedvote.getWithVoted.md) method, and the full report — using the [vote.AttachedVote.download](./vote.attachedvote.download.md) method.

## Relationships with Other Objects

**News Feed.** A poll in the news feed is attached to a post. The link works through the post `ID`: pass it to the `vote.AttachedVote.*` methods in the `entityId` parameter along with `moduleId` set to `blog`. You can retrieve the post `ID` using the [log.blogpost.get](../log/log-blogpost-get.md) method.

**Messenger.** A poll in the chat is attached to a messenger message. The link works through the message identifier: pass `messageId` from the result of the [vote.Integration.Im.send](./vote.integration.im.send.md) method to the `vote.AttachedVote.*` methods in the `entityId` parameter along with `moduleId` set to `Im`.

**User.** The methods [vote.AttachedVote.getAnswerVoted](./vote.attachedvote.getAnswerVoted.md) and [vote.AttachedVote.getWithVoted](./vote.attachedvote.getWithVoted.md) return a list of users who voted and basic information about them: ID, name, position, image. To get detailed information about the voting user, use the [user.get](../user/user-get.md) method.

## How to Download Results

To download a file with results via a link:

1. Obtain the link from the `downloadUrl` parameter. This parameter is available in the methods:

   - [vote.AttachedVote.vote](./vote.attachedvote.vote.md)

   - [vote.AttachedVote.recall](./vote.attachedvote.recall.md)

   - [vote.AttachedVote.getWithVoted](./vote.attachedvote.getWithVoted.md)

   - [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md)

   - [vote.AttachedVote.get](./vote.attachedvote.get.md)

2. Append the Bitrix24 domain to the link from the parameter.

3. Log in to Bitrix24 in your browser and navigate to the generated link.

To download a file with results via the app or webhook, use the [vote.AttachedVote.download](./vote.attachedvote.download.md) method.

## How to Stop a Poll

A poll can be stopped in the Bitrix24 interface or using the [vote.AttachedVote.stop](./vote.attachedvote.stop.md) method. After stopping, no employees will be able to vote or change their responses. The results of the poll can be viewed and downloaded after it has been stopped.

You can only resume a poll after stopping it using the [vote.AttachedVote.resume](./vote.attachedvote.resume.md) method.

## How to Delete a Poll

To completely delete a poll, use the methods:

- [log.blogpost.delete](../log/log-blogpost-delete.md) — if the poll is linked to a post in the news feed

- [im.message.delete](../chats/messages/im-message-delete.md) — if the poll was created in a chat

These methods will delete the post or message containing the poll and its results.

## Overview of Methods {#all-methods}

> Scope: [`vote`](../scopes/permissions.md)
>
> Who can perform the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [vote.Integration.Im.send](./vote.integration.im.send.md) | Creates and sends a vote in the chat ||
|| [vote.AttachedVote.vote](./vote.attachedvote.vote.md) | Votes in the attached poll ||
|| [vote.AttachedVote.recall](./vote.attachedvote.recall.md) | Revokes a vote ||
|| [vote.AttachedVote.resume](./vote.attachedvote.resume.md) | Resumes voting ||
|| [vote.AttachedVote.stop](./vote.attachedvote.stop.md) | Stops voting ||
|| [vote.AttachedVote.get](./vote.attachedvote.get.md) | Returns data of the attached poll ||
|| [vote.AttachedVote.getAnswerVoted](./vote.attachedvote.getAnswerVoted.md) | Returns a list of users who voted for the answer ||
|| [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md) | Returns multiple polls ||
|| [vote.AttachedVote.getWithVoted](./vote.attachedvote.getWithVoted.md) | Returns poll data with information about voters ||
|| [vote.AttachedVote.download](./vote.attachedvote.download.md) | Downloads a report on the voting ||
|#