# Markers in Prompts

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

Below is the instruction on the markers that our AI module understands and can correctly interpret for the neural network.

## Basic Markers

`{original_message}` — the original message for the pre-prompt. In the text editor architecture, this is all the text typed or the manually selected part of the typed text.  
Example of a pre-prompt:

```
Continue the text: {original_message}
```
As a result, the text will be continued based on the text typed above or the manually selected part of the text.

`{user_input}` — what the user has directly entered.  
Example of a pre-prompt:

```
Write a story about {user_message}
```

These markers can also be used together:  
```
Write a story about {user_message} continuing the text: {original_message}.
```

`{role}{/role}` — a paired marker that indicates the neural network's role. Inside, the text is set in free form, giving the neural network some instructions. For example, "speak like a robot." On one hand, it seems that the same can be written simply in the pre-prompt. On the other hand, instructions are still instructions, and neural networks take them more seriously.

Features of using the role marker:
- Other markers can be used inside the instruction
- Only the first instruction is used; the others are simply cut out
- The marker is added after [if-conditions](./conditions.md), meaning one **{role}** block can be used for `if` and another for `else`

## Additional Markers

`{author_message}` — the text of the original post or task description.  
Example:

```
Highlight the main points based on the text: {author_message}
```

`{context_messages}` — the context "how much will fit." For example, the original post + all recent comments, or chat correspondence over a certain period. Or task description + comments on it.  
Example:

```
Formulate the task result based on its description and comments: {context_messages}
```

These are all the basic markers that can be expanded by developers, but it's better not to rely on that (unless a mutually beneficial agreement is in place).

`{language}` — the language of the account (not the user!). For example, German, English, Spanish — these values will replace the marker.  
Example of a pre-prompt:
```
Translate the text into {language}. Text: {original_message}
```

## Author Markers

When we work with contextual messages, each message from the context has an author, their position, and other characteristics. Since the author is a user, we can use the user's string fields. A list of such fields can be obtained using the [user.fields](../../user/user-fields.md) method.

When writing a marker, one rule must be followed. Everything should be in lowercase: `{author.lower_case_field}`.

Let's look at an example. We will use the NAME and WORK_POSITION fields of the author in the pre-prompt:  
```
Praise the author.  
The post you need to praise: {original_message}  
Author's name: {author.name}  
Author's position: {author.work_position}
```

## Markers of the Current Result

During the operation of CoPilot, a mini-dialog occurs. You ask a question (or work with text), and you receive a result that you can continue to work with.

A set of results is formed, which you can refer to in the markers. Currently, the stack size is three messages.

This set has several features:
1. It is filled from the bottom up, meaning the most recent result is at the bottom.
2. The most recent result is the one that was visible at the moment of sending the new request (not the one that will be received at the moment of the request).
3. Numbering starts from zero.

Example:

```
{current_result_0} — the result that was visible at the moment of sending the request (using the prompt)  
{current_result_1} — the previous such result  
{current_result_2} — the result before the previous one  
```

If any markers are still empty, they are cut out from the text.