# Conditions in Prompts

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

In the pre-prompt, similar to programming, you can use `if` conditions and `switch` branching.

## Conditions

Let's start with an example:

```js
@if (engine.code = ChatGPT)
	Speak on behalf of the Automation rule.
@else
	Speak on behalf of a person.
@endif
```

As a result, depending on the selected provider on the account, the final pre-prompt will differ.

What can you write in the `@if ()` condition? Let's take a look below. Case sensitivity and extra spaces do not matter.

## System Fields

- `engine.code` — provider code (ChatGPT, [ThirdParty](*ThirdParty))
- `engine.category` — can take values text or image. However, currently, CoPilot pre-prompts only work for text.
- `context.module` — the module that calls CoPilot. For example, you can modify the pre-prompt differently if the request is for the CRM module.

An example of a condition based on a system field was at the beginning of the page. Here’s another one:
```js
@if (engine.code = ChatGPT)
	Don't forget that you are ChatGPT.
@endif
```

## Checking for Null

```js
@if (author.name = null)
	speak on behalf of an anonymous user.
@endif
```

## Negation

```js
@if (engine.code != YandexGPT)
	{context_messages}
@endif
```

## Working with Basic Markers

These are [basic markers](./markers.md), as well as pre-installed markers by the developer. However, since they contain user content, it makes sense to use them only for checking for null.

```js
@if (marker.original_message != null)
	praise for {original_message}
@else
	praise for {user_message}
@endif
```
On the other hand, through marker checking, you can rely on some system pre-installed marker that is always set by the developer under certain conditions.

```js
@if (marker.role = vip)
	write very importantly.
@endif
```

## Working with Result Markers

You can find information about result markers on the documentation page about [markers](./markers.md).

How to use them in conditions? Logically, you can only use them for null checks. For example:

```js
@if (current_result0 != null)
	This is not the user's first clarification, please be more careful!
@endif
```
An important clarification — you should only check the availability of the marker for your logic. A statement like this:

```js
@if (current_result0 != null)
	{current_result0}
@endif
```

is meaningless, as if the marker is absent, it will automatically be removed from the text.

## Functions Inside If-Conditions

You can use functions inside the if-expression condition. Currently, only the length definition `length()` is supported. You can pass any marker into this function, including user-defined ones.

```js
@if (length(marker.user_message) < 10)
	The final response should be very concise.
@else
	Write like Tolstoy.
@endif
{user_message}
```

## Setter Functions

Inside prompts, you can set temperature and tokens. Moreover, they can be different depending on [conditions](*conditions).

- `@setTemperature()`
- `@setTokens()`

Let's enhance the example from the previous section about functions using setters:

```js
@if (length(marker.user_message) < 10)
	The final response should be very concise.
	@setTemperature(0.12)
	@setTokens(200)
@else
	Write like Tolstoy.
	@setTemperature(1)
	@setTokens(1200)
@endif
```

## Branching

This is the well-known `switch` in programming. It comes in handy when you have different prompt blocks, but each of them is executed strictly in a certain order. A good example is when you have different prompt texts for different providers. Let's consider this example.

```js
@switch (engine.code)
@case(ChatGPT)
	**instructions for GPT**
@case(YandexGPT)
	**instructions for YandexGPT**
@default
	**instructions for other providers**
@endswitch
```
What can you insert into `switch`? Everything that you can in `if`.

The branching condition has the highest priority, meaning that if-expressions can be included within case blocks. There can be multiple branching blocks, although this may reduce readability.

Example of combined use of switch and if:

```js
@switch (engine.code)
@case(ChatGPT)
	you are a pig 
	@if(author.personalgender = m) pink @else blue @endif
@case(YandexGPT)
	you are a bear
@default
	you are a wolf
@endswitch
@switch (engine.code)
@case(ChatGPT)
	from Pluto
@case(YandexGPT)
	from Jupiter
@default
	@if(author.personalgender = m) from Mars @else from Venus @endif
@endswitch
```

[*ThirdParty]: ThirdParty — "third party". That is, this can be the code of a third-party provider developed by a partner.

[*conditions]: These can be both if-conditions and switch branching.