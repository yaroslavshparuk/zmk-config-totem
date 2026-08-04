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

## PROSPECTOR DONGLE

This config can be built two ways. `build.yaml` produces both sets of firmware, so
pick one set and stick to it.

**Dongle mode** — a [Prospector](https://github.com/carrefinho/prospector) (XIAO
nRF52840 + ST7789 screen) is the central; both halves are peripherals. The screen
is driven by [YADS](https://github.com/janpfischer/zmk-dongle-screen), pulled in
via `config/west.yml`.

| File | Flash to |
| --- | --- |
| `totem-prospector-dongle` | the Prospector dongle |
| `totem-left-peripheral` | left half |
| `totem-right` | right half |

**Dongle-less mode** — the left half is the central, as before.

| File | Flash to |
| --- | --- |
| `totem-left-standalone` | left half |
| `totem-right` | right half |

`totem-settings-reset` erases all pairings from any of the three XIAOs.

### Flashing and pairing order

The battery widget assigns its indicators in pairing order, so the sequence
matters:

0. First time only, when converting an existing keyboard: flash
   `totem-settings-reset` to the dongle and to **both** halves. The left half
   still holds its old central-role bonds, which stop it pairing as a
   peripheral.
1. Switch both halves off.
2. Flash the dongle, then unplug it.
3. Flash the left half, then the right half.
4. Plug the dongle back in.
5. Switch on the **left** half, wait for its battery indicator to appear.
6. Switch on the **right** half.

If the indicators end up swapped, flash `totem-settings-reset` to the dongle and
repeat from step 2.

### Screen controls

YADS listens for three keycodes, which are not bound in the keymap by default:

- `F22` — toggle the screen off/on
- `F23` / `F24` — brightness down/up

Brightness otherwise tracks the Prospector's ambient light sensor. Set
`CONFIG_DONGLE_SCREEN_AMBIENT_LIGHT=n` in
`config/boards/shields/totem/totem_dongle.conf` if your dongle has no sensor
fitted. All other screen options are documented in the
[YADS readme](https://github.com/janpfischer/zmk-dongle-screen#configuration-options).

### Zephyr 4.1 note

ZMK `main` runs Zephyr 4.1, which YADS only supports on its `upgrade-4.1`
branch — that's what `config/west.yml` pins, and it's why the board is spelled
`xiao_ble//zmk` in `build.yaml`. Don't move the module to `main` until
[issue #29](https://github.com/janpfischer/zmk-dongle-screen/issues/29) closes.
