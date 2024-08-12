---
layout: "zabbix"
page_title: "Zabbix: zabbix_item"
sidebar_current: "docs-zabbix-resource-item"
description: |-
  Provides a zabbix item resource. This can be used to create and manage Zabbix Item.
---

# zabbix_item

An [item](https://www.zabbix.com/documentation/current/manual/api/reference/item) is used to gather data from a host.

## Example Usage

Create a new item

```hcl
resource "zabbix_host_group" "demo_group" {
  name = "Template demo group"
}

resource "zabbix_template" "demo_template" {
  host        = "template"
  name        = "template demo"
  description = "An exemple of template with item"
  groups      = [zabbix_host_group.demo_group.name]
}

resource "zabbix_item" "demo_item" {
  name        = "demo item"
  key         = "demo.key"
  delay       = "34"
  description = "Item for the demo template"
  trends      = "300"
  history     = "25"
  host_id     = zabbix_template.demo_template.id
}
```

## Argument Reference

The following arguments are supported:

* `host_id` - (Required) ID of the host or template that the item belongs to.
* `delay` - (Required) Update interval of the item. Accepts seconds or a time unit with suffix (30s,1m,2h,1d).
* `key` - (Required) Item key.
* `name` - (Required) Name of the item.
* `type` - (Required) Type of the item. Can be `0` (Zabbix agent), `1` (SNMPv1 agent), `2` (Zabbix trapper), `3` (simple check), `4` (SNMPv2 agent), `5` (Zabbix internal), `6` (SNMPv3 agent), `7` (Zabbix agent active), `8` (Zabbix aggregate), `9` (web item), `10` (external check), `11` (database monitor), `12` (IPMI agent), `13` (SSH agent), `14` (TELNET agent), `15` (calculated), `16` (JMX agent).
* `value_type` - (Required) Type of information of the item. Can be `0` (numeric float), `1` (character), `2` (log), `3` (numeric unsigned), `4` (text).
* `interface_id` - (Optional)  ID of the item's host interface.
Not required for template items. Optional for internal, active agent, trapper, aggregate, calculated, dependent and database monitor items.
* `data_type` - (Optional, removed in v3.4) Data type of the item. Can be `0` (default decimal), `1` (octal), `2` (hexadecimal), `3` (boolean).
* `delta` - (Optional, removed in v3.4) Value that will be stored. Can be `0` (default as is), `1` (Delta, speed per second), `2` (Delta, simple change).
* `description` - (Optional) Description of the item.
* `history` - (Optional) Duration to keep item's history data. Before Zabbix Server version 3.4, an integer representing a number of days. Since Zabbix Server version 3.4, a string composed of a number and a time unit is required instead of an integer. Default is `90` for Zabbix Server version < 3.4 and `90d` for version >= 3.4.
* `trends` - (Optional) Duration to keep item's trends data. Before Zabbix Server version 3.4, an integer representing a number of days. Since Zabbix Server version 3.4, a string composed of a number and a time unit is required instead of an integer. Default is `365` for Zabbix Server version < 3.4 and `365d` for version >= 3.4.
* `trapper_host` - (Optional) Allowed hosts. Used only by trapper items.
* `status` - (Optional) Whether the trigger is enabled or disabled. Can be `0` (default, enabled), `1` (disabled).

## Import

Items can be imported using their id, e.g.

```
$ terraform import zabbix_item.new_item 123456
```
