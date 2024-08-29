# Windows 11 Stuff

---

## USB-C Hubs

* When connecting a USB-C hub to windows it reports an unknown device, and (Port
  Reset Failure) it may be due to the hub being powered. Disconnect the hub and
  uninstall the device driver (right-click the Windows menu and go to Device
  Manager). Then reconnect the hub without the power and Windows might now
  recognise it correctly, then you can reconnect the power.

## [MSys2](https://www.msys2.org/)

* For VCV Rack development, you should use the MinGW64 environment
  * MinGW64 is `x86_64` arch, uses `gcc` and includes `msvcrt` & `libstdc++`
    * See [MSys2 Environments](https://www.msys2.org/docs/environments/)

* Install MSys2 using Scoop
  * `scoop bucket add main`
  * `scoop install main/msys2`

* To run MSys2.mingw64 in Windows Terminal:
  * Under `Settings`, click `Add a new profile`
    * Set the `Command line` to `C:\Users\dant\scoop\apps\msys2\2024-05-07\msys2_shell.cmd  -defterm -here -no-start -mingw64 -shell bash`
    * Set the `Starting directory` to `%USERPROFILE%`
