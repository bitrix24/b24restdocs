# Application Automation Rules

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

An introductory article about Automation Rules with key points:

- What Automation Rules are and where they are used
- What scenarios can be created by adding your own Automation Rules from applications
- Why this is better than adding application actions (link), mentioning smart scenarios

from the description of the method bizproc.event.send
An article about how waiting for results from Automation Rules and workflow actions works. Key points:

- In what scenarios are such Automation Rules and actions needed?
- What are the downsides and features?
- How does it work? This is where the method bizproc.event.send comes into play in standard formatting

from the description of the method bizproc.robot.add
- a link to the article about Automation Rules with "waiting". Also, such an article is needed

{% endnote %}

{% endif %}

#|
|| **Method** | **Description** ||
|| [bizproc.robot.add](./bizproc-robot-add.md) | Registers a new Automation Rule ||
|| [bizproc.robot.update](./bizproc-robot-update.md) | Updates the fields of an Automation Rule ||
|| [bizproc.robot.list](./bizproc-robot-list.md) | Retrieves a list of Automation Rules registered by the application ||
|| [bizproc.robot.delete](./bizproc-robot-delete.md) | Deletes a registered Automation Rule ||
|| [bizproc.event.send](./bizproc-event-send.md) | Returns the output parameters of the action specified in the action description || 
|#