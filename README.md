# MKS WIFI Guide

> **NOTE**: information of this repository are primarily referred to install the MKS wifi board into a FLSUN Q5 3D printer (MKS Robin Nano V1.2 board). However,  most of the information in this article should be compatible with most 3D printers on the market.


The MKS WIFI board is a simple ESP8266 board (see [this](https://www.mischianti.org/2021/11/17/mks-wifi-for-makerbase-robin-boards-and-how-to-wiring-esp12-nodemcu-1/) article). 


Like any other ESP8266 board, the firmware is composed by two parts:
  - the ESP Firmware. The binary file must be named `MksWifi.bin`
  - the SPIFFS File System, used to store firmware files (i.e., the WebView pages). The binary file must be named `MksWifi_WebView.bin`.

## Original firmware
> Makerbase repo: [https://github.com/makerbase-mks/MKS-WIFI](https://github.com/makerbase-mks/MKS-WIFI).

The original firmware functions are limited only to a TCP communication socket, only usable with a Cura plugin (see [this](https://www.mischianti.org/2021/11/24/mks-wifi-for-makerbase-robin-communication-protocol-and-cura-plugin-3/) article), and a simple firmware update page.

> TODO: add a firmware update page image

The firmware doesn't have SPIFFS File System, so the installation needs only to flash the `MksWifi.bin` file.

### Install

SD method (must be used for the first flash on a branded new board):

1. Get the bin file
   
    You can download it from the Makerbase repo, into the folder `firmware_release`. Simply download the zip file and extract it.
    
    You can also found some pre-downloaded versions into the `Original firmware/Firmware per WiFi` and `Original firmware/MksWifi_CS1.0.4_200227` folders (file `MksWifi.bin`).

2. Copy the `MksWifi.bin` file to the sdcard

    Simply copying the `MksWifi.bin` should be enough to start the ESP8266 flash. However, a full printer flash is recommended: copy the entire printer firmware to the root of the sdcard, including the `MksWifi.bin` file. An example is the `Original firmware/Firmware per WiFi` folder.

3. Insert sdcard to the relative board(Robin series and mks tft series)
4. Reboot the board, firmware will updated automatically

Webpage method (only usable on a already flashed board):

1. Get the bin file
   
   See the SD method notes.

2. Open the `<printer_ip>` webpage.
3. Choose the `MksWifi.bin` file **under the `wifi firmware` section**
4. Click the `update` button

**NOTE**: The SD method seems to work only for the first flash on a new board (*need to investigate this issue*). So, for successive flashes you need to use the webpage method.

## BeePrint
> BeePrint repos
>  - firmware: [https://github.com/xreef/MKS_WIFI_upgrade_with_BeePrint_web_interface](https://github.com/xreef/MKS_WIFI_upgrade_with_BeePrint_web_interface)
>  - web interface: [https://github.com/xreef/beeprint_3d_printer_web_interface](https://github.com/xreef/beeprint_3d_printer_web_interface)


The BeePrint alternative firmware is based on the original Makerbase firmware, to which it adds a websocket feature, needed to create feature-rich dashboards.

> TODO: Add MKS-WiFi-BeePrint-interface.jpg image

> **NOTE**: BeePrint firmware only works on printers that use a Repetiter firmware (no Marlin).

### Install
0. Install the [original firmware](#original-firmware)
    
    See the notes of the firmware currently installed on the wifi board to find how to restore the original firmware

1. Download the `MksWifi.bin` and the `MksWifi_WebView.bin` files from the [release](https://github.com/xreef/MKS_WIFI_upgrade_with_BeePrint_web_interface/releases/tag/F1.2_B1.5) page
2. Open the `<printer_ip>` webpage
3. Flash the `MksWifi_WebView.bin` file **under the `web view` section**
4. Flash the `MksWifi.bin` file **under the `wifi firmware` section**

> To restore the original firmware, go to the `<printer_ip>/update` page and flash the original `MksWifi.bin` file under the `wifi firmware` section

## ESP3D
> ESP3D repo: [https://github.com/luc-github/ESP3D](https://github.com/luc-github/ESP3D)

ESP3D is another alternative firmware, compatible with all printer board firmwares (see the [compatibility list](https://github.com/luc-github/ESP3D/wiki/Firmware--support)). However, there are some limitation with this firmware and Marlin (see below). 

> TODO: add ESP3D image

### Marlin limitations
Marlin has some limitation with the files management. Uploaded files must follow the [DOS 8.3](https://en.wikipedia.org/wiki/8.3_filename) format (max 8 characters for the name and max 3 characters for the extension). Files in the sdcard that doesn't follow this format will be displayed truncated into the ESP3D files list.

In addition, the ESP3D V2.1 use a very slow file upload protocol (see [this](https://github.com/luc-github/ESP3D/discussions/535) and [this]((https://github.com/luc-github/ESP3D/issues/575)) issues). The V3.0 implements the MKS protocol, but is not compatible with the Q5 board.

> For more information, see also this [article](https://github.com/Foxies-CSTL/Marlin_2.0.x/wiki/5.Firmware-Wifi#for-the-wifi-module-esp8266-or-mks-wifi), referred to the custom Q5 Marlin firmware.

### Install
The installation consists of two part: installation of the firmware and uploading of the webpages. 

The official [installation guide](https://github.com/luc-github/ESP3D/wiki/Install-Instructions) says to build the firmware form source, flash it on the board and finally upload the web files to the file system using the integrated uploader. No needs to flash the file system.

However, other file systems previously installed on the wifi board (i.e. BeePrint) may cause conflicts with ESP3D, so it is a good practice to overwrite the file system during the installation of ESP3D (or fully erase the wifi board).

So, in this guide we will first flash an old pre-built ESP3D version, overwriting the file system with the pre-built ones included into the release. Then, we will update it with a new builded ESP3D version.

1. Install the [original firmware](#original-firmware)
    
    See the notes of the firmware currently installed on the wifi board to find how to restore the original firmware

2. Flash the pre-build ESP3D firmware

   This pre-build firmware rar file comes from the Makerbase's [MKS-SGEN_L-V2](https://github.com/makerbase-mks/MKS-SGEN_L-V2/wiki/MKS_TFT_WIFI) repository. You can download it from there, otherwise you'll find the pre-downloaded version into the `ESP3D/ESP3D firmware (with FS)` folder.

   1. Get the required files from the [repository](https://raw.githubusercontent.com/makerbase-mks/MKS-SGEN_L-V2/master/Tools/ESP3D%20firmware.rar) page (or from the `ESP3D/ESP3D firmware (with FS)`) folder)
   2. Rename the `firmware.bin` file to `MksWifi.bin` and the `esp3d.spiffs.bin` to `MksWifi_WebView.bin`
   3. Open the `<printer_ip>` webpage
   4. Flash the `MksWifi_WebView.bin` file **under the `web view` section**
   5. Flash the `MksWifi.bin` file **under the `wifi firmware` section**
3. Connect to the `ESP3D` wifi network and open the page `http://192.168.0.1` 
4. Upload the `index.html.gz` file

    You can download it from the [repository](https://raw.githubusercontent.com/makerbase-mks/MKS-SGEN_L-V2/master/Tools/ESP3D%20firmware.rar) or find it into the `ESP3D/ESP3D firmware (with FS)`) folder.
5. Click the `"Go to ESP3d interface"` button and complete the configuration wizard
  > TODO: add the ESP-UI.png image
6.  Build the latest ESP3D version from source
    1. Clone the ESP3D [repository](https://github.com/luc-github/ESP3D) (make sure you are on the `2.1.x` branch)
    2. Open it with PlatformIO
    3. Check that the default environment is the esp8266 (`default_envs` entry into the `platformio.ini` file). Or you can also select the `env:esp8266` environment from the VS Code blue bottom bar
    4. Build the firmware (checkmark icon on the VS Code blue bottom bar)
    5. You'll find the builded `firmware.bin` file into the `.pioenvs/esp8266/` folder. **Rename it to `MksWifi.bin`**

    **Note**: since the 2.1.x branch is no longer updated, you could also use the pre-builded version into the `ESP3D/ESP3D Precompiled V2.1.1` folder.
7. Update the ESP3D firmware with the one built in step above
   1. Go to the `<printer_ip>` page
   2. Select the ESP3D tab
   3. Click the update button (the orange one)
   4. Select the `MksWifi.bin` file and click `Update`
8. Update the file system
   1. Go to the `<printer_ip>` page
   2. Select the ESP3D tab
   3. Click the ESP3D Filesystem button (the green one)
   4. Delete all file listed
   5. Upload all the files you find into the `esp3d\data` folder of the [repository](https://github.com/luc-github/ESP3D). Alternatively, if you have used the pre-compiled V2.1.1 version, upload all the files you find into the `ESP3D/ESP3D Precompiled V2.1.1/FilesToUpload` folder


> To restore the original firmware:
> 1. Go to the `<printer_ip>` page
> 2. Select the `ESP3D` tab
> 3. Click on the update button (the orange one) 
> 4. Flash the original `MksWifi.bin` file

## esphome-marlin-uart
> esphome-marlin-uart repo: [https://github.com/mulcmu/esphome-marlin-uart](https://github.com/mulcmu/esphome-marlin-uart)

This is a custom ESPHome component to integrate a FLSUN QQ-S Pro printer with Home Assistant (but it should be compatible with other printers).

I not have tested it.


## Useful links
- **Makerbase MKS-WIFI repository**. Contains all the information about the board, and the original `MksWifi.bin` firmware: [https://github.com/makerbase-mks/MKS-WIFI](https://github.com/makerbase-mks/MKS-WIFI)
- **Mischianti MKS Wifi articles series**. Contains a lot of information about the mks wifi hardware and original firmware, how to compile and flash it by hardware on a MKS WIFI board (as well as on a ESP9266 NodeMCU). It also contains information about the BeePrint custom firmware: [https://www.mischianti.org/category/project/web-interface-beeprint-for-mks-wifi/](https://www.mischianti.org/category/project/web-interface-beeprint-for-mks-wifi/)
- **BeePrint repositories**: [firmware](https://github.com/xreef/MKS_WIFI_upgrade_with_BeePrint_web_interface) and [web interface](https://github.com/xreef/beeprint_3d_printer_web_interface)
- **Foxies-CSTL FLSUN Marlin repository**. Contains, into the Wifi section of the wiki, some information about ESP3D Marlin compatibility, and how to flash it on a FLSUN printer: [wiki](https://github.com/Foxies-CSTL/Marlin_2.0.x/wiki/5.Firmware-Wifi) and [repository](https://github.com/Foxies-CSTL/Marlin_2.0.x/tree/Firmwares/ESP3D)
- **Makerbase MKS-SGEN_L-V2 board wiki**. Contains some information about how to flash ESP3D on MKS WIFI board using a USB-TTL converter: [https://github.com/makerbase-mks/MKS-SGEN_L-V2/wiki/MKS_TFT_WIFI](https://github.com/makerbase-mks/MKS-SGEN_L-V2/wiki/MKS_TFT_WIFI)