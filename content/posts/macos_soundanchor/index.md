---
title: "macOS: automatic Audio Input and Ouput selection"
draft: false
date: 2025-11-23
authors:
  - name: Moritz J.
    # link: https://github.com/XYZ
    # image: https://github.com/XYZ
tags:
  - Apple
  - Audio
  - Bluetooth
  - macOS
  - Microphone
showToc: true
breadcrumbs: false
params:
  images:
    - "/images/og/macos_soundanchor.png"
---

In this post we take a look at SoundAnchor. It's a small macOS application that allows you to set priority based rankings for audio input & outputs devices like microphones or speakers. No more switching to bad, build-in Bluetooth microphones. 

<!--more-->

{{< figure
  src="soundanchor_screenshot.png" 
  alt="SoundAnchor screenshot"
  caption="SoundAnchor screenshot from the official website"
  width="50%"
  align=center
>}}

## Annoying audio device switching on macOS

macOS keeps switching to the built-in microphone of my Bose QC45 headphones - which sounds terrible.  
Additionally, when the microphone is in use, the Headphones switch to using the Bluetooth HFP profile, which significantly reduces playback quality too.

In short, I want to use my QC45 strictly as an output device, while keeping my dedicated external microphone as the default input at all times.

## Priority based & automatic audio device switching

This can be easily achieved through [**SoundAnchor**](https://apps.kopiro.me/soundanchor/) (Website/Homebrew: **free** || [AppStore](https://apps.apple.com/app/soundanchor/id6741025554): **$1.99**)

After installing the program, you will find a microphone icon in your Mac's menu bar.  
Click it and a small pop-up appears.

{{< figure
  src="microphone_icon_menu_bar.svg" 
  caption="The menu bar icon (leftmost) of SoundAnchor"
  width="50%"
  align=center
>}}

## How to use
1) Reorder your Input and Output Devices by dragging the {{< icon "menu" >}} icon next to each device.
2) Enable "auto-switching". When active, SoundAnchor automatically selects the highest-priority available device.

### Example Setup
Here's an example of my personal SoundAnchor configuration:

{{< figure
  src="personal_soundanchor_configuration.svg" 
  width="100%"
  align=center
>}}

**Input Devices**  
<span style="font-size: 22px;">①</span> UMC202HD 192K (my audio interface/microphone) sits at the top of the list, giving it the highest priority.    
*As long as* or *as soon as* the device is connected to my Mac, macOS selects it as the default Input Device.  
The audio interface is currently connected, that's why its icon is blue.  
<span style="font-size: 22px;">②</span> The Bose QC45 are at the bottom of the list. They have the lowest priority, since I **never** want them to be used as microphone/Input Device.

**Output Devices**  
<span style="font-size: 22px;">③</span> Beats Flex are greyed out, that means they are not connected.  
If I were to connect them to my Mac, they would instantly become the current/default Output Device, because they are first in the list.    
<span style="font-size: 22px;">④</span> In absence of highest priority entry, the Bose QC45 come next. Since they are currently connected, they are the default Audio Output.

If both output devices are disconnected, macOS automatically falls back to the Mac mini's built-in speakers.