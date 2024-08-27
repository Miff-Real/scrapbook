# VCV Rack
## Development
### WSL environment

* [Windows Terminal](https://aka.ms/terminal)
* Install [WSL 2](https://learn.microsoft.com/en-us/windows/wsl/install)
  * Install the Debian distribution `wsl --install -d Debian`
  * If you have both Ubuntu and Debian install, set Debian as default `wsl -s Debian`
* Map a network drive in file explorer so that you can access the WSL files directly in Windows
  * Open `This PC` in file explorer, right-click and select `Add a network location`
  * Use the address `\\wsl.localhost\debian`
* Update package manager
  * `sudo apt-get update`
* Install curl
  * `sudo apt-get install curl`
* Optional: install starship prompt
  * `curl -sS https://starship.rs/install.sh | sh`
  * add `eval "$(starship init bash)"` to `.bashrc`

### VCV build

* Install Ubuntu 16.04+ packages listed in [the docs](https://vcvrack.com/manual/Building)
* Download the Rack-SDK (for Linux) and extract to your home directory
* Add the Rack SDK path in `.bashrc`
  * `export RACK_DIR="/home/dant/Rack-SDK"`
* clone your plugin repo and try to build it (for Linux)

### Local GitHub Actions using Act

Allows you to build all platforms locally

* install docker desktop using WSL
* install Act using Scoop
