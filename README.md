# Embedded Controller for MSI laptops

## Disclaimer

This driver might not work on other laptops produced by MSI. Use it at your own risk, we are not responsible for any damage suffered.

Also, and until future enhancements, no DMI data is used to identify your laptop model. In the meantime, check the list of supported devices and the msi-ec.c file before using.

The driver has no effect on ACPI, so if you have any ACPI errors, the driver can't fix them; consider extracting the ACPI tables and/or following the [Arch wiki](https://wiki.archlinux.org/title/DSDT)

You might want to use a graphical interface instead of executing commands to use the driver: [MControlCenter](https://github.com/dmitry-s93/MControlCenter). 


## Supported devices

Before installation, check if your device is supported in [this list](https://github.com/BeardOverflow/msi-ec/discussions/277).

Can't find your device in the list? Check the contribution guides and open a new issue.

## Contribute
Eager to support the project? Your help is always welcome to keep the project alive and going!
- Read how to add support for your device and get the necessary information on Windows and Linux in [this guide](docs/device_support_guide.md).
- Get solutions for common MSI laptop problems (ACPI errors, slow charger detection,...) and submit fixes if you have any in this [discussion](https://github.com/BeardOverflow/msi-ec/discussions/130#).
- If you want to go deeper: [read into this discussion regarding EC firmware naming patterns](https://github.com/BeardOverflow/msi-ec/discussions/98).


## Installation

### From GitHub

#### Prerequisities:

> [!NOTE]
> Minimal kernel version is 6.5.0
> 
> Check version with `uname -r`

1. Install the following packages using the terminal:
   - For Debian: `sudo apt install build-essential linux-headers-amd64`
   - For Ubuntu: `sudo apt install build-essential linux-headers-generic`
   - For Fedora: `sudo dnf install kernel-devel`
   - For Arch:   `sudo pacman -S --needed base-devel linux-headers`
2. Clone this repository and cd to it: `git clone https://github.com/BeardOverflow/msi-ec && cd msi-ec`   
3. Choose one of the following installation methods

#### (Recommended) Installation using [DKMS](https://en.wikipedia.org/wiki/Dynamic_Kernel_Module_Support):

1. Verify that dkms is available: `which dkms`
2. Install the msi-ec kernel module: `sudo make dkms-install`
3. **REBOOT**
4. (Optional) To uninstall: `sudo make dkms-uninstall`

#### (OR) Classic installation (without DKMS):

1. Build the driver: `make`
2. Install the msi-ec kernel module: `sudo make install`
3. **REBOOT**
4. (Optional) To uninstall: `sudo make uninstall`

### From AUR (Arch Linux)
1. Install any AUR helper ([yay](https://github.com/Jguer/yay) for example)
2. Run `yay -S msi-ec-git`

### On NixOS
Add these lines to your configuration
```nix
boot.extraModulePackages = [ config.boot.kernelPackages.msi-ec ];
boot.kernelModules = [ "msi-ec" ];
```


## Current Support in the Kernel

Features already merged in kernel 6.4 and up:
- Battery thresholds

Still not merged:
- Enable/Disable webcam
- Switch Function key and Windows Key positions
- Battery mode (min, medium, max)
- Enable/Disable Cooler boost
- Power shift modes (eco, confort, sport, turbo)
- Enable/Disable Super battery
- Fan modes (auto, silent, basic, advanced)
- Cpu/Gpu Temperatures
- Cpu/Gpu Fan speeds


## Usage

This driver exports a few files in its own platform device, msi-ec, and is available to userspace under:

- `/sys/devices/platform/msi-ec/webcam`
  - Description: This entry allows enabling the integrated webcam (as if it was done by a keyboard button).
  - Access: Read, Write
  - Valid values:
    - on: integrated webcam is enabled
    - off: integrated webcam is disabled

- `/sys/devices/platform/msi-ec/webcam_block`
  - Description: This entry allows blocking the integrated webcam. Being blocked by this entry, webcam can't be enabled by a keyboard button or by writing into the webcam file.
  - Access: Read, Write
  - Valid values:
    - on: integrated webcam is blocked
    - off: integrated webcam is not blocked

- `/sys/devices/platform/msi-ec/fn_key`
  - Description: This entry allows switching the position between the function key and the windows key.
  - Access: Read, Write
  - Valid values:
    - left: function key goes to the left, windows key goes to the right
    - right: function key goes to the right, windows key goes to the left

- `/sys/devices/platform/msi-ec/win_key`
  - Description: This entry allows changing the position for the function key.
  - Access: Read, Write
  - Valid values:
    - left: windows key goes to the left, function key goes to the right
    - right: windows key goes to the right, function key goes to the left

- `/sys/devices/platform/msi-ec/cooler_boost`
  - Description: This entry allows enabling the cooler boost function. It provides powerful cooling capability by boosting the airflow.
  - Access: Read, Write
  - Valid values:
    - on: cooler boost function is enabled
    - off: cooler boost function is disabled

- `/sys/devices/platform/msi-ec/available_shift_modes`
  - Description: This entry reports all supported shift modes.
  - Access: Read
  - Valid values: Newline separated list of strings.

- `/sys/devices/platform/msi-ec/shift_mode`
  - Description: This entry allows switching the shift mode. It provides a set of profiles for gaining CPU & GPU overclock/underclock.
  - Access: Read, Write
  - Valid values:
    - unspecified (read-only)
    - Values reported by `/sys/devices/platform/msi-ec/available_shift_modes`. Some of the possible values:
      - eco: low clock frequency and voltage for the CPU & GPU, aka power saving mode
      - comfort: dynamic clock frequency and voltage for the CPU & GPU, aka power balanced mode
      - sport: full clock frequency and voltage for the CPU & GPU, aka default desktop mode
      - turbo: over-voltage and over-clock for the CPU & GPU, aka overclocking mode

- `/sys/devices/platform/msi-ec/super_battery`
  - Description: This entry allows switching the super battery function.
  - Access: Read, Write
  - Valid values:
    - on: super battery function is enabled
    - off: super battery function is disabled

- `/sys/devices/platform/msi-ec/available_fan_modes`
  - Description: This entry reports all supported fan modes.
  - Access: Read
  - Valid values: Newline separated list of strings.

- `/sys/devices/platform/msi-ec/fan_mode`
  - Description: This entry allows switching the fan mode. It provides a set of profiles for adjusting the fan speed under specific criteria.
  - Access: Read, Write
  - Valid values:
    - Values reported by `/sys/devices/platform/msi-ec/available_fan_modes`. Some of the possible values:
      - auto: fan speed adjusts automatically
      - silent: fan is disabled
      - basic: fixed 1-level fan speed for CPU/GPU (percent)
      - advanced: fixed 6-levels fan speed for CPU/GPU (percent)

- `/sys/devices/platform/msi-ec/fw_version`
  - Description: This entry reports the firmware version of the motherboard.
  - Access: Read
  - Valid values: Represented as string

- `/sys/devices/platform/msi-ec/fw_release_date`
  - Description: This entry reports the firmware release date of the motherboard.
  - Access: Read
  - Valid values: Represented as string

- `/sys/devices/platform/msi-ec/cpu/realtime_temperature`
  - Description: This entry reports the current cpu temperature.
  - Access: Read
  - Valid values: 0 - 100 (celsius scale)

- `/sys/devices/platform/msi-ec/cpu/realtime_fan_speed`
  - Description: This entry reports the current cpu fan speed.
  - Access: Read
  - Valid values: 0 - 100 or 0 - 150 (percent)

- `/sys/devices/platform/msi-ec/gpu/realtime_temperature`
  - Description: This entry reports the current gpu temperature.
  - Access: Read
  - Valid values: 0 - 100 (celsius scale)

- `/sys/devices/platform/msi-ec/gpu/realtime_fan_speed`
  - Description: This entry reports the current gpu fan speed.
  - Access: Read
  - Valid values: 0 - 100 or 0 - 150 (percent)

In addition to these platform device attributes the driver registers itself in the Linux power_supply subsystem (Documentation/ABI/testing/sysfs-class-power) and is available to userspace under:

- `/sys/class/power_supply/<supply_name>/charge_control_start_threshold`
  - Description: Represents a battery percentage level, below which charging will begin.
  - Access: Read, Write
  - Valid values: 0 - 100 (percent)
    - 50: when min battery mode is configured
    - 70: when medium battery mode is configured
    - 90: when max battery mode is configured

- `/sys/class/power_supply/<supply_name>/charge_control_end_threshold`
  - Description: Represents a battery percentage level, above which charging will stop.
  - Access: Read, Write
  - Valid values: 0 - 100 (percent)
    - 60: when min battery mode is configured
    - 80: when medium battery mode is configured
    - 100: when max battery mode is configured

Led subsystem allows us to control the leds on the laptop including the keyboard backlight

- `/sys/class/leds/platform::<led_name>/brightness`
  - Description: sets the current state of the led.
  - Access: Read, Write
  - Valid values: 0 - 1
    - 0: Led off
    - 1: Led on

- `/sys/class/leds/msiacpi::kbd_backlight/brightness`
  - Description: sets the current state of keyboard backlight.
  - Access: Read, Write
  - Valid values: 0 - 3
    - 0: Off
    - 1: On
    - 2: Half
    - 3: Full

### Debug mode

You can use module *parameters* to get direct read-write access to the EC or force-load a configuration
for a specific firmware.

**Be very careful, since writing into the unknown addresses of the EC memory may be dangerous!**
If you did something wrong, please, reset the EC using the reset button on the bottom of your laptop.

#### `debug`, bool

Set this parameter to `true` to enable debug attributes, located at `/sys/devices/platform/msi-ec/debug/`,
even if your EC is not supported yet.
Normal attributes will still be accessible if your firmware is supported.

You can use `make load-debug` command to load the module in the debug mode after building it from source.

| name       | permissions | description                                                                                                                                                                    |
|------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fw_version | RO          | returns your EC firmware version                                                                                                                                               |
| ec_dump    | RO          | returns an EC memory dump in the form of a table                                                                                                                               |
| ec_get     | RW          | receives an EC memory address in the hexadecimal format on write; returns a value stored in the EC memory at this address on read                                              |
| ec_set     | WO          | receives an address-value pair in the following format: `aa=vv`, where `aa` and `vv` are address and value in the hexadecimal format; then writes the value into the EC memory |

#### `firmware`, string

Set this parameter to a supported EC firmware version to use its configuration and test if it is compatible with your EC.
**Please verify that the attributes return the correct data before attempting to write into them!**
