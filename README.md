# Install latest Windows 11 ARM image with Project Valhalla installer
### (GPU DRIVER UPDATE BELOW WINDOWS TUTORIAL↓↓↓)

Follow the oficial Project Valhalla guide from here https://github.com/ProjectValhalla/OdinMultiBootGuides
until you finish the USB stick preparation

Your USB stick should look like this before starting this guide:

![App Screenshot](https://i.imgur.com/VHoJnOM.png)

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

11 - In line 57 change /Index:1 to /Index:2, this is because Windows 11 Pro is Index 2 in the retail ISOs. If you skip this step you will install Windows Business and will be able to login only with a corporate account.

    Line 57:

    From:
    dism /Apply-Image /ImageFile:%WinPESource%images\install.swm /SwmFile:%WinPESource%images\install*.swm /Index:1 /ApplyDir:w: /ScratchDir:w:\temp

    To: dism /Apply-Image /ImageFile:%WinPESource%images\install.swm /SwmFile:%WinPESource%images\install*.swm /Index:2 /ApplyDir:w: /ScratchDir:w:\temp

12 - Eject your USB and proceed with the installation

### Protip: during the windows setup do not connect to Wi-Fi to allow creation of local account

### After installation you can install updates from Windows Update like any other PC, but it takes some time to download and install
