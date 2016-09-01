# lab1
Breadboarding lab resources and assignment spec.

## Setup
### Toolchain installation
_Set up required software._

1. Install [GCC-ARM](https://launchpad.net/gcc-arm-embedded) to build the embedded firmware.
  - On Debian-based systems (including Ubuntu), a [PPA](https://launchpad.net/~team-gcc-arm-embedded/+archive/ubuntu/ppa) is available. Note: the one included by default in the system package manager may not work.
  - On Windows and Mac, [download and run this installer](https://launchpad.net/gcc-arm-embedded).
1. Install [SCons](http://scons.org/), the build system.
  - On Debian-based systems (including Ubuntu), this is available as a package.
    ```
    sudo apt-get install scons
    ```
  - On Windows, [download and run this installer](http://scons.org/pages/download.html). You will need to install [Python 2.7, 32-bit]](https://www.python.org/downloads/) if you do not have it already (as of SCons 2.5.0, there is no Python3 support yet and the installer will not detect 64-bit Python versions).
1. Install [OpenOCD](http://openocd.org/), a program that interfaces with the on-board debugger.
  - *IMPORTATNT*: Current releases of OpenOCD do not support the Nucleo L432KC. We've added support and [built binaries which you can download and install](https://github.com/cs194-126/openocd/releases). For the crazy or paranoid among us, [you can see the code we changed](https://github.com/cs194-126/openocd/commit/f2501b08a11931191048a76883e0541f9cb1d079) and build it from source if you wish.

### Cloning this repository
_Get the code._

#### Command-line option
1. Ensure command-line git is installed.
  - On Debian-based systems (including Ubuntu), this is available as a package.
    ```
    sudo apt-get install git
    ```
  - On Windows, [download and run this installer](https://git-scm.com/download/win).
1. Clone (download a copy of) the repository:
  ```
  git clone --recursive git@github.com:cs194-126/lab1.git
  ```
1. To pull new updates your local repository from GitHub:
  ```
  git pull
  git submodule update --init --recursive
  ```
  Submodules (essentially other repositories that are cloned and tracked as part of this repository) are not automatically updated as part of pull, which is why you must execute the submodule update separately.   

#### GUI option
_Available for Windows and Mac only._
1. Download and install [GitHub Desktop](https://desktop.github.com/).
1. Clone [this repository](https://github.com/cs194-126/lab1) to desktop using the "Clone or download" button on the web interface. It should automatically launch GitHub Desktop.
1. In the GitHub Desktop interface, you can sync the repository (push new changes to GitHub if you have the appropriate permissiosn, as well as pull updates from GitHub) using the "Sync" button.

### Sanity-checking
_Check that the compiled firmware works._

### Installing Eclipse
_This section is optional, for people want to work with an IDE and GUI debug tools. This is recommended for beginners, as it provides a low learning curve and integrated way to code, program, and debug firmware. For the masochists among us that love vim/emacs and command-line GDB, feel free to skip this section._

## Assignment
### Hardware
#### Breadboard connection refresher

### Software 

## Checkoff Spec
_How to get credit for what you did!_ 

1. Show us your circuit with firmware running. A LED should be pulsing, linearly varying in perceived intensity from 0-100% brightness then ramping back down, with a full cycle taking approximately 2 seconds.
2. Press and hold the button. The LED intensity should freeze at the point where the button is pressed. After the button is released, the LED should continue pulsing as described in the previous section. 
3. We may ask you to show your code as well as explain parts of it. Make sure you understand everything you write!
