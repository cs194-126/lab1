# lab1
Breadboarding, blinky LED "hello, world", and pulsing LED.

## Setup
### Toolchain installation
_Set up required software._

0. Install [GCC-ARM](https://launchpad.net/gcc-arm-embedded) to build the embedded firmware.
  - On Debian-based systems (including Ubuntu), a [PPA and the instructions for setting it up are available](https://launchpad.net/~team-gcc-arm-embedded/+archive/ubuntu/ppa). Note: the one included by default in the system package manager may not work.
  - On Windows and Mac, [download and run this installer](https://launchpad.net/gcc-arm-embedded).
0. Install [SCons](http://scons.org/), the build system.
  - On Debian-based systems (including Ubuntu), this is available as a package:
  
    ```
    sudo apt-get install scons
    ```
    
  - On Windows, [download and run this installer](http://scons.org/pages/download.html). You will need to install [Python 2.7, 32-bit]](https://www.python.org/downloads/) if you do not have it already (as of SCons 2.5.0, there is no Python3 support yet and the installer will not detect 64-bit Python versions).
0. Install [OpenOCD](http://openocd.org/), a program that interfaces with the on-board debugger.
  - **IMPORTANT**: The latest official OpenOCD release does not support the Nucleo L432KC. We've added support and [built binaries which you can download and install](https://github.com/cs194-126/openocd/releases). For the paranoid or truly crazy among us, [you can see the code we changed](https://github.com/cs194-126/openocd/commit/f2501b08a11931191048a76883e0541f9cb1d079) and build it from source if you wish.

_Note: for this lab, we don't check what environment you use, so this checkoff can be completed with [mbed online compiler](https://developer.mbed.org/compiler/) if you so choose. However, the system described by the installation instructions above will be required when the JITPCB labs roll around (since JITPCB does not have integration with the mbed online compiler), so you might as well get this set up now._

### Cloning this repository
_Get the code._

#### Command-line option
0. Ensure command-line git is installed.
  - On Debian-based systems (including Ubuntu), this is available as a package.
  
    ```
    sudo apt-get install git
    ```
    
  - On Windows, [download and run this installer](https://git-scm.com/download/win).
0. Clone (download a copy of) the repository:

  ```
  git clone --recursive git@github.com:cs194-126/lab1.git
  ```
  
0. To pull new updates your local repository from GitHub:

  ```
  git pull
  git submodule update --init --recursive
  ```
  
  Submodules (essentially other repositories that are cloned and tracked as part of this repository) are not automatically updated as part of pull, which is why you must execute the submodule update separately.   

#### GUI option
_GitHub Desktop is only avialable for Windows and Mac._

0. Download and install [GitHub Desktop](https://desktop.github.com/).
0. Clone [this repository](https://github.com/cs194-126/lab1) to desktop using the "Clone or download" button on the web interface. It should automatically launch GitHub Desktop.
0. In the GitHub Desktop interface, you can sync the repository (push new changes to GitHub if you have the appropriate permissiosn, as well as pull updates from GitHub) using the "Sync" button.

### Build & sanity-check
_Check that the compiled firmware works._

0. In a command prompt inside the repository, run:

  ```
  scons
  ```

  to run the build system, which compiles both the mbed library in `mbed/` and the top-level program in `src/`. This should produce a compiled, flashable binary, `build/program.bin`.
0. Connect your microcontroller board to your computer via USB. You should see a flash drive named `NUCLEO_L432KC` show up.
0. Copy `build/program.bin` into the `NUCLEO_L432KC` flash drive to write the program to the microcontroller. This may take a few seconds.
0. If it worked, the onboard user-controlled LED (the green one next to the reset button) shoud blink at about 1Hz. 

### Installing Eclipse
_This section is optional, for people who want to work with an IDE and GUI debug tools. This is recommended for beginners, as it provides a low learning curve and integrated way to code, program, and debug firmware. For the masochists among us that love vim/emacs and command-line GDB, feel free to skip this section._

#### Initial setup
0. [Download Eclipse](https://www.eclipse.org/downloads/). Eclipse IDE for C/C++ developers is a good option.
0. Install some Eclipse plugins:
  - (menu) > Help > Install New Software..., then enter the update site URL in the "Work with..." field.
  - If some components cannot be installed, you may need to update your version of Eclipse.
  - Install [SConsolidator](http://www.sconsolidator.com/) (update site: <http://www.sconsolidator.com/update>), which allows some integration of SCons scripts.
  - Install [GNU ARM on Eclipse](https://gnuarmeclipse.github.io) (update site: <http://gnuarmeclipse.sourceforge.net/updates>), which provides Eclipse program and debug integration.
0. Define where the OpenOCD binary can be found:
  - (menu) > Preferences > Run/Debug > OpenOCD, set the Folder to the folder where the OpenOCD binary is located.

#### Project configuration
0. Add this repository as a Eclipse project:
  - (menu) > New > Project > New SCons project from existing source. Under Existing Code Location, choose the folder where you checked out this repository.
0. Build the project.
  - In the Project Explorer, right-click the newly created project and click Build.
  - If the build succeeded, this should create all the `.elf` files (compiled firmware) in the `build/` directory.
  - PROTIP: you can also start a build with Ctrl+B.
0. Set up a debug configuration. Right-click the `.elf` file and select Debug As > Debug Configurations...
  0. From the list on the left side of the new window, right click GDB OpenOCD Debugging and select New.
  0. Under the Main tab:
    - Give the configuration a name.
    - Ensure the Project field matches the project name.
    - Set the C/C++ Application to the path to the `.elf` file, like `build/program.elf`.
    - If you want to auto-build before launching, set it in Build (if required) before launching, either for this debug target only, or by modifying the workspace-wide setting.
  0. Under the Debugging tab:
    - Leave the default OpenOCD Setup > Executable configuration of `${openocd_path}/${openocd_executable}`.
    - Set the GDB Client Setup > Executable to `arm-none-eabi-gdb`.
    - Set the Config Options to include flags for interface / target configuration (`-f`), and startup commands (`-c`):

     ```
     -f interface/st-link-v2-1.cfg
     -f target/stm32l4x.cfg
     -c init
     -c "reset halt"
     ```

  0. Under the Startup tab:
    - In Run/Restart Commands, if you want the target to start running immediately after flashing completes, disable the Set breakpoint at: option.
  0. Under the Common tab:
    - If you want to launch this target from the quickbar, check the options in Display in favorites menu.
0. Try launching the debug target. This will flash the microcontroller and start it.
  - If all goes well, you should be able to pause the target (and Eclipse should bring up the next line of code the microcontroller will execute). The normal debugging tools are available: step-into, step-over, step-out, breakpoints, register view, memory view, and more.
  - PROTIP: you can launch the last debug target with Ctrl+F11.

## Assignment
In this assignment, you will build, on a breadboard, a ciruit that pulses a LED with a button that freezes the pulsing.

### Hardware
#### Breadboard connection refresher
Recall that breadboards are wired internally as such:

![Breadboard internals](/docs/breadboard-internals.svg.png?raw=true)

Caveat emptor: some breadboards divide up the power rails into quadrants, adding a break in the power rails among the center:

![Breadboard with broken rails](/docs/breadboard-internals-broke.svg.png?raw=true)

Others have the power rails connected all the way across the length of the breadboard. 

You will want to check if your breadboard is wired like this.

#### LED Circuit
You will need a LED connected to a PWM-capable output. Note that the pin the onboard LED is connected is NOT PWM-capable, so you will need to wire up an external circuit. The typical circuit for an indicator LED is a LED in series with a resistor:

![LED circuit](/docs/led.png?raw=true)

(this example is wired up such that the LED turns on when the voltage on the pin is high).

[The pinning guide](https://developer.mbed.org/platforms/ST-Nucleo-L432KC/) may be helpful.

#### Switch Circuit

You will also need a switch connected to any IO. The general circuit block for a switch is wiring up the switch between the IO pin and ground (such that the switch shorts the IO to ground when pressed), and a weak pull-up resistor between the IO pin and high (such that the IO is pulled high when the switch is not pressed):

![Switch circuit](/docs/switch.png?raw=true)

You can omit the resistor because the chip has built-in configurable pull-ups on all IOs (so just wire up the switch between IO and ground).

![Switch circuit](/docs/switch-nores.png?raw=true)

### Software
In the included example code, you have `DigitalOut led(LED1)`, which creates an object of class `DigitalOut` (with one argument, PinName `LED1`) as `led`. For this assignment, the following other classes may be helpful:
 - `DigitalIn(PinName, mode)`: a digital input object, with an optional `mode` parameter (configurable as `PullUp` or `PullDown`). Objects can be read as a int, returning 0 when the input is low and 1 when the input is high.
 - `PwmOut(PinName)`: a PWM output. A float can be assigned to this object, setting the duty cycle (fraction of time the output is high, `1.0f` = 1 period). The default period is 20ms, which is fast enough for LED dimming.
 
Note that if you simply vary the PWM output to a LED linearly, you will get nonlinear perceived brightness (see [Stevens' power law](https://en.wikipedia.org/wiki/Stevens%27_power_law) for more information). To compensate, you want the PWM duty cycle to be the square of the desired perceived brightness.

See the Checkoff Spec for more details of what your firmware should do. There are many ways to structure your code to achieve the desired effect, some easier and cleaner than others, but checkoff will not take into account code quality.

## Checkoff Spec
_How to get credit for what you did!_ 

Objective: demonstrate you can use a breadboard, set up some basic circuits, write some basic code, and compile and deploy it.

1. Show us your circuit with firmware running. A LED should be pulsing, approximately linearly varying in perceived intensity from 0-100% brightness then ramping back down to 0%, with a full cycle taking approximately 2 seconds. The ramping should appear smooth.
2. Press and hold the button. The LED intensity should freeze at the point where the button is pressed. After the button is released, the LED should continue pulsing as described in the previous section. 
3. We may ask you to show your code as well as explain parts of it. Make sure you understand everything you write!
