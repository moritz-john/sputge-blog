---
breadcrumbs: true
title: "homeassistant.koplugin"
params:
  images:
    - "/images/og/homeassistant_koplugin.png"
---

<br>

<div style="text-align: center;">
  <a href="#installation--configuration" style="font-size: 24px;">Jump to the installation instructions</a>
</div>


## Use case 
This [KOReader](https://koreader.rocks/) plugin lets you control Home Assistant entities without leaving your current book!

{{< figure
  src="homeassistant_koreader.gif"
  caption="homeassistant.koplugin in action"
  width="80%"
  align=center
>}}

## Features

- Control any number of Home Assistant entities from KOReader 
- Basic service support (e.g. `light/turn_on`, `switch/toggle`, `fan/turn_on`)  
- Lightweight, unobtrusive interface  
- Simple text-based configuration  
- Success/error notifications

## Installation & Configuration:
### 1. Download the Plugin

Download the latest release and unpack `homeassistant.koplugin.zip`:  
{{< icon "github" >}} https://github.com/moritz-john/homeassistant.koplugin/releases

### 2. Edit `config.lua`
#### 2.1 Change Connection Settings

Edit the Home Assistant connection settings:

```lua {filename="config.lua"}
-- Home Assistant connection settings
host = "192.168.1.10",  -- Change to your Home Assistant IP Address
port = 8123,            -- Default Home Assistant Port
token =                 -- Change to your own Long-Lived Access Token
"PasteYourHomeAssistantLong-LivedAccessTokenHere",
```

>[!important] Long-Lived Access Token
> Create one under:   
> [**Home Assistant**](https://my.home-assistant.io/redirect/profile): *Profile → Security (scroll down) → Long-lived access tokens → Create token*  
> _Copy the token now - you won’t be able to view it again._

#### 2.2 Add your own Home Assistant Entities

For each entity you want to control, add an entry with:

```
{
id = "light.example"         → Home Assistant Entity ID
service = "light/toggle"     → Domain-specific service to call
label = "Light Example"      → Optional text label
},
```

>[!warning]
> Use the **service** format `light/turn_on`.  
> Do not use automation-style **action** syntax ~~`light.turn_on`~~.

Example entries for Home Assistant entities in `config.lua`.  
Be aware of proper indentations, `{}` and `,` otherwise you will get syntax errors.
```lua {filename="config.lua"}
{
    id = "light.reading_lamp",
    service = "light/toggle",   
    label = "Toggle: Reading Lamp",
},
{
    id = "light.all_lights",
    service = "light/turn_on",
    label = "Turn on ALL lights",
},
{
    id = "switch.coffee_machine",
    service = "switch/turn_on",
    label = "Coffee Time",
},
{
    id = "fan.ceiling_fan",
    service = "fan/turn_on",
    label = "",
},
[...]
```

>[!Info]- Behavior with empty `label`
> If you leave the `label` field empty, the submenu entry for that entity will look like this:  
> `<id> → <(service)>`  
{{< figure
  src="empty_label.png"
  width="70%"
  align=center
>}} 

#### Example Actions & Services
Here are common Home Assistant services you can use in `config.lua`:

| Entity Type      | Action Name                                              | Corresponding Service                                          | Example Entity ID          |
| :--------------- | :------------------------------------------------------- | :------------------------------------------------------------- | :------------------------- |
| **Light**        | light.turn_on <br/> light.turn_off <br/> light.toggle    | `light/turn_on` <br/> `light/turn_off` <br/> `light/toggle`    | light.reading_lamp         |
| **Switch**       | switch.turn_on <br/> switch.turn_off <br/> switch.toggle | `switch/turn_on` <br/> `switch/turn_off` <br/> `switch/toggle` | switch.outlet_couch        |
| **Fan**          | fan.turn_on <br/> fan.turn_off <br/> fan.toggle          | `fan/turn_on` <br/> `fan/turn_off` <br/> `fan/toggle`          | fan.ceiling_fan            |
| **Scene**        | scene.turn_on                                            | `scene/turn_on`                                                | scene.reading_mood         |
| **Automation**   | automation.trigger                                       | `automation/trigger`                                           | automation.bed_routine     |
| **Input Button** | input_button.press                                       | `input_button/press`                                           | input_button.wake_computer |

>[!Note] 
> Only _basic_ services are supported.  
> Additional service data (e.g. `rgb_color`) is not.

### 3. Copy files
After editing `config.lua` copy the entire `homeassistant.koplugin` folder into `koreader/plugins/`.

### 4. Restart KOReader
The plugin appears under **Tools → Page 2 → Home Assistant**.

## Requirements
- KOReader (tested with: 2025.10 "Ghost" on a Kindle Basic 2024)  
- Home Assistant & a Long-Lived Access Token
- HTTP access (HTTPS currently not supported)

## Screenshots

{{< figure
  src="tools_menu_entry.png"
  caption="Home Assistant menu entry under Tools"
  width="90%"
  align=center
>}}

{{< figure
  src="submenu_entries.png"
  caption="Example Home Assistant entities in the submenu"
  width="90%"
  align=center
>}} 

{{< figure
  src="failure_success_message.png"
  caption="Failure & Success notification"
  width="90%"
  align=center
>}} 


## Links
[homeassistant.koplugin Repository](https://github.com/moritz-john/homeassistant.koplugin)  
[KOReader Website](https://koreader.rocks/)

[Home Assistant REST API](https://developers.home-assistant.io/docs/api/rest/)  
[Home Assistant Services](https://data.home-assistant.io/docs/services/)  
[Home Assistant Performing Actions](https://www.home-assistant.io/docs/scripts/perform-actions/)


