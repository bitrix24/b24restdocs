# Office Networks: Overview of Methods

An office network is a group of IP addresses used within an organization's local network. Working with the ranges of IP addresses in the office network is done using the methods from the group timeman.networkrange.*.

> Quick Navigation: [all methods and events](#all-methods)

## Get Range

You can find out the current settings of the IP address ranges using the [timeman.networkrange.get](./timeman-networkrange-get.md) method. This method returns a list of all configured ranges.

## Set Range

The [timeman.networkrange.set](./timeman-networkrange-set.md) method creates or updates ranges. You can specify a range as:
- a single IP address. For example, `10.10.0.1`
- a block of addresses in the format `start_address-end_address`. For example, `10.0.0.0-10.255.255.255`

## Check IP Address

The [timeman.networkrange.check](./timeman-networkrange-check.md) method checks whether the specified IP address belongs to the office network ranges. If no address is specified, the system will check the current IP.

[Report on Identified Absences](../timecontrol/timeman-timecontrol-reports-get.md) contains the IP addresses at the start and end of the employee's day. Using the [timeman.networkrange.check](./timeman-networkrange-check.md) method, you can verify whether the person was in the office network at those times.

## Overview of Methods {#all-methods}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can perform the method: administrator

#| 
|| **Method** | **Description** ||
|| [timeman.networkrange.get](./timeman-networkrange-get.md) | Retrieves the network address ranges that are part of the office network ||
|| [timeman.networkrange.set](./timeman-networkrange-set.md) | Sets the network address ranges that are part of the office network ||
|| [timeman.networkrange.check](./timeman-networkrange-check.md) | Checks if the IP address is within the network address ranges of the office network ||
|#