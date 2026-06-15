# Employees: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Employees in Bitrix24 are users of the company who work with tasks, internal communications, CRM, and other tools. Within the company structure, employees are distributed across departments and teams and can be leaders, deputies, or regular participants.

The methods `humanresources.employee.*` return data from the employee's digital profile: user ID, name, position, avatar, profile link, departments, and teams. These methods can be used to find employees by name, get the total number of employees, check the user's subordinates, and find employees who belong to multiple departments.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Work with the employee list in Bitrix24](https://helpdesk.bitrix24.com/open/21379110/)

## How to Get Started

1. Find an employee using the [humanresources.employee.search](./humanresources-employee-search.md) method if part of the name is known.
2. Obtain the `userId` from the search response or using the [user.get](../../../user/user-get.md) method.
3. Get the field descriptions using the [humanresources.employee.field.list](./humanresources-employee-field-list.md) and [humanresources.employee.field.get](./humanresources-employee-field-get.md) methods if you need to understand in advance what employee data can be retrieved.
4. Check the user's subordinates using the [humanresources.employee.subordinates](./humanresources-employee-subordinates.md) method if you need to assess the composition of departments where the user is a leader or deputy.
5. Get the total number of employees using the [humanresources.employee.count](./humanresources-employee-count.md) method.
6. Find employees in multiple departments using the [humanresources.employee.multidepartment](./humanresources-employee-multidepartment.md) method if you need to check for dual employment.

## Section Limitations

- The methods in this section are available to authorized users linked to a department in the company structure.

## Connection with Other Objects

**Users.** The `userId` field corresponds to the Bitrix24 user ID. It can be obtained using the [user.get](../../../user/user-get.md) method or from the response of [humanresources.employee.search](./humanresources-employee-search.md).

**Departments and Teams.** The responses from the methods return data about the employee's connection with departments and teams. Details about a department or team can be obtained using the [humanresources.node.get](../node/humanresources-node-get.md) method.

**Participants of Departments and Teams.** If you need to change the composition of a department or team after finding an employee, use the methods [humanresources.node.member.add](../node-member/humanresources-node-member-add.md), [humanresources.node.member.move](../node-member/humanresources-node-member-move.md), and [humanresources.node.member.remove](../node-member/humanresources-node-member-remove.md).

## Overview of Methods {#all-methods}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can perform the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [humanresources.employee.search](./humanresources-employee-search.md) | Searches for employees by name ||
|| [humanresources.employee.subordinates](./humanresources-employee-subordinates.md) | Returns the user's subordinates by departments ||
|| [humanresources.employee.count](./humanresources-employee-count.md) | Returns the number of employees in the company structure ||
|| [humanresources.employee.multidepartment](./humanresources-employee-multidepartment.md) | Returns employees who belong to multiple departments ||
|| [humanresources.employee.field.list](./humanresources-employee-field-list.md) | Returns a list of employee fields ||
|| [humanresources.employee.field.get](./humanresources-employee-field-get.md) | Returns the description of an employee field ||
|#

## Continue Exploring

- [{#T}](../node/index.md)
- [{#T}](../node-member/index.md)
- [{#T}](../index.md)
