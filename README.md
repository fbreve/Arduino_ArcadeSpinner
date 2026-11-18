# Arduino_ArcadeSpinner

## About this fork

This repository is a fork of [willoucom/Arduino_ArcadeSpinner](https://github.com/willoucom/Arduino_ArcadeSpinner), an Arduino Pro Micro USB arcade-spinner project. The **fork-specific firmware change** is the pin mapping in [`Arduino_ArcadeSpinner.ino`](Arduino_ArcadeSpinner.ino), adapted in September 2023 for a commercial spinner. The upstream software, credit notices, and 3D-model files are retained.

- **Hardware reference:** [video of the commercial spinner used for this fork](https://www.youtube.com/watch?v=cjLsDffC8pk). This replaces a discontinued/unavailable sales link; it is a visual reference, **not** a firmware compatibility test.
- **Historical Windows observation (2023):** the buttons and rotary encoder worked in all four modes with this adapted firmware.
- **Known MiSTer FPGA limitation (2023):** the four buttons were recognized, but spinner rotation did not produce usable wheel events in any of the four modes on the maintainer's MiSTer. See the [upstream issue #4](https://github.com/willoucom/Arduino_ArcadeSpinner/issues/4). No fix or current-hardware retest is claimed.

**Important:** the original project's MiSTer gameplay videos, supported-game list, enclosure photos, and assembly instructions below are **upstream examples**, not proof that this particular fork/hardware combination works on MiSTer.

## Original project overview (from upstream)

The original project recreates arcade spinners, particularly for games such as Arkanoid, using readily available components.

![Img!](img/pic.jpg "Arcade Spinner")

## In action (upstream demonstrations)

These videos show the original project's controller. They do not demonstrate MiSTer support for the commercial spinner adapted in this fork.

### Arkanoid with rotary controller
[![IMAGE ALT TEXT](http://img.youtube.com/vi/QnDsVMLP6zM/0.jpg)](http://www.youtube.com/watch?v=QnDsVMLP6zM "Arkanoid with rotary controller on MisterFPGA")

### Puzz'Loop 2 with rotary controller
[![IMAGE ALT TEXT](http://img.youtube.com/vi/TzKd8ytODUM/0.jpg)](http://www.youtube.com/watch?v=TzKd8ytODUM "Puzz'Loop 2 with rotary controller on MisterFPGA")

### Eco Fighter / Ultimate Ecology with rotary controller
[![IMAGE ALT TEXT](http://img.youtube.com/vi/sb1mO-uq1ag/0.jpg)](http://www.youtube.com/watch?v=sb1mO-uq1ag "Eco Fighter / Ultimate Ecology with rotary controller on MisterFPGA")



## Usage

These are the firmware's available modes. Availability of a mode does not establish working spinner input on MiSTer for this fork.

By default the device operate as a 4 button controller with Spinner/Paddle attached

You can activate an additional behavior by holding a button when you plug in the device : 

- Press Button0 (Big Left button) to enable MouseX Emulation
- Press Button1 (Big Right button) to enable Paddle Emulation
- Press Button2 (Small Left button) to switch device to mr.Spinner (enabling composite device on mister fpga)
 - In mr.Spinner mode, each button press is redirected to button 1

## MiSTer FPGA — upstream compatibility examples

**Upstream-only list:** the games below were reported as working with the original project's hardware/firmware. They have **not** been validated with this fork's commercial spinner. For the fork's 2023 test outcome, see [About this fork](#about-this-fork).

### Working (upstream)

| Game                          | Note|
|---                            |--- | 
| Arkanoid (World)              | |
| Block Gal                     | |
| Breakout (TTL)                | Enable spinner in controls|
| Crater Raider                 | |
| Dark Planet                   | |
| Demolition Derby              | |
| Discs of Tron                 | |
| Eco Fighters                  | Use mr.Spinner mode |
| Forgotten Worlds              | Use mr.Spinner mode |
| Kick                          | |
| Kozmik Krooz'r                | |
| Midnight Resistance           | Use mr.Spinner mode |
| Moonwar                       | |
| Pong                          | Use Paddle emulation and set your player to Paddle |
| Pop'n Bounce (NeoGeo)         | Enable Paddle in Dip settings (Soft dip->Pop'n bounce->Controller = paddle) |
| Puzz Loop 2                   | Enable Paddle in Service mode (System -> Operation) |
| Tron                          | |
| Two Tigers (tron conversion)  | |

### Working with workaround (upstream)
| Game                  | Note|
|---                    |---| 
| Block block (world)   | Use alternatives/block block (world 910910), must use Mouse Emulation |
| Super Breakout        | Enable paddle in controls, must use paddle emulation |

## Mame

You can use the controller with mame and derivative by using the controller in Mouse Emulation mode.

> Note: You can make mouse emulation the default by changing `bool mouse_emu = 0;` to `bool mouse_emu = 1;` in the source. This changes firmware behavior and is not a verified MiSTer workaround for this fork.

You need to change the `dial device assignment` option in the mame settings either in mame.ini or using the GUI.

### Edit mame.ini 
```
dial_device               mouse
```
### Using MAME GUI

![Img!](img/mame.jpg "MAME")

## Build yourself

### Parts 

- Rotary encoder (Model: C38S6G5-600B-G24N)
- 40mm Knob with internal diameter of 6mm 
- Arduino Pro Micro
- 2 24mm button
- 2 30mm button
- Enclosure 3D printed or any box

#### 3D printed enclosure

You can use the provided .stl files inside the [3D Files](./3D%20Files/) folder.

I suggest to print the enclosure with the flat surface on top and bottom laying on the printing bed.

If you have some skills in 3D modeling, you can also use the .f3d archive which is usable with Autodesk Fusion 360.

#### Prepare the rotary encoder

![Img!](img/encoder.jpg "rotary encoder")

1. Remove the metal enclosure and extract the mechanism.
2. Unsolder all wires.
3. Solder fresh wires on GND, VCC, A and B pads.
4. Recycle the original metal case and wire for another project.

### Arduino pin mapping for this fork

![Upstream Arduino Pro Micro wiring illustration; check the fork-specific table below](img/arduino.jpg "Upstream wiring reference")

**Use the table below for the current fork firmware**, rather than the original project's pinout in the illustration. These are Arduino **digital pin numbers** from [`Arduino_ArcadeSpinner.ino`](Arduino_ArcadeSpinner.ino); verify the physical pin labels and wiring on your exact Pro Micro-compatible board before connecting anything.

| Signal | Arduino digital pin in this fork | Firmware definition |
| --- | --- | --- |
| Encoder A | D3 | `pinA = 3` |
| Encoder B | D2 | `pinB = 2` |
| Large left button | D4 | `Button0 = 4` |
| Large right button | D5 | `Button1 = 5` |
| Small left / Select / Coin | D10 | `Button2 = 10` |
| Small right / Start | D15 | `Button3 = 15` |

The buttons are configured with `INPUT_PULLUP` and are active when connected to **GND**. The encoder also needs appropriate GND and supply connections; confirm voltage and pinout against the actual board and encoder specifications.

The upstream assembly guide recommends bridging **J1** to supply 5 V to *its* encoder. **Do not automatically copy that modification** to a different commercial spinner or board: confirm the acceptable voltage and the purpose of J1 first. No physical-wiring validation was performed in this documentation update.

### Flash your Arduino

1. Install Arduino IDE 2.x
2. Open Arduino_ArcadeSpinner.ino
    1. (Optional) Change options in the configuration section.
3. Connect your Arduino pro micro to your computer
4. Select Arduino Leonardo with the correct com port
5. Flash the controller

## Technical informations
In the mister source code there is two special cases

If the device name start by MiSTer-S1 the device is considered as a mr.Spinner

```
// mr.Spinner
// 0x120  - Button
// Axis 7 - EV_REL is spinner
// Axis 8 - EV_ABS is Paddle
// Overlays on other existing gamepads
if (strstr(uniq, "MiSTer-S1")) input[n].quirk = QUIRK_PDSP;
if (strstr(input[n].name, "MiSTer-S1")) input[n].quirk = QUIRK_PDSP;
```

If the device name start by MiSTer-A1 the device is considered as an arcade stick with a spinner and/or paddle
```
// Arcade with spinner and/or paddle:
// Axis 7 - EV_REL is spinner
// Axis 8 - EV_ABS is Paddle
// Includes other buttons and axes, works as a full featured gamepad.
if (strstr(uniq, "MiSTer-A1")) input[n].quirk = QUIRK_PDSP_ARCADE;
if (strstr(input[n].name, "MiSTer-A1")) input[n].quirk = QUIRK_PDSP_ARCADE;
```

## Rotary encoder

The rotary encoder uses a magnet to detect rotation, it translates the orientation into pulses which are then interpreted by the arduino controller.

![Img!](img/magnet.jpg "Magnet view")
![Img!](img/magnet2.jpg "Magnet controller")


## License and attribution

This fork retains the original authors' notices and the [GNU GPL-3.0 license](LICENSE). The Arduino firmware originates from [Wilfried Jeanniard's project](https://github.com/willoucom/Arduino_ArcadeSpinner), which credits earlier work by Alexey Melnikov and Mikael Norrgård in its source headers. The included CAD/STL designs and images come from the upstream project; this fork does not claim authorship of those materials.

The 2023 adaptation changed the firmware pin mapping. The November 2026 README maintenance changes **documentation only**: it does not modify the Arduino code, recompile the firmware, or establish new Windows/MiSTer compatibility results.
