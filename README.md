# How to install Pop!\_OS on a Razer Blade 2020
This post will outline the steps I took to install and configure a dual boot with POP!\_OS on my Razer Blade 15 early 2020 edition, and give you my overall experience so far.

Everything is running properly with the default and the laptop is fully functional! I am detailing the steps that I followed to proceed with the installation and configuration. You can contribute to this guide by submitting a pull request.

Disclaimer: Follow this tutorial at your own risk. I take no responsibility for your computer, operating system or data.

## What you'll need

You will need:
- a Razer Blade 15 Laptop
- a Razer Blade 15 Charger
- a USB Flash Drive (at least 8 GB)
- a Wi-Fi or a wired connection

## Update the drivers in Windows 10
During these first step, we'll prepare the laptop for the installation and make sure that everything is up to date.

- Download the official BIOS update guide along with the installation program: <http://drivers.razersupport.com//index.php?_m=downloads&_a=view&parentcategoryid=1004&pcid=992&nav=0,350,992>
- You can check for Windows updates here on this page: <https://support.microsoft.com/en-us/help/4027667/windows-10-update>
- Run Intel DSA to check if there are outdated hardware drivers on your laptop, and make sure to update them all: <https://www.intel.com/content/www/us/en/support/detect.html>
- Make sure that NVIDIA Driver is up to date: <https://www.geforce.com/drivers>

#### Optional: check your Windows installation.

This is more Windows 10 maintenance at this point, but why not?

Open the terminal as an administrator:
```
SFC /scannow
```
```
DISM /Online /Cleanup-Image /CheckHealth
```

## Configure Windows and the BIOS
Here we'll make sure that Windows 10 and the BIOS are properly configured for the dual boot.

#### Disable fast startup

You can check out this online resource: https://www.windowscentral.com/how-disable-windows-10-fast-startup

I recommend that you disable fast startup on Windows 10. In some cases, Windows 10 may not completely shut down but instead go into some kind of hibernation mode, even though you specifically selected to shut down your computer. If it happens and you then boot into Linux, you may not be able to easily boot back into Windows 10.

#### Resize the Windows partition

You can check out this online resource: https://www.howtogeek.com/101862/how-to-manage-partitions-on-windows-without-downloading-any-other-software/

You can decide how much space you want to allocate to the POP!\_OS installation. You can also resize the partition with the POP!\_OS installer but it is best practice to do into from Windows. You can leave the space you reserved for POP!\_OS unallocated and format it from the installer.

Next we'll take care of the BIOS configuration. To go into the BIOS shut down your computer, then press the power button and immediately begin to rapidly press the ESC or F1 key at a rate of 1.5 times per second. Once you are in the BIOS:
- Disable fast boot: Go to 'Boot' > 'Fast Boot' select 'Disable'
- Disable secure boot: Go to 'Security' > 'Secure Boot' select 'Disable'
- Disable CSM (default): Go to 'Boot' > 'CSM Configuration' > 'CSM Support' select 'Disable'

## Prepare an installation media

Here we'll prepare an installation media for POP!\_OS

#### Download POP!\_OS ISO

You can grab their latest ISO from their official website: https://pop.system76.com/

Select the NVIDIA option. I chose the LTS version 20.04, but feel free to try the latest version.

#### Flash the installation media

I used Rufus: https://rufus.ie/

Feel free to use any other preferred methods. I used GPT as the partition scheme and BIOS or UEFI as the target system. If you're not sure, leave the defaults.

## Install POP!\_OS

- Shut down the laptop then press the power button and immediately begin to rapidly press the F12 key and select the USB Flash Drive in the boot menu.
- Once you've booted, I recommend that you connect to the internet via wifi or via an ethernet cable.
- Run the installer from the desktop. Start the installation process
- For the hard drive configuration most of the internet recommends to choose the option "install alongside Windows". This is a good option, but I don't like the fact that it touches Window's boot partition. I personally went with the custom option. For that select the 'Custom (Advanced)' option and click on the 'Custom (Advanced)' button. This resource explains how to proceed: https://linoxide.com/distros/install-pop-os-20-04/.

I selected the unallocated space that I created in Windows 10 and selected 'New' to create a FAT32 partition of 512 MiB with the flag 'boot/efi' that I configured as the '/boot' mount point. I then used the rest of the unallocated space to create an EXT4 partition with the flag 'root' that I configured as the '/' (root). Once the partitions were created, on the next step, I make sure you turn on the 'Use partition' option on the FAT32 partition you just created and set 'Use as:' to '/boot/efi'.  Turn on the 'Use partition' option on the EXT4 partition you and set 'Use as:' to 'Root /'. 

- Once the installation is complete, you will be prompted to reboot. Remove the installation USB when prompted.

When the laptop boots back up, it will detect and automatically select POP!\_OS boot loader. We will can change that in the next step.

## Configure the boot order in the BIOS

To go into the BIOS shut down your computer, then press the power button and immediately begin to rapidly press the ESC or F1 key. Once you are in the BIOS select the 'boot' tab. Here you can select Windows 10 or POP!\_OS as the first option. To boot into the second option, you'll have to press F12 during the boot and select it manually.

## Finish POP!\_OS configuration

First install all updates, either via the graphical interface or via the command line:
```
sudo apt update
sudo apt upgrade
```

Install openrazer and polychromatic to control the keyboard RGB patterns (like Razer Synapse on Windows). Run the following commands in terminal:

```
sudo add-apt-repository ppa:openrazer/stable
sudo add-apt-repository ppa:polychromatic/stable

sudo apt update

sudo apt install openrazer-meta polychromatic
```

In my case the program failed to start, the following command fixed it:
```
sudo gpasswd -a $USER plugdev
```
Reboot your computer.

## Donate to System 76

Because why not? For me POP!\_OS runs perfectly on my Razer Blade and made using Linux on that laptop a totally viable option, which definitely adds to the appeal of the laptop!

## Credits

- This walk through has been inspired by the following post: https://github.com/nrobinson2000/linux-blade
- You can also find a wealth of info here: https://github.com/rolandguelle/razer-blade-stealth-linux
