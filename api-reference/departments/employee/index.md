# Employees: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Employees in Bitrix24 are users of the company who work with tasks, internal communications, CRM, and other tools. Within the company structure, employees are distributed across departments and teams and can be leaders, deputies, or regular participants.

The methods `humanresources.employee.*` help you find an employee by name, count the company's employees and the user's subordinates, and retrieve employees who belong to multiple departments. The search returns data from the employee's digital profile: user ID, name, position, avatar, profile link, departments, and teams.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Work with the employee list in Bitrix24](https://helpdesk.bitrix24.com/open/21379110/)

{% note info "" %}

The methods in this section belong to REST 3.0 and are called at a URL containing `/rest/api/`. The PHP SDK, BX24.js, and PHP CRest do not support this URL yet. The call specifics and response format are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

## How to Get Started

1. If part of the name is known, find the employee using the [humanresources.employee.search](./humanresources-employee-search.md) method.
2. Retrieve the employee's `userId` from the search response.
3. If you need to understand in advance what employee data can be retrieved, call the [humanresources.employee.field.list](./humanresources-employee-field-list.md) and [humanresources.employee.field.get](./humanresources-employee-field-get.md) methods.
4. If you need to find out in which departments the user is a leader or deputy and how many subordinates they have, call the [humanresources.employee.subordinates](./humanresources-employee-subordinates.md) method.
5. If you need the total number of employees, call the [humanresources.employee.count](./humanresources-employee-count.md) method.
6. If you need to check for dual employment, find employees in multiple departments using the [humanresources.employee.multidepartment](./humanresources-employee-multidepartment.md) method.


## Section Limitations

- The methods in this section are available to authorized users linked to a department in the company structure. The field methods `humanresources.employee.field.list` and `humanresources.employee.field.get` are available to any user.


## Connection with Other Objects

**Users.** The `userId` field corresponds to the Bitrix24 user ID. It can be obtained using the [user.get](../../user/user-get.md) method or from the response of [humanresources.employee.search](./humanresources-employee-search.md).

**Departments and Teams.** The methods return the employee's departments and teams. Additional data about a department or team can be retrieved using the [humanresources.node.get](../node/humanresources-node-get.md) method.

**Participants of Departments and Teams.** If you need to change the composition of a department or team after finding an employee, use the methods [humanresources.node.member.add](../node-member/humanresources-node-member-add.md), [humanresources.node.member.move](../node-member/humanresources-node-member-move.md), and [humanresources.node.member.remove](../node-member/humanresources-node-member-remove.md).


## Overview of Methods {#all-methods}

> Scope: [`humanresources`](../../scopes/permissions.md)
>
> Who can perform the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [humanresources.employee.search](./humanresources-employee-search.md) | Searches for employees by name ||
|| [humanresources.employee.subordinates](./humanresources-employee-subordinates.md) | Returns the number of the user's subordinates by department ||
|| [humanresources.employee.count](./humanresources-employee-count.md) | Returns the number of employees in the company structure ||
|| [humanresources.employee.multidepartment](./humanresources-employee-multidepartment.md) | Returns employees who belong to multiple departments ||
|| [humanresources.employee.field.list](./humanresources-employee-field-list.md) | Returns a list of employee fields ||
|| [humanresources.employee.field.get](./humanresources-employee-field-get.md) | Returns the description of an employee field ||
|#


## Continue Exploring

- [{#T}](../node/index.md)
- [{#T}](../node-member/index.md)
- [{#T}](../index.md)
