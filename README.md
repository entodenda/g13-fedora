# Readme File

This code was forked from https://github.com/Tetz95/linux-g13-driver.

It hasn't been updated in 4 years, and seemed like a good place to start for my own purposes.

## Notes

I've only used this in Fedora 42 so far. Based on the fork, it should work in Arch and Ubuntu. I will eventually test it.

## Requirements

### libusb-1.0

For Fedora, libusb and its development files are required. You can install them by running:

```
sudo dnf install libusb1 libusb1-devel
```

For Ubuntu, it should be installed already, but if you don't have it, you can get it by typing:

```
sudo apt-get install libusb-1.0-0
```

For Arch Linux you can install it by typing:

```
sudo pacman -S libusb
```

### Java version 17 or higher

For Fedora, you'll need a full, "headful" Java Runtime Environment (JRE) to use the graphical interface. The java-latest-openjdk package is recommended as it includes both the runtime and GUI support.

``` 
sudo dnf install java-latest-openjdk 
```

For Ubuntu, it can be installed by typing:

```
sudo apt-get install default-jre
```

For Arch Linux it can be installed by typing:

```
sudo pacman -S jre8-openjdk
```

## Download

Download zip file from [TBD](https://github.com/entodenda/g13-fedora).  
Unzip the file into a folder you wish to build it.

## Build

Open the terminal  
Go to the directory where you unzipped your download  
type `make`

## Permissions & Udev Rules (For Fedora)

This driver needs permissions to access the USB device and create virtual input events. The simplest way to achieve this is by adding your user to the necessary groups and creating a udev rule.

1. Create a `plugdev` group and add your user to it.
    ```
    sudo groupadd plugdev
    sudo usermod -aG plugdev <username>
    ```

2. Add your user to the `input` group.
    ```
    sudo usermod -aG input <username>
    ```

3. Create a `udev` rule to grant access to the G13 device.
    ```
    sudo nano /etc/udev/rules.d/99-logitech-g13.rules
    ```
    Add this line to the file, save, and exit.
    ```
    SUBSYSTEM=="usb", ATTR{idVendor}=="046d", ATTR{idProduct}=="c21c", MODE="0666"
    ```

4. Create a `udev` rule for the uinput device.
    ```
    sudo nano /etc/udev/rules.d/99-uinput.rules
    ```
    Add this line to the file, save, and exit.
    ```
    KERNEL=="uinput", MODE="0660", GROUP="input", OPTIONS+="static_node=uinput"
    ```

5. Reload the udev rules and restart the system for changes to take effect.
    ```
    sudo udevadm control --reload-rules
    sudo udevadm trigger --subsystem-match=misc
    ```

## Running Application

Run the config tool first!
In a command prompt go to the directory where you unzipped your download and type:

    java -jar Linux-G13-GUI.jar

This will bring up the UI and create the initial files needed for your driver.
All config files are saved in `$(HOME)/.g13`

Run the driver
In a command prompt go to the directory where you unzipped your download and type:

    ./G13-Linux-Driver

If you are configuring the application while the driver is running, the driver will not pick up changes unless you select a different bindings set or you can restart the driver.

The top 4 buttons under the LCD screen select the bindings.

The joystick currently only supports key mappings.
