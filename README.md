Installing Arch: [Installing Arch](https://wiki.archlinux.org/title/Installation_guide)

Windoof/Tiny11:
===========

### Dual Boot:
- Windows immer zuerst installieren
- EFI partition größer machen:
> Shift+F10
>
> `Diskpart.exe`
> 
> `list disk` -> 0
> 
> `select disk 0` (oder andere Zahl)
> 
> `create partition efi size=1000`

### Windows konfigurieren
- Whatsapp installieren
- Chrome installieren: `$Path = $env:TEMP; $Installer = "chrome_installer.exe"; Invoke-WebRequest "http://dl.google.com/chrome/install/375.126/chrome_installer.exe" -OutFile $Path$Installer; Start-Process -FilePath $Path$Installer -Args "/silent /install" -Verb RunAs -Wait; Remove-Item $Path$Installer`
- Fast Startup & Hibernation ausmachen: `> powercfg /H off`

Arch Installation vorbereiten
===========

- Herunterladen: [Download Seite](https://archlinux.org/download/) | [Direkter Download](https://geo.mirror.pkgbuild.com/iso/2024.09.01/)
- Bootstick erstellen (Rufus,...)
- Neu Starten
- Secure Boot deaktivieren (F2 Bios Menue)
- Boot Menue (F12): von Usb Booten
- Machen lassen bis komische Farben auftauchen, bei ekelhaftem piepen (Menü) gerne enter um Prozess zu beschleunigen

Arch Installieren
===========
Von Stick booten (Secure boot off!)

### Mit Netzwerk verbinden (wenn kein Lan)
`iwctl`

Netzwerk module anzeigen (Wlan, lan) : `device List`

Netzwerke anzeigen (wlan0 ggf. durch device ersetzen) : `station wlan0 get-networks`

Verbinden: `station wlan0 connect` *netzwerkname*

Wenn verbunden, *Strg+C*

Verbindung testen: `ping 3 google.com`

 ### Archinstall:
> pimmel


Linux Scheiß von Jan
===========
### Bluetooth
[Bluetooth Devices, Mac adress](https://wiki.archlinux.org/title/Bluetooth#Dual_boot_pairing)
### iPhone Hotspot
To set up an iPhone cable hotspot on Linux (Arch), you'll need to follow these steps:

1. Install required packages:
	 `libimobiledevice` and `usbmuxd` for USB tethering
	 `pand` for Bluetooth tethering (optional)
2. Pair your iPhone with your computer:
	 Connect your iPhone to your Linux machine via USB
	 Enable Personal Hotspot on your iPhone
	 Verify that `usbmuxd.service` starts automatically when the device is connected
3. Configure USB tethering:
	 Create a netcfg network profile (e.g., `tether`) with the following settings:
		+ `CONNECTION="ethernet"`
		+ `DESCRIPTION="Ethernet via pand tethering to iPhone"`
		+ `INTERFACE="bnep0"`
		+ `IPHONE="00:00:DE:AD:BE:EF"` (replace with your iPhone's MAC address)
		+ `PREUP="pand -E -S -c ${IPHONE} -e ${INTERFACE} -n 2>/dev/null"`
		+ `POSTDOWN="pand -k ${IPHONE}"`
		+ `IP="dhcp"`
4. Activate the profile:
	 Run `netcfg tether` to bring up the interface
	 Verify that your Linux machine is connected to the internet through the iPhone's cellular connection
5. Optional: Configure Bluetooth tethering:
	 Install `blueman` and set up Bluetooth on your Linux machine
	 Pair your iPhone with your computer using Bluetooth
	 Enable Network Access Point mode on your iPhone
	 Verify that `blueman` reports a successful connection

Notes:

 USB tethering is recommended as it provides a more stable connection and uses less battery power than Bluetooth.
 If you're using a jailbroken iPhone, you can install apps like MyWi or iTether for tethering.
 Since iOS 14, USB tethering may not work reliably with Linux.
 Make sure your iPhone's cellular plan supports hotspots/tethering.

Remember to adjust the `IPHONE` MAC address and other settings according to your specific setup.

Sonstiger Müll:
=============

fdisk /dev/sda
    (m)
    p
    n
    enter
    +1024M 
    n
    enter
    enter
    p
    w
Archinstall? lol
    Disk Configuration
        Manual
        Drive waehlen
        efi
            assign mountpoint
            /boot
            waehlen
            mark/unmark to be formatted
            waehlen
            change filesystem
            fat32
        root
            assign mp
            /
            wahlen
            mark\
            waehlen
            change fs
            ext4
        confirm, exit
    hostname arsch
    root unso, nutzer
    Desktop ENV
    Audio: pipewire
    Additional (Anydesk usw)
    Network config: NetworkManager
    Install

    nicht chrooten

Windows in systemd boot fixxen:
    Terminal:
    sudo
    fdisk -l
    pacman -S edk2-shell
    cp /usr/share/edk2-shell/x64/Shell.efi /boot/efishellx64.efi
    cd /boot/loader/entries
    ls -al
    vi efishellx64.conf
    nano efishellx64.conf
        title UEFI Shell x64
        efi /efishellx64.efi
         write
    blkid
        suche sda1, windows efi, UUID

    reboot, UEFI SHELL
        map
            scroll, suche UUID, FS0 oder so
        reset
    arch
        console
        sudo
        cd /boot/loader/entries
        nano windows11.conf
            title Windows 11
            efi /efishellx64.efi
            options -nointerrupt -nomap -noversion HD0a0b:EFI/Microsoft/Boot/Bootmgfw.efi (Backslash Pfad!!)
             save
        ls -al
    reboot
    

