### Disclaimer: I'm not affiliated with Project Valhalla and I'm not responsible for any possible damage to devices

# Install latest Windows 11 ARM image with Project Valhalla installer
### (GPU DRIVER UPDATE BELOW WINDOWS TUTORIAL↓↓↓)

Follow the oficial Project Valhalla guide from here https://github.com/ProjectValhalla/OdinMultiBootGuides
until you finish the USB stick preparation

Your USB stick should look like this before starting this guide:

![App Screenshot](https://i.imgur.com/VHoJnOM.png)

### In case of error during windows installation (after plugging USB stick step) you don't need to start from scratch from QFIL, or repartition android/windows, just follow the steps carefully to prepare the USB stick again, insert it into your Odin and reboot. Installation will proceed as normal.

## Windows Image download and preparation
1 - Go to https://massgrave.dev/windows_arm_links.html and download the latest Windows 11 ARM for your language

2 - Mount the downloaded ISO

3 - We will have to split the new install.wim from the ISO file because it is bigger than 4GB. The Project Valhalla installer uses a FAT32 formatted USB stick and FAT32 does not allow >4GB files. Refer to this documentation for next steps: https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/split-a-windows-image--wim--file-to-span-across-multiple-dvds?view=windows-11

4 - Create a folder named "sources" on the root of your C:\ drive

5 - With the mounted ISO execute the following command (change the ISO drive letter (F:) if necessary):

```Dism /Split-Image /ImageFile:F:\sources\install.wim /SWMFile:C:\sources\install.swm /FileSize:4000```

6 - You will have two files in C:\sources\
    ```install.swm```
    ```install2.swm```

## USB stick modification
7 - Delete the install.wim in your USB stick located in images folder.

8 - Copy the install.swm and install2.swm from C:\sources to USB\images\

9 - The images folder in your USB stick should look like this:

![App Screenshot](https://i.imgur.com/kywBTjf.png)

10 - In your USB stick, go to installer folder, open the install.cmd file with a text editor.

11 - In line 57 change /Index:1 to /Index:2, this is because Windows 11 Pro is Index 2 in the retail ISOs. If you skip this step you will install Windows Enterprise and will be able to login only with a corporate account.

    Line 57:

    From: dism /Apply-Image /ImageFile:%WinPESource%images\install.swm /SwmFile:%WinPESource%images\install*.swm /Index:1 /ApplyDir:w: /ScratchDir:w:\temp

    To: dism /Apply-Image /ImageFile:%WinPESource%images\install.swm /SwmFile:%WinPESource%images\install*.swm /Index:2 /ApplyDir:w: /ScratchDir:w:\temp

## ATTENTION: There are 2 lines that contain /Index:1 very close to eachother, its the second one, number 57

12 - Eject your USB and proceed with the installation

### The "Getting ready" screen on first boot after removing USB might take some time, its not stuck, just wait and it will reboot itself

### Protip: during the windows setup do not connect to Wi-Fi to allow creation of local account

### After installation you can install updates from Windows Update like any other PC, but it takes some time to download and install

![App Screenshot](https://i.imgur.com/p2DE1Ay.png)

# Update GPU Driver from POCO F1

## This allows to run some games that were not running with stock valhalla drivers (ie Need For Speed Most Wanted 2005)

1 - Go to https://github.com/edk2-porting/WOA-Drivers/releases/tag/2210.1-fix

2 - Download the beryllium.zip file

3 - Extract the GPU folder to your desktop

4 - Go to device manager, video adapters, right click Adreno 630, update drivers

5 - Choose to select driver from your computer, in the next screen select the option to search for drivers in a folder, select "from disk"

6 - Select the qcdx850.inf file from your Desktop\GPU folder

7 - It will display a message saying that the driver is not signed, install anyway

8 - Your GPU will display as Adreno 680 in device manager, it works fine, just ignore

![App Screenshot](https://i.imgur.com/PtKBMSR.png)

# Other tweaks

You can use winutil to debloat windows after installation, but I recommend using only the "tweaks" tab, don't use it to install softwares or to mess with windows update: https://github.com/ChrisTitusTech/winutil

Do not install .NET 8.0 (any arch) as it seems to corrupt Windows installation.

Do not install Borderless Gaming as it seems to corrupt Windows installation.

Do not try to remove Test Mode watermark using "Bcdedit.exe -set TESTSIGNING OFF" it will cause boot error

Do not try to remove Test Mode watermark with 3rd party tools, some are flagged with virus, others cause boot error / system corruption

Activate .NET 2.0 and 3.5 from control panel: https://www.howtogeek.com/880208/how-to-enable-net-framework-2-0-and-3-5-in-windows-11/

Install Visual C++ Redistributable:(ARM64, X86 and X64): https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170

[Install OpenCL™ and OpenGL® Compatibility Pack](https://www.microsoft.com/store/productId/9NQPSL29BFFF?ocid=pdpshare)

Install DirectX Runtime: https://www.microsoft.com/en-us/download/details.aspx?id=35

Activate Windows 11 using massgrave: https://github.com/massgravel/Microsoft-Activation-Scripts

You can use dControl to disable Windows Defender to squeeze a little bit more of performance: 

https://www.sordum.org/9480/defender-control-v2-1/

DO AT YOUR OWN RISK!!!! You have to disable Windows Defender real time protection before downloading dControl

And you are good to go.
