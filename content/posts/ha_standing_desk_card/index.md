---
title: "Quick Home Assistant Standing Desk Bubble Card"
draft: false
date: 2025-11-06
authors:
  - name: Moritz J.
    # link: https://github.com/XYZ
    # image: https://github.com/XYZ
tags:
  - ESPHome
  - Home Assistant
  - Standing Desk
showToc: true
params:
  images:
    - "/images/og/ha_standing_desk_card.png"
---

This quick guide shows how to create a simple [Bubble Card](https://github.com/Clooos/Bubble-Card) for your smart standing desk in Home Assistant.

<!--more-->

## Introduction

I use a BOHO Office Basic Line standing desk. It's connected to Home Assistant using a [Upsy Desky](https://www.tindie.com/products/tjhorner/upsy-desky/) (but before that I was using a [DIY solution](https://community.home-assistant.io/t/desky-standing-desk-esphome-works-with-desky-uplift-jiecang-assmann-others/383790/18) from the Home Assistant Forums).

The desk's control panel looks like this:

{{< figure
  src="standing_desk_panel.png" 
  alt="Picture of a standing desk control panel"
  width="90%"
  align=center
>}}

Here is what the finished Home Assistant card looks like:

{{< figure
  src="ha_standing_desk_card.png" 
  alt="Picture of a standing desk control panel"
  width="90%"
  align=center
>}}

The button symbols on the card correspond to the real control panel.

## Bubble Card Code

```yaml
type: custom:bubble-card
card_type: button
button_type: state
name: Schreibtisch
icon: mdi:desk
sub_button:
  - entity: button.upsy_desky_bbc92c_down
    icon: mdi:arrow-down-bold-box-outline
    tap_action:
      action: toggle
  - entity: button.upsy_desky_bbc92c_up
    icon: mdi:arrow-up-bold-box-outline
    tap_action:
      action: toggle
  - entity: button.upsy_desky_bbc92c_preset_1
    icon: mdi:numeric-1-box-outline
    tap_action:
      action: toggle
  - entity: button.upsy_desky_bbc92c_preset_2
    icon: mdi:numeric-2-box-outline
    tap_action:
      action: toggle
  - entity: button.upsy_desky_bbc92c_preset_3
    icon: mdi:numeric-3-box-outline
    tap_action:
      action: toggle
card_layout: large
entity: sensor.upsy_desky_bbc92c_desk_height
button_action:
  tap_action:
    action: none
  double_tap_action:
    action: none
  hold_action:
    action: none
tap_action:
  action: none
double_tap_action:
  action: none
hold_action:
  action: none
scrolling_effect: false
show_name: false
```

As you can see, the card mainly consists of `sub_button` elements and the desk height sensor.  
You only need to replace the entity names with the ones from your setup.

## Optional (for Upsy Desky users)

### Expose the Up and Down buttons to Home Assistant

This part is only relevant if you are also using a Upsy Desky.   
By default, it exposes entities such as ‘Preset Buttons’ and ‘Desk Height’ to Home Assistant, but not the ‘Up’ and ‘Down’ buttons.

You can expose these entities to Home Assistant by adding the following snippet to your `upsy-desky.yaml` [ESPHome](https://esphome.io/) configuration file:

```yaml
button:
  - platform: output
    output: standing_desk_up_pin
    name: Up
    duration: 300ms
  - platform: output
    output: standing_desk_down_pin
    name: Down
    duration: 300ms
```

> [!INFO]- My full `upsy-desky.yaml` configuration
> ```yaml
> substitutions:
>   name: upsy-desky-bbc94e
>   friendly_name: Upsy Desky bbc94e
> packages:
>   tj_horner.upsy_desky: github://tjhorner/upsy-desky/firmware/stock.yaml@v4.0.2
> esphome:
>   name: ${name}
>   name_add_mac_suffix: false
>   friendly_name: ${friendly_name}
> api:
>   encryption:
>     key: XYZ
> 
> wifi:
>   ssid: !secret wifi_ssid
>   password: !secret wifi_password
> 
> button:
>   - platform: output
>     output: standing_desk_up_pin
>     name: Up
>     duration: 300ms
>   - platform: output
>     output: standing_desk_down_pin
>     name: Down
>     duration: 300ms
> ```

### Fix potential build errors during an ESPHome update

Occasionally (this happened twice so far), adding the `button:` snippet caused a `- platform: outputs` error when trying to update the ESPHome firmware.

Here is a workaround.

Edit your `upsy-desky.yaml` file:

1) Remove the custom `button` snippet
2) Manually change firmware version `github://tjhorner/upsy-desky/firmware/stock.yaml@v4.0.2` to the newest version
3) Compile and flash the firmware wirelessly  
_
4) Add the `button` snippet back
5) Clean Build Files (`⋮` →  Clean Build Files)
6) Flash the ESPHome firmware again
7) Done, no more error messages
