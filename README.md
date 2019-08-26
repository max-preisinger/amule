--- **This Project is not maintained** ---

# Description

This is a fork of the aMule project. It implements a country blacklist to block (server and / or client) connections to specified countries.

# Usage

Create a file in the aMule config directory (usually `~/.aMule`) called `countryfilter.txt`. Write the [country code](https://dev.maxmind.com/geoip/legacy/codes/iso3166/) of the country you want to block **in upper letters** in this file. Use a new line for every country.

To activate the blacklist, go to `Preferences`->`Security` and check the checkbox `Activate Country Blacklist in ~/.aMule/countryfilter.txt`. If the checkbox `Filter clients` is checked, the country blacklist is applied to all clients. If the checkbox `Filter servers` is checked, the country blacklist is applied to all servers. If neither of those checkboxes are checked, the blacklist will have no effect. 

You can edit the `countryfilter.txt` file while the program is running and apply the changes with clicking the button `Preferences`->`Security`->`Reload List`.

# Compilation and Installation

1. This project needs the IP2Country module. Therefore you need to install the GeoIP library. On Ubuntu do this with: `sudo apt install libgeoip-dev`
1. Clone or download this repo
2. Go into the root of the repo
3. `mkdir build`
4. `cd build`
5. `../configure --enable-geoip`
6. `make`
7. Create the folder `~\.aMule` and move the file `INSTALL-UTILS/GeoIP.dat` into this folder. This is necessary because the GeoIP library is discontinued, and the database cannot be downloaded any more from the official source. The aMule team did not solve this issue yet. You can get this legacy database also from [here](https://packages.debian.org/jessie/all/geoip-database/download).
8. Start the program `build/src/amule` (but better read the known buggs before, to prevent system freeze)

# Known Bugs

* Even before modifying this fork, the original aMule project has some serious memory leak issues. After a while, the RAM will be running full. To prevent system freeze, run `ulimit -v 900000` in the terminal window, from where you start the program. This will limit the RAM usage of this terminal window to 900MB, and thus the program crashes before the whole system freezes.
* According to [this source](https://github.com/amule-project/amule/issues/2), wxDesigner is neither free nor maintained any more. Also, the [(probably?) official Website of wxDesigner](http://www.wxdesigner-software.de/) is now used as a scam shop. Therefore I had to hack the checkbox into the `muuli_wdr.cpp` file, which one should not manually edit and which is therefore not very sustainable.
* The GeoIP library is discontinued. The workaround is described in the installation procedure. A better way would be to switch to the [GeoIP2 library](https://dev.maxmind.com/geoip/geoip2/downloadable/) from the same provider.
* The compiled program is quite likely to crash with segmentation faults, which are hard to reproduce. It seems to be very likely that an empty server list is one reason for a segmentation fault. Therefore, as a workaround, clear the file `countryfilter.txt`, before starting the program. If all servers are filtered out by the country blacklist, the program will crash on startup. (If the program crashes on startup, and the file is empty, just restart the program. Usually with the next try it will start.) When the program is started, add some countries in the file, and click the `Reload List` button to apply the changes. But don't block all the servers, otherwise the program will crash.