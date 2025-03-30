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

## a) Linux. Asenna Debian tai Kali Linux virtuaalikoneeseen. (Tätä alakohtaa ei poikkeuksellisesti tarvitse raportoida, jos sinulla ei ole mitään ongelmia. Jos on mitään haasteita, tee täsmällinen raportti)

- Asentelin Kalin täältä https://www.kali.org/get-kali/#kali-live
- Latasin kaksi eri isoa, mutta tätä päädyin käyttämään ![image](https://github.com/user-attachments/assets/fa98f291-8902-4303-8560-81195cdaab85)
- Asennuksessa ei ongelmia

## b) Ei voi kalastaa. Osoita, että pystyt katkaisemaan ja palauttamaan virtuaalikoneen Internet-yhteyden

- Voidaan nyt kokeilla ensin, että kaikki toimii. Tuossa -c ja 5 tarkoittaa siis sitä, että se tekee pingin 5-kertaa ja lopettaa, jotta ei tarvitse montaa minuuttia odotella
![image](https://github.com/user-attachments/assets/2478af21-f765-4f06-947f-98014e062c15)
- Kaikessa yksinkertaisuudessaan navigoidaan tohon (tai mennään asetusten kautta), mutta koska kaikki on yhden klikkauksen päässä, se mielestäni on ajanhukkaa
- Klikataan Disconnect ![image](https://github.com/user-attachments/assets/6c241b07-8bcd-46ea-a9bc-adc93177ae8d)
- Nyt pitäisi olla kaikki pois. Voidaan varmistaa se pingaamalla, jolloin Googlen nimipalvelimiin ei saa yhteyttä
- ![image](https://github.com/user-attachments/assets/16758cb7-2585-4400-becb-c019c2347ad7)
- Ja avaamalla varuiksi vielä FireFox ja yrittämällä avaamata sillä jokin sivu

![image](https://github.com/user-attachments/assets/a62e81f6-f751-4429-89a7-82a94710fa18)

- Kaikki siis ok. Palautuu samalla taktiikalla, eli klikataan portin kuvasta - valitaan oikea yhteys
- ![image](https://github.com/user-attachments/assets/c750cc53-570c-4e2a-bd8f-0a63b195adaf)
- Voidaan vielä varmistaa, että pystytään surffailemaan kalakuvia
- ![image](https://github.com/user-attachments/assets/0de6e555-e857-4162-9880-0db9cd3eca02)


## c) Wireshark. Asenna Wireshark. Sieppaa liikennettä Wiresharkilla. (Vain omaa liikennettäsi. Voit käyttää tähän esimerkiksi virtuaalikonetta).

- Kaikki alkaa ```sudo apt-get install wireshark```
- ```whoami``` ahven ```sudo adduser ahven wireshark``` ```newgrp wireshark``` ```wireshark```
- Käytännössä täysin siis Teron ohjeiden mukaan. Tässä ei kuitenkaan kysytty tuota ei sudojen osallisuutta pakettien kaappaamiseen.
- ![image](https://github.com/user-attachments/assets/b02a93e8-f45c-4e9d-a2e1-a95d954419c5)
- Valitaan tuo eth0
- Sen voi myös tarkistaa ```ip a``` - komennolla, josta näkyy sen olevan aktiivisessa käytössä
- ![image](https://github.com/user-attachments/assets/78c1ac62-283c-40ee-9442-842ac21f7be8)



















### Lähteet
Wireshark - Getting Started, Tero Karvinen (03/2025), Luettavissa: https://terokarvinen.com/wireshark-getting-started/, Luettu 30/03/2025
Network Interface Names on Linux, Tero Karvinen (03/2025), Luettavissa: https://terokarvinen.com/network-interface-linux/, Luettu 30/03/2025


