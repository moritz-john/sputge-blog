---
breadcrumbs: true
title: "homeassistant.koplugin"
---

> Documentation for {{< icon "github" >}} https://github.com/moritz-john/homeassistant.koplugin

<br>

<div style="text-align: center;">
  <a href="#installation--configuration" style="font-size: 28px;">Jump to the installation instructions</a>
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

- Control an unlimited number of Home Assistant entities from KOReader.
- Support for basic actions like `light.turn_on`, `switch.toggle` or `fan.turn_on`
- Lightweight, non-intrusive interface while reading your book.
- Simple, editable configuration file.  
- Success and error notifications.

## Installation & Configuration:
### 1. Download the Plugin

Download the latest release from the plugin repository:  
https://github.com/moritz-john/homeassistant.koplugin/releases

Unpack `homeassistant.koplugin.zip`

### 2. Edit `config.lua`
#### 2.1 Change Connection Settings

Edit the Home Assistant connection settings:

```lua {filename="config.lua"}
-- Home Assistant connection settings
host = "192.168.1.10", -- Change to your Home Assistant IP Address
port = 8123,            -- Default Home Assistant Port
token =                 -- Change to your own Long-Lived Access Token
"PasteYourHomeAssistantLong-LivedAccessTokenHere",
```

>[!important]- Long-Lived Access Token
> You can create a Long-Lived Access Token here:    
> **Home Assistant**: *Profile → Security (scroll down) → Long-lived access tokens → Create token*  
> _Copy the token now - you won’t be able to view it again._

#### 2.2 Add your own Home Assistant Entities

For each entity you want to control, add an entry with:

```
{
id = "light.example"         → Home Assistant Entity ID
service = "light/toggle"     → Domain specific service to execute
label = "Light Example"      → Optional menu label
},
```

>[!warning]
> Make sure that you use the **service** syntax: e.g. `light/turn_on` and **not** the action syntax ~~`light.turn_on`~~ in `config.lua`.


```lua {filename="config.lua"}
{
    id = "light.reading_lamp",      -- Home Assistant Entity ID
    service = "light/toggle",       -- <domain>/<service>
    label = "Toggle: Reading Lamp", -- Optional: custom menu label
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

>[!Info]- Optional menu `label`
> If you leave the **label** field empty, the submenu entry for that entity will look like this:  
> `<id> → <(service)>`  
{{< figure
  src="empty_label.png"
  width="70%"
  align=center
>}} 

#### Example Actions & Services
Here is a list of common **Entity Types**, their [**Actions**](https://www.home-assistant.io/docs/scripts/perform-actions/) and [**Services**](https://data.home-assistant.io/docs/services/).  
You can use it to complete your `config.lua` file.

| Entity Type      | Action Name                                              | Corresponding Service                                          | Example Entity ID          |
| :--------------- | :------------------------------------------------------- | :------------------------------------------------------------- | :------------------------- |
| **Light**        | light.turn_on <br/> light.turn_off <br/> light.toggle    | `light/turn_on` <br/> `light/turn_off` <br/> `light/toggle`    | light.reading_lamp         |
| **Switch**       | switch.turn_on <br/> switch.turn_off <br/> switch.toggle | `switch/turn_on` <br/> `switch/turn_off` <br/> `switch/toggle` | switch.outlet_couch        |
| **Fan**          | fan.turn_on <br/> fan.turn_off <br/> fan.toggle          | `fan/turn_on` <br/> `fan/turn_off` <br/> `fan/toggle`          | fan.ceiling_fan            |
| **Scene**        | scene.turn_on                                            | `scene/turn_on`                                                | scene.reading_mood         |
| **Automation**   | automation.trigger                                       | `automation/trigger`                                           | automation.bed_routine     |
| **Input Button** | input_button.press                                       | `input_button/press`                                           | input_button.wake_computer |

>[!Info] Actions & \<domain>\/\<service>
> **homeassistant.koplugin** supports _basic_ actions.   
> e.g.: action [light.turn_on](https://www.home-assistant.io/integrations/light/#action-lightturn_on) will be translated to the service API call `/api/services/light/turn_on`.  
> Adding an optional data attribute like `rgb_color` is not supported

### 3. Copy files
After editing `config.lua` copy the whole `homeassistant.koplugin` folder into `koreader/plugins/`.

### 4. Restart KOReader
Access the plugin under **Tools → Page 2 → Home Assistant**.

## Requirements
- KOReader (tested with: 2025.10 "Ghost" on a Kindle Basic 2024)  
- Home Assistant & Long-Lived Access Token
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






