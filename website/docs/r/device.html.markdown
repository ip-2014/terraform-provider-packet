---
layout: "packet"
page_title: "Packet: packet_device"
sidebar_current: "docs-packet-resource-device"
description: |-
  Provides a Packet device resource. This can be used to create, modify, and delete devices.
---

# packet\_device

Provides a Packet device resource. This can be used to create,
modify, and delete devices.

## Example Usage

```hcl
# Create a device and add it to cool_project
resource "packet_device" "web1" {
  hostname         = "tf.coreos2"
  plan             = "baremetal_1"
  facility         = "ewr1"
  operating_system = "coreos_stable"
  billing_cycle    = "hourly"
  project_id       = "${packet_project.cool_project.id}"
}
```

```hcl
# Same as above, but boot via iPXE initially, using the Ignition Provider for provisioning
resource "packet_device" "pxe1" {
  hostname         = "tf.coreos2-pxe"
  plan             = "baremetal_1"
  facility         = "ewr1"
  operating_system = "custom_ipxe"
  billing_cycle    = "hourly"
  project_id       = "${packet_project.cool_project.id}"
  ipxe_script_url  = "https://rawgit.com/cloudnativelabs/pxe/master/packet/coreos-stable-packet.ipxe"
  always_pxe       = "false"
  user_data        = "${data.ignition_config.example.rendered}"
}
```

## Argument Reference

The following arguments are supported:

* `hostname` - (Required) The device name
* `project_id` - (Required) The id of the project in which to create the device
* `operating_system` - (Required) The operating system slug
* `facility` - (Required) The facility in which to create the device
* `plan` - (Required) The hardware config slug
* `billing_cycle` - (Required) monthly or hourly
* `user_data` (Optional) - A string of the desired User Data for the device.
* `public_ipv4_subnet_size` (Optional) - Size of allocated subnet, more
  information is in the
  [Custom Subnet Size](https://help.packet.net/technical/networking/custom-subnet-size) doc.
* `ipxe_script_url` (Optional) - URL pointing to a hosted iPXE script. More
  information is in the
  [Custom iPXE](https://help.packet.net/technical/infrastructure/custom-ipxe)
  doc.
* `always_pxe` (Optional) - If true, a device with OS `custom_ipxe` will
  continue to boot via iPXE on reboots.

## Attributes Reference

The following attributes are exported:

* `id` - The ID of the device
* `hostname`- The hostname of the device
* `project_id`- The ID of the project the device belongs to
* `facility` - The facility the device is in
* `plan` - The hardware config of the device
* `network` - The private and public v4 and v6 IPs assigned to the device
* `locked` - Whether the device is locked
* `billing_cycle` - The billing cycle of the device (monthly or hourly)
* `operating_system` - The operating system running on the device
* `state` - The status of the device
* `created` - The timestamp for when the device was created
* `updated` - The timestamp for the last time the device was updated
* `tags` - Tags attached to the device
