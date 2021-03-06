# Building RefindPlus on Mac OS
These are step-by-step instructions for setting up a bespoke EDK II build environment for buidling the rEFInd Boot Manager on Mac OS systems using Xcode development tools.

These steps have been verified on Mac OS v10.12 (Sierra) and Mac OS v10.14 (Mojave) and should also work on Mac OS v10.13 (High Sierra) and Mac OS v10.15 (Catalina).


## Activate Mac OS Development Tools

### Xcode
Download [Xcode](https://developer.apple.com/xcode) from the Mac App Store and install.
**NB:** Tested on v9.4.1 and v11.3 but should also work on v10.x and v12.x

### Xcode Commandline Tools
After installing Xcode, you will additionally need to install commandline tools.  To do this, at a Terminal prompt, enter:

```
$ xcode-select --install
```

### HomeBrew

#### Installing HomeBrew

While Xcode provides a full development environment as well as a suite of different utilities, it does not provide all tools required for Tianocore development.  These tools can be provided in a number of ways, but the two most popular ways come from HomeBrew and MacPorts.  This guide focuses on HomeBrew but equivalent steps can be taken in MacPorts.

If you do not have HomeBrew already installed, go to the [HomeBrew Home Page](https://brew.sh) and follow the instructions.
Installation involves copying and pasting a one-line code string into Terminal and pressing `Enter`.

#### Update PATH environment variable

Tools installed using HomeBrew are placed in `/usr/local/bin` which is non-standard to avoid conflicts with pre installed tools.  The `PATH` environment variable must therefore be updated so newly installed tools are found.

```
$ export PATH=/usr/local/bin:$PATH
```

### Install the MTOC Utility with HomeBrew

The mtoc utility is required to convert from the macOS Mach-O image format to the PE/COFF format as required by the UEFI specification.

```
$ brew install mtoc
$ brew upgrade mtoc
```

### Install the Netwide Assembler (NASM) with HomeBrew

The assembler used for EDK II builds is the Netwide Assembler (NASM).

```
$ brew install nasm
$ brew upgrade nasm
```

### Install the ACPI Compiler with HomeBrew

The ASL compiler is required to build ACPI Source Language code to support EDK II firmware builds.

```
$ brew install acpica
$ brew upgrade acpica
```

### Install the QEMU Emulator with HomeBrew (Optional)

The QEMU emulator from http://www.qemu.org/ must be installed to support running the OVMF platforms from the OvmfPkg.

```
$ brew install qemu
$ brew upgrade qemu
```

## Prepare the TianoCore (UDK2018) Environment
### Fork the Refind-UDK Repository

Navigate to `https://github.com/dakanji/Refind-UDK` and fork the repository

### Clone the Forked Refind-UDK Repository
In Terminal, create a `RefindPlus` folder under your `Documents` folder, `cd` to that directory and clone the rEFInd ready TianoCore (UDK2018) repository into an `edk2` folder:

```
$ mkdir ~/Documents/RefindPlus
$ cd ~/Documents/RefindPlus
$ git clone https://github.com/YOUR_GITHUB_USERNAME/Refind-UDK.git edk2
$ cd ~/Documents/RefindPlus/edk2
$ git checkout rudk
$ git remote add upstream https://github.com/dakanji/Refind-UDK.git
```

**NB:** Replace `YOUR_GITHUB_USERNAME` above with your actual GitHub User Name.
Your Refind-UDK folder (UDK2018) will be under `Documents/RefindPlus/edk2`

### Build BaseTools C Tool Binaries

```
$ cd ~/Documents/RefindPlus/edk2
$ make -C BaseTools/Source/C
```


## Prepare the RefindPlus Environment
### Fork the RefindPlus Repository

Navigate to `https://github.com/dakanji/RefindPlus` and fork the repository

### Clone the Forked RefindPlus Repository

In Terminal, clone the forked `RefindPlus` repository into a `Working` folder under your `RefindPlus` folder:

```
$ cd ~/Documents/RefindPlus
$ git clone https://github.com/YOUR_GITHUB_USERNAME/RefindPlus.git Working
$ cd ~/Documents/RefindPlus/Working
$ git checkout GOPFix
$ git remote add upstream https://github.com/dakanji/RefindPlus.git
```

**NB:** Replace `YOUR_GITHUB_USERNAME` above with your actual GitHub User Name.
Upstream will be `dakanji/RefindPlus` as shown.
Your woking repository will be under `Documents/RefindPlus/Working`


## Build RefindPlus
- Navigate to your `/Documents/RefindPlus/edk2/000-BuildScript` folder in Finder
- Drag the `RefindPlusBuilder.sh` file into Terminal
  - Enter a space and `MyEdits`, or any other branch name, to the end of the line if you want to build on that branch
  - If nothing is entered, the script will default to the `GOPFix` branch
- Press `Enter`


## Updating Cloned Repositories
### Update with the RepoUpdater Script (Recommended)
- Navigate to your `/Documents/RefindPlus/edk2/000-BuildScript` folder in Finder
- Drag the `RepoUpdater.sh` file into Terminal
- Press `Enter`

### Update Manually
#### Refind-UDK
In Terminal, run the following commands:

```
$ cd ~/Documents/RefindPlus/edk2
$ git checkout rudk
$ git pull upstream rudk
$ git push
```

#### RefindPlus
In Terminal, run the following commands:

```
$ cd ~/Documents/RefindPlus/Working
$ git checkout GOPFix
$ git pull upstream GOPFix
$ git push
```
