---
layout: page
title: "Avi-on"
description: "Instructions on how to setup GE Avi-on Bluetooth dimmers within Home Assistant."
date: 2017-01-17 23:17
sidebar: true
comments: false
sharing: true
footer: true
ha_category: Light
ha_iot_class: Assumed State
logo: avi-on.png
ha_release: 0.37
redirect_from:
 - /components/light.avion/
---

Support for the Avi-on Bluetooth dimmer switch [Avi-On](http://avi-on.com/).

## {% linkable_title Setup %}

If you want to add your devices manually (like in the example below) then you need to get the API key. The API key can be obtained by executing the following command:

```bash
$ curl -X POST -H "Content-Type: application/json" \
    -d '{"email": "fakename@example.com", "password": "password"}' \
    https://admin.avi-on.com/api/sessions | jq
```

with the email and password fields replaced with those used when registering the device via the mobile app. The pass phrase field of the output should be used as the API key in the configuration.

## {% linkable_title Configuration %}

To enable these lights, add the following lines to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
light:
  - platform: avion
```

There are two ways to configure this component: username & password, or list of devices. You must choose one.

{% configuration %}
username:
  description: The username used in the Avion app. If username and password are both provided, all associated switches will automatically be added to your configuration.
  required: false
  type: string
password:
  description: The password used in the Avion app.
  required: false
  type: string
devices:
  description: An optional list of devices with their Bluetooth addresses.
  required: false
  type: list
  keys:
    name:
      description: A custom name to use in the frontend.
      required: false
      type: string
    api_key:
      description: The API Key.
      required: true
      type: string
    id:
      description: The ID of the dimmer switch. Only needed for independent control of multiple devices.
      required: true
      type: string
{% endconfiguration %}

## {% linkable_title Full example %}

If username and password are not supplied, devices must be configured manually like so:

```yaml
# Manual device configuration.yaml entry
light:
  - platform: avion
    devices:
      00:21:4D:00:00:01:
        name: Light 1
        api_key: YOUR_API_KEY
```

For independent control of multiple devices, you must specify each device's ID (integer starting with 1). Each switch's ID can be guessed or detected from the Avi-On API.

```yaml
# Manual device configuration.yaml entry
light:
  - platform: avion
    devices:
      00:21:4D:00:00:01:
        name: Light 1
        api_key: YOUR_API_KEY
        id: 1
      00:21:4D:00:00:02:
        name: Light 1
        api_key: YOUR_API_KEY
        id: 2
```
