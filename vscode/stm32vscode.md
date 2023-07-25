Source: https://gist.github.com/bonnee/393c4be25d2e8620d9ec406073940d3a

# STM32 developement on Visual Studio Code
This guide will help you install and setup Visual Studio Code for programming and debugging STM32 boards.

I tested this guide under Arch Linux and Ubuntu 18.04. If you get it to work under other setups, please let me know so I will update the steps with more info.

## Warning
If you were using STM32CubeIDE or SystemWorkbench before, you need to convert your projects in order for them to work. The conversion procedure is fully reversible.<br/>
Up until the time of writing this guide, it is not possible to use STM32CubeIDE and Visual Studio Code on the same project unless some configuration changes are made on CubeIDE. 
Besides that, it is reccomended that every person that works on a project runs the same working environment.

# Installation
## 1. STM32CubeMX
If you already have CubeMX installed, you can skip this step
#### Arch Linux
Install `stm32cubemx` from the [AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository). If you don't have access to the AUR or you just don't want to use it, follow the step below.
#### Inferior operating systems (Windows, Ubuntu, Temple OS...)
Download the .zip from [here](https://www.st.com/en/development-tools/stm32cubemx.html) (you have to sign up to ST's website) and install the right version for your OS.

## 2. GNU ARM Embedded Toolchain
These are the tools needed to compile and debug the code.
#### Arch Linux
Install `arm-none-eabi-gcc` `arm-none-eabi-gdb` `arm-none-eabi-newlib`.
#### Debian/Ubuntu
Install `gcc-arm-none-eabi` `gdb-multiarch` `libnewlib-arm-none-eabi`.
#### Windows
Install the toolchain from [arm's website](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)

## 3. Make
#### Linux
Install `make` from your package manager
#### Windows
Install make from this [this](http://gnuwin32.sourceforge.net/packages/make.htm) page (updated in 2006 though...). 
It should be possible to get the current version of make through [WSL](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux), but I don't have experience with it.

## 4. OpenOCD
Open On-Chip Debugger is the software that will take care of uploading the compiled software to the STM32, and during debug, it will open the connection between the computer and the STM32.<br/>
If you are on Windows you could probably get OpenOCD installed, but I don't have idea on how to do it. Contact me if you're willing to find a way.
#### Linux
Install `openocd` from your package manager

## 5. Visual Studio Code
The next steps aren't really necessary to get the thing working. You could just use the a terminal and your favourite editor and you would have (almost) all the functionality of the complete setup. Visual Studio Code is just a pretty front-end.
#### Linux
[This](https://code.visualstudio.com/docs/setup/linux) page contains the instructions to install the latest version of VSCode on many distributions.
#### Windows
Download and install the program from the [official website](https://code.visualstudio.com/).

## 6. C/C++ Extension
This extension will take care of intellisense, syntax highlighting and more.<br/>
Install [this](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) extension in Visual Studio Code.
## 7. stm32-for-vscode
Same as above, but with [this](https://marketplace.visualstudio.com/items?itemName=bmd.stm32-for-vscode) extension. 

# Configuration
## 1. STM32CubeMX
By default CubeMX generates projects in a format called EWARM. Unfortunately, EWARM is currently not supported by the VSCode extension, but the more generic Makefile structure is. In order to configure CubeMX to support VSCode you have to navigate to `Project Manager->Project->Toolchain/IDE` and set it to Makefile.

Under the `Code generator` tab, enable the `Copy all used libraries into the project folder` option. This step is needed because stm32-for-vscode doesn't support the implicit inclusion of libraries yet.<br/>
After doing that, click on `GENERATE CODE`; You should see the new Makefile project structure being created.

## 2. Visual Studio
Using the `Open folder` menu in Visual Studio Code, navigate to your project's root folder and open it.
Now open the command palette (`Ctrl+Shift+P` or `F1`) and run `Build STM32 Project`. A terminal should appear and you will see gcc (hopefully) compiling your project.

# Usage
## Compiling & Flashing
After you call `Build STM32 Project` for the first time, stm32-for-vscode will create two custom tasks (that can be accessed by typing `Ctrl+Shift+B`) to build your code and to flash it into the STM32 board.

## Debugging
To debug the code just press `F5` inside VSCode and the debugger will start automatically. Remember to stop the debugger before flashing new code.

# Troubleshooting
### `Error: libusb_open() failed with LIBUSB_ERROR_ACCESS` during upload
This happnes because you don't have permission to open the serial device. In order to fix this you need to add your user to the group that owns the serial interface. To get the serial interface name you can run `dmesg | grep tty` after plugging the STM32 in. Read the last line, you should see something like:
```
cdc_acm 2-2:1.2: ttyACM0: USB ACM device
```
In this example `ttyACM0` is the device name.<br/>
Now that you know the name you can get the owner by doing `ls -l /dev/ttyACM0`. The output looks like this:
```
crw-rw----  1 user   group   4, 166 set 11 08:45 /dev/ttyACM0
```
`group` indicates the group name.

Now you just need to add your user to the group by doing `sudo usermod -aG group $USER`, substituting `group` with the group name you have discovered above.

You are done. Log out of your account and log back in to apply the change.