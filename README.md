Hackintosh - Mojave 安裝紀錄
===


### 配備

* Processor : Intel i5-8400
* MotherBoard : ASRock Z370M-ITX/ac
* Memory : Micron Ballistix 2400 8G x 2
* SSD : Micron MX500 1TB
* CASE : Fractal Design Node 304
* PSU : Corsair SF450 Gold
* Cooling Fan : Cryorig C7
* Bluetooth&Wifi : Boardcom BCM94352Z

### 安裝
#### 前置作業
* 準備軟體：
    * Unibeats
    * Mojave
    * Multibeats 
    * Clover Configurator
    * Kexts:
        1. [AirportBrcmFixup.kext - Wifi](https://github.com/acidanthera/AirportBrcmFixup/releases)
        2. BrcmFirmwareRepo.kext - Bluetooth
        3. [BrcmPatchRAM2.kext - Bluetooth](https://github.com/RehabMan/OS-X-BrcmPatchRAM)
        4. SmallTree-Intel-211-AT-PCIe-GBE.kext - 第二網卡
        5. [USBInjectAll.kext - 修正 USB](https://bitbucket.org/RehabMan/os-x-usb-inject-all)
        6. XHCI-200-series-injector.kext - Wifi
        7. USBPorts.kext - 修正 USB (如果有用 Hackintool 再裝)
* BIOS 設定
    * Advanced > USB Configuration > XHCI Hand-off > ENABLED
    
* 製作安裝 USB Drive
    1. 格式化 USB Drive 成日誌式 GPT 架構
    1. 在英文環境下才能執行 Unibeats 安裝在 USB
    1. 用附件 config.plist 覆蓋 USB: /EFI/CLOVER/config.plist
    1. 用 Clover Configurator 修改 config.plist :
        * Boot -> nv_disabe=1,
        * Devices -> Fake ID -> IntelGFX = 0x12345678
        * Graphics -> ig-platform-id = 0x12345678
        > 為了避免開機卡在 [macOS Mojave 10.14.3: IOConsoleUsers: gIOScreenLockState 3, hs 0, bs 0, now 0, sm 0x0](https://olarila.com/forum/viewtopic.php?t=8849&start=20)
        * **記得儲存 config.plist**


#### 進行安裝
* USB 要插在 [ USB 2.0 port](https://www.tonymacx86.com/threads/solved-install-10-13-6-hid-legacy-shim-2-prohibited.257627/)
* 拔除其他不相干的硬碟
* Boot 選擇 USB 的 UEFI
* 安裝選單步驟跟原生 mac 步驟一樣
* 安裝過程會重開機，在Clover選單選擇系統碟
#### 安裝完成後
1. 執行 MultiBeats
    * Drivers > Audio > AppleALC
    * Drivers > Audio > 100/200/300 series audio support (HDAS - HDEF)
    * Drivers > Misc > FakeSMC (+plugins if you prefer)
    * Drivers > Network > Intel > IntelMausiEthernet v2.4.0 (for first Ethernet port)
    * Bootloader > Clover UEFI
    * Customise > Graphics > WhateverGreen (and NOT INSTALL "Intel HD630 Coffee Lake", ONLY WHATEVERGREEN)
    * System Definition > Mac Mini > macmini6,2 (will be replaced next, maybe not needed, but I did, and all was fine)
    > 安裝完先別重開機
1. 覆蓋 config.plist 到 系統 : /EFI/CLOVER/config.plist 
1. 複製 USBPorts.kext to 系統 : /EFI/CLOVER/Kext/Other
1. 複製要安裝的 Kext (1 ~ 6) 到 Desktop/Install
1. 執行 安裝 Kext:
    ```bash=
    cd ~/Desktop/Install
    sudo cp -R *.kext /Library/Extension
    sudo kextcache -i /
    ```
    > 也可以使用 KextBeat 進行安裝
1. 重開機BIOS的Boot改成系統碟，就可以讀取系統碟的EFI上面的Clover導引並正確載入Kext，這時候就可以拔掉USB了

* 如果不想開機看到其他選項如：Preboot,Recovery... 修改config.plist : 
    * Gui > Hide Volume > Add Preboot ...
### 多重開機選單
* 先用 macOS 格式化 Windows Disk > Mac日誌式,GPT 架構
* 開啟 BIOS > CSM
* 用 Windows Install USB 開機
* 磁碟分割時候保留 macOS 產生的 EFI,用其他的空間就好
* 安裝完成後 關閉 BIOS > CSM
* 再改回 Clover EFI 開機 > 選擇 Windows EFI 磁區，就可以導引到 Windows 了
### 除錯
* Bluetooth Keyboard or Mouse delay 
    : 用 Hackintool
* 用 DisplayPort 有時候畫面會黑掉只剩游標
    : 改用 HDMI 或是重開螢幕
* Windows and Mac 每次開機時間都會不同步
    : 因為Windows 是 GMT, Mac 是 UTC,把 Windows 改成 UTC就好
### 參考
[HOW TO MAKE A MACOS 10.14 MOJAVE FLASH DRIVE INSTALLER](https://hackintosher.com/guides/how-to-make-a-macos-10-14-mojave-flash-drive-installer/)

[[Great Success] mini HTPC: ASRock Z370M-ITX/ac - i5-8400 - UHD 630 Graphics - Mojave](https://www.tonymacx86.com/threads/great-success-mini-htpc-asrock-z370m-itx-ac-i5-8400-uhd-630-graphics-mojave.270629/)

[[Success][Guide] Gigabyte Z370n WiFi / i5-8400 / 970 EVO / Mojave 10.14.4 Vanilla "Hackintosh Deluxe"
](https://hackintosher.com/forums/thread/success-guide-gigabyte-z370n-wifi-i5-8400-970-evo-mojave-10-14-4-vanilla-hackintosh-deluxe.704/)

[GUIDE TO FRESH INSTALLING MACOS MOJAVE ON A HACKINTOSH](https://hackintosher.com/guides/guide-to-fresh-installing-macos-mojave-on-a-hackintosh-10-14)
### 太閒可以做的事
* [自訂 System Info](http://www.idownloadblog.com/2017/01/13/how-to-modify-about-this-mac-hackintosh/)