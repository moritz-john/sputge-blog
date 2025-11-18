---
title: "How to Print Apple Notes on a Bluetooth Receipt Printer"
draft: false
date: 2025-11-18
authors:
  - name: Moritz J.
    # link: https://github.com/XYZ
    # image: https://github.com/XYZ
tags:
  - Apple
  - Bluetooth
  - To-Do
  - Printer
showToc: true
breadcrumbs: false
params:
  images:
    - "/images/og/apple_notes_to_receipt_printer_bluetooth.png"
---

After testing around 20 apps that claimed to support Bluetooth ESC/POS printing from an iPhone, I finally found **PrinterApp**.
It was the most promising option, and although it lacked plain-text input from the Share Sheet, the developer was quick to implement the feature. (Thanks again!)

<!--more-->

{{< figure
  src="printed_apple_note.jpeg"
  alt="Screenshot of a simple Apple Shortcut"
  width="60%"
  align=center
>}}

> [!INFO] Difference from [*Printing To-Do lists from Apple Notes on a cheap ESC/POS receipt printer*](/posts/apple_notes_to_receipt_printer_usb) 
> This guide explains a simple method using an iPhone + Bluetooth.
The other post covers a more advanced setup using a Linux server + USB, which supports automatic formatting but requires more work to implement.

## Requirements

- An ESC/POS printer with Bluetooth support (e.g.: https://aliexpress.com/item/1005006625823799.html)
- An iPhone
- [**Thermal printer PrinterApp**](https://apps.apple.com/en/app/thermal-printer-printerapp/id6748481333?l=en-GB) from the AppStore.

### Connect to Your Receipt Printer via PrinterApp

<p style="font-size: 18px;"> 
1) Turn on your Bluetooth ESC/POS receipt printer.
</p>

<p style="font-size: 18px;"> 
2) Open PrinterApp and tap <b>'{{< icon "printer" >}} Connect Printer'</b>.
</p>

{{< figure
  src="PrinterApp_overview.png"
  alt="Screenshot of a simple Apple Shortcut"
  width="70%"
  align=center
>}}

<p style="font-size: 18px;"> 
3) Select your Bluetooth printer from the list.
</p>

{{< figure
  src="PrinterApp_printer_list.png"
  alt="Screenshot of a simple Apple Shortcut"
  width="70%"
  align=center
>}}

<p style="font-size: 18px;"> 
4) After connecting successfully, your printer’s name will appear at the top of the app.
</p>

{{< figure
  src="PrinterApp_connected.png"
  alt="Screenshot of a simple Apple Shortcut"
  width="70%"
  align=center
>}}

### Share from Apple Notes to PrinterApp

<p style="font-size: 18px;"> 
1) Leave PrinterApp running in the background and switch to Apple Notes.
</p>

<p style="font-size: 18px;"> 
2) Open the note you want to print and tap the <b>"Share"</b> icon.
</p>

{{< figure
  src="apple_notes_share_icon.png"
  alt="Screenshot of a simple Apple Shortcut"
  width="70%"
  align=center
>}}

<p style="font-size: 18px;"> 
3) In the Share Sheet, choose <b>"Send Copy"</b> and select the <b>PrinterApp</b> icon.
</p>

{{< figure
  src="apple_notes_share_menu.png"
  alt="Screenshot of a simple Apple Shortcut"
  width="70%"
  align=center
>}}

> [!INFO] Can't see the PrinterApp icon?
> If the PrinterApp icon doesn’t appear:  
> • Scroll to the far right of the app row.  
> • If still hidden, tap **"{{<icon "dots-horizontal">}} More"** and enable PrinterApp or add it to your favourites for quicker access.

<p style="font-size: 18px;"> 
4) Done! Your Bluetooth receipt printer should start printing and the PrinterApp app shows you a success message:
</p>

{{< figure
  src="PrinterApp_success.png"
  alt="Screenshot of a simple Apple Shortcut"
  width="70%"
  align=center
>}}


## Limitations

The PrinterApp prints your whole note as is!   
If you want checkboxes such as `[ ]` at the start of each item, you need to add them manually inside Notes.