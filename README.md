# Virtual Garage Door SMJ (Konnected Optimized) for Hubitat

A Hubitat Parent/Child app architecture designed to sync a Virtual Garage Door device with a physical contact sensor and a Konnected pushable button. 

This app ensures the virtual device accurately reflects the physical door's state (even if opened/closed manually), includes safety verification delays, audible buzzer warnings, and Hubitat Safety Monitor (HSM) mode restrictions.

## 🌟 Features

- **Konnected Optimized**: Uses `capability.pushableButton` for both the garage door opener trigger and the audible buzzer.
- **Bi-Directional Sync**: Automatically updates the virtual device if the physical door is opened or closed manually.
- **Safety Verification**: Double-checks the physical sensor after a command to ensure the door actually moved. Resets the virtual device if it fails.
- **HSM Integration**: Restricts garage door operation to specific Hubitat Safety Monitor (HSM) modes (e.g., Disarmed, Home, Away).
- **Audible Warning**: Triggers a Konnected buzzer before closing, with a user-definable delay and a "silence switch" override.
- **Parent/Child Architecture**: Easily manage multiple garage doors/gates from a single parent app. Deleting the parent automatically cleans up all child instances.

## 📋 Prerequisites

1. **Hubitat Elevation Hub**
2. **Virtual Garage Door Device**: A virtual device with `capability.garageDoorControl` and `capability.contactSensor` (e.g., "Virtual Garage Door" or "Virtual Switch" configured appropriately). **Easiest is to setup a virtual device (1 for each garage door you want to control) and change the driver for each one created to the built in Virtual Garage Door Controller Driver, I also reccomend nameing them something that you will remember they are virtual so you don't confuse them with the real sensors/buttons later below when setting up the app** ex: "1 Car Garage Door Controller" or "2 Car Garage Door Controller" etc.
3. **Physical Contact Sensor**: To detect the actual open/closed state of the door/gate.
4. **Konnected Button Trigger**: Configured in Hubitat with `capability.pushableButton` (one for the opener relay, and optionally one for the buzzer).

## 🛠️ Installation Instructions

 ⚠️ **CRITICAL**: Because this uses a Parent/Child architecture, **you must install the Parent App first**. Do *not* try to create the Child App directly from the "Add User App" menu, or you will get an "Error 404: has no parent" message.

### Step 1: Add the Code to Hubitat
1. Log in to your Hubitat hub and navigate to **Apps Code**.
2. Click **+ New App**.
3. Paste the **Parent App** code. Ensure the `namespace` (e.g., `smj_garage`) and `name` (`Virtual Garage Door Manager`) are noted. Click **Save**.
4. Click **+ New App** again.
5. Paste the **Child App** code. Ensure the `namespace` *exactly matches* the Parent App's namespace, and `singleInstance: false` is set in the definition. Click **Save**.

### Step 2: Install the Parent App
1. Navigate to the main **Apps** dashboard (left menu).
2. Scroll to the bottom and click **+ Add User App**.
3. Select **Virtual Garage Door Manager** from the list. *(Do not select the Child app)*.
4. Click **Done** to create the parent instance.

### Step 3: Create Child Instances (Your Garage Doors)
1. On the main **Apps** dashboard, click on the newly created **Virtual Garage Door Manager** app to open its settings.
2. Tap the **"Add New Garage Door"** button *inside* the app's interface.
3. This will open the Child App configuration screen. Select your devices, configure your timings, and click **Done**.
4. Repeat Step 3 for each additional garage door or gate you want to manage.

## ⚙️ Configuration Guide

- **Garage Door Button**: The Konnected relay/button that triggers the physical door.
- **Physical Sensor**: The contact sensor monitoring the actual door.
- **Virtual Garage Door**: The virtual device that Hubitat dashboards/Rules will interact with.
- **Virtual Garage Door Sensor**: The contact sensor attribute of the *same* virtual device above.
- **Operation Delay**: Time (in seconds) to wait before verifying the door actually moved. (Default: 25s).
- **Audible Warning**: (Optional) Konnected buzzer button, delay before closing, and a switch that, when turned ON, silences the buzzer.
- **HSM Mode Restriction**: Select which HSM states allow the door to operate. If blocked, an optional push notification is sent.
- **Notifications**: Select Hubitat notification devices (e.g., Pushover, Hubitat Mobile App) for status updates.

## 🔧 Troubleshooting

### Virtual device isn't syncing
Ensure the "Virtual Garage Door Device" and "Virtual Garage Door Device sensor" are pointing to the *same* physical/virtual device, just selecting different capabilities (Garage Door Control vs. Contact Sensor).

## 🙏 Credits & History

- **Original Concept & Code**: LGKahn (`lgkapps`)
- **Hubitat Port & Virtual Garage Door Updates**: LGKahn (v4.x)
- **Konnected Pushable Button Optimization, HSM Integration, & Parent/Child Architecture**: SMJ (2026)

*Licensed under the Apache License, Version 2.0. See the original code headers for full license details.*
