Kotitehtävät

Kotitehtävät ovat kurssilta "Verkkoon Tunkeutuminen ja Tiedustelu - Network Attacks and Reconnaissance" ja löytyvät osoitteesta https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#verkkojen-perusteet 

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 30/03/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 30/03/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)


## h1 Sniff
x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)
## *Karvinen 2025: Wireshark - Getting Started https://terokarvinen.com/wireshark-getting-started/*
- Asennus perus - ```sudo apt-get install wireshark```
- "Should non-superusers be able to capture packets - yes" - Sniffailuun tehdään oma ryhmä
- Itsellekin muistiin kelloasetukset ```sudo timedatectl set-time "2025-03-30 21:06"```
- ```sudo adduser <nimi> wireshark``` - Lisätään sniffigrouppiin
- Jotta ei tarvitse kirjautua ulos saadakseen muutokset voimaan, laiskojen tapa avata uusi shelli ```newgrp wireshark``` ```wireshark```
- Voi tallentaa ja ladata myöhemmin tarkastelua varten
*Filtterit*
  - Voi lisätä filttereitä lähempää tarkastelua varten ja niitä voi myös yhdistää, esim "tcp and ip.addr == < ip-osoite >", jotta näkee vain haluamansa liikenteen
- Oikeeklikkaus - "Follow: TCP Stream" - salaamattoman liikenteen lukemista tekstinä

## Karvinen 2025: Network Interface Names on Linux https://terokarvinen.com/network-interface-linux/
- Network interfaces - hyvin pitkälti verkkokortti, mutta ei fyysinen
- Nimen alku identifioi tyypin
- en -> Wired *E*ther*n*et
- wl -> *WL*AN, WiFi
- lo -> *Lo*opback adapter

Yleiset network interface nimet
- wlp4s0 -> WiFi card
- enp1s0 -> Wired ethernet card
- lo -> Loopback adapter
- enx738899738899 -> Wired ethernet card -> x jälkeen MAC - numero - eli laitteen identifiointi tai se, missä se on yhteydessä tietokoneeseen
- ```ip a```
- ```ip route```
- Voi katsoa omat nimensä
* Lisää aiheesta: https://wiki.debian.org/NetworkInterfaceNames, https://www.freedesktop.org/software/systemd/man/latest/systemd.net-naming-scheme.html





















### Lähteet
Wireshark - Getting Started, Tero Karvinen (03/2025), Luettavissa: https://terokarvinen.com/wireshark-getting-started/, Luettu 30/03/2025
Network Interface Names on Linux, Tero Karvinen (03/2025), Luettavissa: https://terokarvinen.com/network-interface-linux/, Luettu 30/03/2025


