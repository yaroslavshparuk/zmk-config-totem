<picture>
  <source media="(prefers-color-scheme: dark)" srcset="/docs/images/TOTEM_logo_dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="/docs/images/TOTEM_logo_bright.svg">
  <img alt="TOTEM logo font" src="/docs/images/TOTEM_logo_bright.svg">
</picture>

# ZMK CONFIG FOR THE TOTEM SPLIT KEYBOARD

[Here](https://github.com/GEIGEIGEIST/totem) you can find the hardware files and build guide.\
[Here](https://github.com/GEIGEIGEIST/qmk-config-totem) you can find the QMK config for the TOTEM.

TOTEM is a 38 key column-staggered split keyboard running [ZMK](https://zmk.dev/) or [QMK](https://docs.qmk.fm/). It's meant to be used with a SEEED XIAO BLE or RP2040.


![TOTEM layout](/docs/images/TOTEM_layout.svg)



## HOW TO USE

- fork this repo
- `git clone` your repo, to create a local copy on your PC (you can use the [command line](https://www.atlassian.com/git/tutorials) or [github desktop](https://desktop.github.com/))
- adjust the totem.keymap file (find all the keycodes on [the zmk docs pages](https://zmk.dev/docs/codes/))
- `git push` your repo to your fork
- on the GitHub page of your fork navigate to "Actions"
- scroll down and unzip the `firmware.zip` archive that contains the latest firmware
- connect the left half of the TOTEM to your PC, press reset twice
- the keyboard should now appear as a mass storage device
- drag'n'drop the `totem_left-xiao_ble-zmk.uf2` file from the archive onto the storage device
- repeat this process with the right half and the `totem_right-xiao_ble-zmk.uf2` file.

## DONGLE (NICE!NANO + OLED)

The repo also builds firmware for a dedicated central dongle (nice!nano) with a 1.3" 128x64 SH1106 OLED, driven by [englmaxi/zmk-dongle-display](https://github.com/englmaxi/zmk-dongle-display) (layer, modifiers, HID indicators, peripheral battery, bongo cat). The OLED is wired SDA = P0.20 / SCL = P0.17, same as the Skeletyl dongle.

Firmware in the `firmware.zip` archive:

| File | Target |
| --- | --- |
| `totem_dongle-nice_nano-zmk.uf2` | dongle without display |
| `totem_dongle_oled_nice_nano-nice_nano-zmk.uf2` | dongle with OLED |
| `totem_left_peripheral-xiao_ble-zmk.uf2` | left half as peripheral (dongle mode) |
| `totem_left-xiao_ble-zmk.uf2` / `totem_right-xiao_ble-zmk.uf2` | standalone Bluetooth mode (left is central) |

### Dongle mode

1. flash `settings_reset` onto the left half, right half and dongle (clears old bonding info — required when switching roles)
2. flash `totem_left_peripheral` onto the left half, `totem_right` onto the right half, and the dongle uf2 onto the dongle
3. plug the dongle in via USB; both halves connect to it as peripherals

### Standalone Bluetooth mode (no dongle)

1. flash `settings_reset` onto both halves
2. flash the default `totem_left` (central) and `totem_right` (peripheral) firmware
3. the left half connects to the host over Bluetooth as before
