# Steam Deck

---

## Desktop Mode

* Account does not have a password by default, you need to set a password before
  you can use `sudo`
  * To set a password use the command `passwd` in a terminal

* The filesystem is readonly by default, you need to unlock it before you can
  use `pacman` to install or update packages
  * To unlock the filesystem use the command `sudo steamos-readonly disable`

* Before you can use `pacman` it needs to have a keyring created and populated
  * To create the keyring use the command `sudo pacman-key --init`
  * To populate the keyring use the command `sudo pacman-key --populate`

* It is wise to keep the package manager `pacman` updated
  * To update use the command `pacman -Suy`

### Wacom One (Gen 1)

* The device is plug-and-play as long as it is powered, best setup is a powered
  USB-C hub that has an HDMI port, plug this into the Steam Deck, and then plug
  the Wacom USB-A and HDMI into the hub.

* If you want to extend the screen instead of mirroring the main display on the
  Wacom, you need to install the Wacom tools
  * Use the command `sudo pacman -S kcm-wacomtablet libwacom`
  * You might need to restart the Steam Deck before the device is recognised
  * There now should be a program in the menu called `Graphic Tablet - Wacom
    Tablet Settings` open this to set the `Map to screen` key combo
  * Map the tablet to the Wacom screen (probably screen 2) and then the pen
    should correctly position the cursor only on that screen
