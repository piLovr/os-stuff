# archinstall
Script(s) that provide arch installation/configuration helpers

Installing Arch: [Installing Arch](https://wiki.archlinux.org/title/Installation_guide)

Arch download: [Download Page](https://archlinux.org/download/)

Arch direct download Link: [Download](https://mirror.informatik.tu-freiberg.de/arch/iso/2024.09.01/)

Mit Wlan verbinden:
==========
`iwctl`

Netzwerk module anzeigen (Wlan, lan) : `device List`

Netzwerke anzeigen (wlan0 ggf. durch device ersetzen) : `station wlan0 get-networks`

Verbinden: `station wlan0 connect` *netzwerkname*

Wenn verbunden, *Strg+C*

Verbindung testen: `ping 3 google.com`
