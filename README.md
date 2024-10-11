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

- Herunterladen: [Arch Download Seite](https://archlinux.org/download/) | [Download Seite TU](https://mirror.informatik.tu-freiberg.de/arch/iso/2024.10.01/) | [Direkter Download](https://mirror.informatik.tu-freiberg.de/arch/iso/2024.10.01/archlinux-2024.10.01-x86_64.iso)
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

Netzwerk module anzeigen (Wlan, lan) : `device list`

Netzwerke anzeigen (wlan0 ggf. durch device ersetzen) : `station wlan0 get-networks`

Verbinden: `station wlan0 connect` *netzwerkname*

*passwort eingeben*

Wenn verbunden, *Strg+C*

Verbindung testen: `ping -c 3 google.com` -> Sollte Ping ausspucken und ein paar Zeilen mit "64 bytes from..."

Nun: `archinstall`

 ### Archinstall:
-  Archinstall language -> German (92% oder so steht da)
- Spiegelserver -> Spiegelserver-Region -> Germany (warten, sucht besten Server für downloads) -> Zurück
- Lokalisierung -> Tastaturlayout einstellen, Lokale sprache = `de_DE.UTF-8` ("/" zum suchen), Lokale Kodierung auf UTF-8 lassen -> zurück (schreib ich jetzt das letzte mal dazu xD
- Laufwekskonfig. -> Partionierung -> Empfohlenes verwenden -> Festplatte auswählen (tendenziell das größte, das wo "size" passt) -> btrfs -> Ja -> Ja (Komprimierung verwenden) -> Zurück
- Festplattenverschlüsselung kannste skippen
- Bootloader: Systemd-boot (Standard)
- Vereinigte Kernel-Images kannste skippen (auf Nein/False lassen)
- Swap auf True lassen
- Gerätename/Hostname kannste anpassen wie du magst für tailscale
- Root-Passwort ist admin passwort, setz es und lass es mich wissen (groß-kleinschreibung bitte beachten lel)
- Benutzerkonto -> hinzufügen (name in kleinbuchstaben), passwort setzen und mich wissen lassen danke, ja, soll superuser (admin) sein -> bestätigen und schließen (Bitte dann in standard übersicht prüfen ob da auch steht "1 User(s)", wenn nicht dann bitte nochmal, account ist wichtig xD
- Profil -> Typ -> Desktop -> Gnome (oder Deepin wenns sein muss xD) -> Greeter -> sddm -> zurück (solltest jetzt in der übersicht sein, beim punkt profil. geh bitte nochmal eins hoch und schau ob da gnome, sddm unso steht, ohne grafische Oberfläche hast dus glaube schwerer (kann ich ja nachher wieder deinstallieren lel)
- Audio -> Pipewire
- Kernel kannste so lassen (linux)
- keine zusätzlichen pakete (machen wir naccher)
- Netzwerkkonfiguration -> NetworkManager
- Zeitzone -> Europe/Berlin (gerne suchen mit "/")
- NTP an lassen
- Keine zusätzlichen repos

- Abfahrt Installieren

### Arch einrichten
- Console auf machen

##### Tailscale
- `sudo su` -> USER passwort eingeben
- `pacman -S tailscale`
- `systemctl enable --now tailscaled`
- `tailscale up` -> bekommst link für dein Handy browser, da auf knopf und fertig
- `tailscale set --ssh`

##### Anydesk
###### Yay installieren (Option 1)
- `sudo pacman -Sy` -> USER passwort eingaben (wenn nix gehen probier halt admin lol)
- `sudo pacman -S glibc lib32-glibc`
- `sudo pacman -S yay`

###### Yay installieren (Option 2)
`pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay-bin.git && cd yay-bin && makepkg -si`

###### Anydesk installieren mit yay
- `yay -S anydesk-bin`

- rest mach ich xD

Copilot chat bez. arch server
===========
[Lamp Gist](https://gist.github.com/superjojo140/18e250786d977b27571124f81bba5018)
[Link](https://github.com/copilot/c/4ea1e118-10c5-4fa7-a2a2-631ebce95368)

## I want to setup a "server" on arch linux. I need a database, which is accessible inside the local network (I use tailscale to access it from elsewhere) and has a phpmyadmin console (which should also be accessible elsewhere). What do I need to install and how to configure it, give me a tutorial

To set up a server on Arch Linux with a database accessible within your local network and a phpMyAdmin console, follow these steps:

### Step 1: Install LAMP Stack
1. **Update your system:**
   ```sh
   sudo pacman -Syu
   ```

2. **Install Apache (httpd):**
   ```sh
   sudo pacman -S apache
   ```

3. **Install MariaDB (MySQL alternative):**
   ```sh
   sudo pacman -S mariadb
   ```

4. **Install PHP and required extensions:**
   ```sh
   sudo pacman -S php php-apache php-mysqli
   ```

### Step 2: Configure Apache
1. **Start and enable Apache:**
   ```sh
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```

2. **Edit the Apache configuration file to support PHP:**
   ```sh
   sudo nano /etc/httpd/conf/httpd.conf
   ```
   Add these lines at the end:
   ```apache
   LoadModule php_module modules/libphp.so
   AddHandler php-script .php
   Include conf/extra/php_module.conf
   ```

3. **Restart Apache:**
   ```sh
   sudo systemctl restart httpd
   ```

### Step 3: Configure MariaDB
1. **Initialize the MariaDB data directory:**
   ```sh
   sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
   ```

2. **Start and enable MariaDB:**
   ```sh
   sudo systemctl start mariadb
   sudo systemctl enable mariadb
   ```

3. **Secure the MariaDB installation:**
   ```sh
   sudo mysql_secure_installation
   ```
   Follow the prompts to secure your installation.

### Step 4: Install phpMyAdmin
1. **Install phpMyAdmin:**
   ```sh
   sudo pacman -S phpmyadmin
   ```

2. **Configure phpMyAdmin:**
   Edit the Apache config to include phpMyAdmin:
   ```sh
   sudo nano /etc/httpd/conf/extra/phpmyadmin.conf
   ```
   Add these lines:
   ```apache
   Alias /phpmyadmin "/usr/share/webapps/phpMyAdmin"

   <Directory "/usr/share/webapps/phpMyAdmin">
       DirectoryIndex index.php
       AllowOverride All
       Options FollowSymlinks
       Require all granted
   </Directory>
   ```
   Include the phpMyAdmin config in the main Apache config:
   ```sh
   sudo nano /etc/httpd/conf/httpd.conf
   ```
   Add this line at the end:
   ```apache
   Include conf/extra/phpmyadmin.conf
   ```

3. **Restart Apache:**
   ```sh
   sudo systemctl restart httpd
   ```

### Step 5: Adjust Firewall and Access
1. **Allow HTTP service through the firewall:**
   ```sh
   sudo ufw allow http
   ```

2. **Configure MariaDB to listen on all interfaces:**
   Edit the MariaDB config:
   ```sh
   sudo nano /etc/my.cnf.d/server.cnf
   ```
   Under the `[mysqld]` section, add:
   ```ini
   bind-address = 0.0.0.0
   ```
   Restart MariaDB:
   ```sh
   sudo systemctl restart mariadb
   ```

### Step 6: Access phpMyAdmin and Database
1. **Access phpMyAdmin:**
   Open a web browser and go to:
   ```
   http://your_server_ip/phpmyadmin
   ```
2. **Login using your MariaDB credentials.**

### Step 7: Tailscale Configuration (Optional)
1. **Install Tailscale:**
   ```sh
   sudo pacman -S tailscale
   ```

2. **Start and enable Tailscale:**
   ```sh
   sudo systemctl start tailscaled
   sudo systemctl enable tailscaled
   ```

3. **Authenticate Tailscale:**
   ```sh
   sudo tailscale up
   ```

You should now have a fully functional server with a database and phpMyAdmin accessible both locally and remotely via Tailscale.

## Can I controll my server using a gui with tailscale? A friend of mine needs his stuff there and has to access it just like with anydesk.

You can control your server using a GUI over Tailscale by using a remote desktop solution. Here are a few options you can choose from:

1. **VNC (Virtual Network Computing)**:
   - Install a VNC server on your Arch Linux server (e.g., TigerVNC).
   - Install a VNC client on the remote machine.
   - Configure Tailscale to allow connections through the VNC port.

2. **RDP (Remote Desktop Protocol)**:
   - Install an RDP server on your Arch Linux server (e.g., xrdp).
   - Use the built-in Remote Desktop Connection client on Windows or other RDP clients on macOS/Linux.
   - Configure Tailscale to allow connections through the RDP port.

3. **AnyDesk**:
   - Install AnyDesk on your Arch Linux server.
   - Install AnyDesk on the remote machine.
   - Use Tailscale to connect to the server's Tailscale IP address.

### Example: Setting up VNC on Arch Linux

1. **Install TigerVNC**:
   ```sh
   sudo pacman -S tigervnc
   ```

2. **Configure VNC server**:
   - Create a configuration file `~/.vnc/xstartup` and add the following:
     ```sh
     #!/bin/sh
     exec /usr/bin/startxfce4
     ```
   - Make the file executable:
     ```sh
     chmod +x ~/.vnc/xstartup
     ```

3. **Start VNC server**:
   ```sh
   vncserver
   ```

4. **Allow VNC through the firewall**:
   ```sh
   sudo ufw allow 5901/tcp
   ```

5. **Use Tailscale to connect**:
   - Get the Tailscale IP address of your server.
   - Use a VNC client to connect to `tailscale_ip:5901`.

This should allow your friend to access the server GUI over Tailscale similarly to AnyDesk.

## He needs to access this server through vnc from any devide (android phone, windows,...) is this possible?

Yes, it is possible to access the server through VNC from any device, including Android phones and Windows PCs. Here's how your friend can do it:

### Step 1: Install a VNC Client on the Device
- **Android**: Install a VNC client like [VNC Viewer](https://play.google.com/store/apps/details?id=com.realvnc.viewer.android) from the Google Play Store.
- **Windows**: Install a VNC client like [TightVNC](https://www.tightvnc.com/download.php) or [RealVNC](https://www.realvnc.com/en/connect/download/viewer/).

### Step 2: Connect to the Server
1. **Open the VNC client on the device**.
2. **Enter the Tailscale IP address of your server followed by the VNC port** (e.g., `100.64.x.x:5901`).
3. **Enter the VNC password** (set during the VNC server setup on the Arch Linux server).

### Step 3: Access the Desktop Environment
- After connecting, your friend will be able to see and interact with the server's desktop environment as if they were physically present at the machine.

This setup allows for remote GUI access to the server from any device.


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
    

