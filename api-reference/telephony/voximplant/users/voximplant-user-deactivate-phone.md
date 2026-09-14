# Deactivate the employee's SIP phone presence with voximplant.user.deactivatePhone

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not to be deployed to prod_" %}

The page is hidden from the menu, the method does not work.

- The required parameters are not specified
- Examples are missing
- Response on success is absent
- Response on error is absent

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `voximplant.user.deactivatePhone` disables the SIP phone presence for an employee. This method checks for the permission to modify the user.

The method is available to the holder of the [permission](https://helpdesk.bitrix24.com/open/18216960/) `User Settings - Modification` according to the value of this permission.

#|
|| **Parameter** / **Type** | **Description** ||
|| **USER_ID**^*^
[`integer`](../../../data-types.md) | User identifier. ||
|#

{% include [Note on required parameters](../../../../_includes/required.md) %}
