Installing Arch: [Installing Arch](https://wiki.archlinux.org/title/Installation_guide)

Tiny11 setup:
===========
EFI partition größer machen:
> Shift+F10
>
> `Diskpart.exe`
> 
> `list disk` -> 0
> 
> `select disk 0` (oder andere Zahl)
> 
> `create partition efi size=1000`

- Whatsapp installieren
- Chrome installieren: `$Path = $env:TEMP; $Installer = "chrome_installer.exe"; Invoke-WebRequest "http://dl.google.com/chrome/install/375.126/chrome_installer.exe" -OutFile $Path$Installer; Start-Process -FilePath $Path$Installer -Args "/silent /install" -Verb RunAs -Wait; Remove-Item $Path$Installer`

Vorbereiten:
===========

- GGF in Windows die Partition verkleinern (Datentraegerverwaltung)
- Herunterladen: [Download Seite](https://archlinux.org/download/) | [Direkter Download](https://geo.mirror.pkgbuild.com/iso/2024.09.01/)
- Bootstick erstellen (Rufus,...)
- Neu Starten
- Secure Boot deaktivieren (F2 Bios Menue)
- Boot Menue (F12): von Usb Booten
- Machen lassen bis komische Farben auftauchen, bei ekelhaftem piepen (Menue) gerne enter um Prozess zu beschleunigen

Mit Wlan verbinden:
==========
`iwctl`

Netzwerk module anzeigen (Wlan, lan) : `device List`

Netzwerke anzeigen (wlan0 ggf. durch device ersetzen) : `station wlan0 get-networks`

Verbinden: `station wlan0 connect` *netzwerkname*

Wenn verbunden, *Strg+C*

Verbindung testen: `ping 3 google.com`

Arch Installieren:
===========

Archinstall:
> pimmel








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
    

