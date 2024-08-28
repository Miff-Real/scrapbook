# VCV Rack
## Development
### WSL environment

Windows Subsystem for Linux, gives you a local linux terminal environment that is useful for certain
dev tasks and is easier to work with than PowerShell or `cmd`

* Optional: install [Windows Terminal](https://aka.ms/terminal)
  * [Alternatives](https://www.puttygen.com/windows-terminal-emulators)
* Install [WSL 2](https://learn.microsoft.com/en-us/windows/wsl/install)
  * I prefer Debian over Ubuntu because it aligns better with my other dev projects
  * Optional: Install the Debian distribution `wsl --install -d Debian`
  * Optional: If you have both Ubuntu and Debian install, set Debian as default `wsl -s Debian`
* Map a network drive in file explorer so that you can access the WSL files directly in Windows
  * Open `This PC` in file explorer, right-click and select `Add a network location`
  * Use the address `\\wsl.localhost\ubuntu` or `\\wsl.localhost\debian`
  * Access WSL files from the network drive, eg. edit your `.bashrc` in a windows text editor

* Run WSL, setup your terminal how you like it, and update your `.bashrc` file
* In WSL:
  * Update package manager
    * `sudo apt-get update`
  * Install curl
    * `sudo apt-get install curl`
  * Optional: install [Starship](https://starship.rs/) prompt, don't forget the [NerdFont](https://www.nerdfonts.com/)
    * `curl -sS https://starship.rs/install.sh | sh`
    * add `eval "$(starship init bash)"` to `.bashrc`

### Linux build in WSL

Using WSL we can build a Linux version of a plugin

* In Windows
  * Create a `rack-dev` directory somewhere, for example `C:\Users\dant\rack-dev`
  * Download the Linux [Rack SDK](https://vcvrack.com/manual/Building#Building-Rack-plugins) and extract it into this directory
  * Optional: install [GitHub Desktop](https://github.com/apps/desktop)
  * Create or clone a plugin git repository in the `rack-dev` directory
    * Note: this git repo can remain local, you do not have to push your code to github.com until/unless you want to have the plugin added to the VCV Rack plugin library
    * If it is a new repo, create and commit the minimum files to build the plugin
    * You can use whatever text editor or IDE you want on Windows for this, I use [VSC](https://code.visualstudio.com/)

* In WSL
  * Install Ubuntu 16.04+ packages listed in [the docs](https://vcvrack.com/manual/Building)
  * Verify you can access the your `rack-dev` directory, it will have the path `/mnt/c/Users/dant/rack-dev`
  * Add the Rack SDK path in `.bashrc`
    * `export RACK_DIR="/mnt/c/Users/dant/rack-dev/Rack-SDK"`
  * `cd /mnt/c/Users/dant/rack-dev/<plugin>`
  * `make dist`
    * verify your linux build resulted in `/mnt/c/Users/dant/rack-dev/<plugin>/dist/<plugin>-<version>-lin-x64.vcvplugin`

### Build using the `rack-plugin-toolchain` and Docker

Should be able to build a docker image that can cross compile a plugin for all platforms

* [install Docker Desktop](https://docs.docker.com/desktop/install/windows-install/) using WSL
  * Annoyingly you have to restart your machine
  * login with GitHub
  * Test that Docker is working correctly by running the tutorial image `docker run -d -p 80:80 docker/getting-started`
  * If everything is good, you can stop and delete the tutorial image

* Clone [@qno](https://community.vcvrack.com/u/qno/)'s [fork of the `rack-plugin-toolchain`](https://github.com/qno/rack-plugin-toolchain) repo into the `rack-dev` directory
  * Need to sort out getting the `MacOSX11.1.sdk.tar.xz` to make MacOS builds work

* In WSL
  * Go to the toolchain directory `cd /mnt/c/Users/dant/rack-dev/rack-plugin-toolchain`
  * Build the Docker image `make docker-build`
  * You can view the build details in the Docker Desktop app
  * When the Rack toolchain needs to be updated (and after the repo has been updated to a new version):
    * `make rack-sdk-clean` then `make rack-sdk-all`
  * To build platform specific versions of a plugin:
    * `make docker-plugin-build-win-x64 PLUGIN_DIR=/mnt/c/Users/dant/rack-dev/DanTModules/`
    * `make docker-plugin-build-lin-x64 PLUGIN_DIR=/mnt/c/Users/dant/rack-dev/DanTModules/`
    * `make docker-plugin-build-mac-x64 PLUGIN_DIR=/mnt/c/Users/dant/rack-dev/DanTModules/`
    * `make docker-plugin-build-mac-arm64 PLUGIN_DIR=/mnt/c/Users/dant/rack-dev/DanTModules/`
  * To build all platforms:
    * `make docker-plugin-build PLUGIN_DIR=/mnt/c/Users/dant/rack-dev/DanTModules/`
  * To check if there are any issue with your plugin code:
    * `make docker-plugin-analyze PLUGIN_DIR=/mnt/c/Users/dant/rack-dev/DanTModules/`

### Local GitHub Actions using Act

Should allow you to build the plugin for all platforms locally using a GitHub workflow

* [install Act](https://nektosact.com/installation/index.html)
  * Optional: use [Scoop](https://scoop.sh/), I already have this installed for other Windows deps
  * Use PowerShell to verify your Act install works `Get-Command act`

To test building the plugin, in PowerShell.

Currently does not work, [due to](https://github.com/nektos/act/issues/973):

* `container: is not yet supported (as in, it will run in a container, but you won't get node and other stuff that GHA mounts into that container for it to work properly)`
* `OCI runtime exec failed: exec failed: unable to start container process: exec: "node": executable file not found in $PATH: unknown`


  * `cd C:\Users\dant\rack-dev\<plugin>\`
  * Show what jobs are available `act --list`
  * `act push` | `act -j build` | `act push --matrix platform:win-x64` | `act push --matrix platform:win-x64 --action-offline-mode`
    * The first time you run this Docker Desktop will require a size selection
      * `Medium size image: ~500MB, includes only necessary tools to bootstrap actions and aims to be compatible with most actions`

 * Opt out of the GitHub runner host env for your local platform, ie build windows on windows directly
  * `act push --matrix platform:win-x64 -P windows-latest=-self-hosted`
