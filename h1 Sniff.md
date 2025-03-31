# Kotitehtävät

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
- Tähän väliin pitää mainita, että GUI näyttää laatikoita sen takia, että painoin "cancel" päivitysten kohdalla, kun piti valita character - set. Korjasin tämän kuitenkin hakemalla ```sudo apt-get upgrade``` ja käyttämällä kohtaa "Get recommended"... tai jotain siihen suuntaan. Viimeinen kohta kuitenkin. Nyt numerot ym. näkyy normaalisti.
- Valitaan tuo eth0
- Sen voi myös tarkistaa ```ip a``` - komennolla, josta näkyy sen olevan aktiivisessa käytössä
- ![image](https://github.com/user-attachments/assets/78c1ac62-283c-40ee-9442-842ac21f7be8)

- Avasin selaimen ja hain siihen duckduckgo - hakukoneen. Capture on siis ajalta, kunnes ankka oli avattuna.
- Ensin näkyy ne, montako pakettia Wireshark kerkesi nappaamaan ennen pysäytystä: ![image](https://github.com/user-attachments/assets/58c8b52a-84cd-4a8c-89cc-4f05bee8d544)
  
- Tässä kohdassa filtteröin pelkän dns - kohdan (1. vaaleanruskea), jossa jokainen rivi on oma DNS- kyselynsä tai vastaus.
- Firefoxin toiminta käynnistyessä (2. sininen). Se tekee DNS - kyselyitä eri Mozillan palveluihin.
-  Tämän jälkeen duckduckgo.com (3. vihreä) sivuston aukeaminen ja parannus sekä siihen liittyvät muut kyselyt. Oletan, että jos "ankka" olisi selaimena, se tekisi lähes yhtä monta DNS - kyselyä, mutta pelkkänä hakukoneena tulos on aika suppea
- Alhaalta (4. oranssi) näkyy maalatun paketin tiedot - Frame kohta näyttää, kuinka iso paketti on, Ethernet 2 verkkoliikenteen lähdöstä loppuun, IPv4 - versio, UDP jossa mukana mm. lähde- ja kohdeportit sekä DNS. Jokaista kohtaa pystyy tutkimaan tarkemmin, jota teemme myöhemmin.
- ![image](https://github.com/user-attachments/assets/beff6ad7-846b-44dc-b4c6-b46391c31367)

## d) Oikeesti TCP/IP. Osoita TCP/IP-mallin neljä kerrosta yhdestä siepatusta paketista. Voit selityksen tueksi laatikoida ne ruutukaappauksesta.

- Wiresharkilla on oma kiva työkalunsa tähän avuksi, joka visualisoi protokollien ja liikenteen sisältöä. Kokeilin tätä toisella kurssilla, kun leikin Wiresharkin kanssa -> Edit -> Preferences -> Layout -> Haluamaan paikkaan "Packet Diagram" -> Apply. Toki se on vain visuaalinen työkalu ja auttaa hahmottamaan eri osia ja itse malli pitää tietää.

- ![image](https://github.com/user-attachments/assets/59035004-bf6e-47fc-8581-e7b3237891db)

- 1. Linkkikerros (Link layer -> ARP, Tunnels, PPP, MAC, jne) 
  ![image](https://github.com/user-attachments/assets/20c358db-457e-415b-879f-5dbc3ad1ee7a)

- Käytännössä koko Ethernet II koostaa linkkikerroksen. Siinä näkyy MAC - lähettäjä sekä kohdeosoitteet ja tyypin datalle, missä se lähetetään, eli tässä tapauksessa IPv4

- 2. Internet - kerros (Internet layer -> IP, ICMP, NDP, ECN, IPsec jne)

![image](https://github.com/user-attachments/assets/4a18b4fa-11ca-4a70-a1ec-c2ed0f202b34)

- Internet - kerros käsittää tässä tapauksessa kohdan Internet Protocol Version 4, jossa jälleen näkyy kohde sekä lähdeosoitteet IPv4 - muodossa. Siitä näkyy versio, pituus, TTL eli kauanko paketilla kesti päästä "verkon yli" reitittimen sisältä ja Header Lenght, joka määrittää otsikon pituuden. Protokollana tässä tapauksessa TCP

- 3. Kuljetuskerros (Transport layer -> TCP, UDP, DCCP, SCTP, RSVP, QUIC jne)

- ![image](https://github.com/user-attachments/assets/0575af3d-7fc4-4135-9446-e8fb7a7b093b)

- Kuljetuskerroksessa Transmission Control Protocol näkyy portit sekä lähettäjälle että vastaanottavalle osapuolelle. Siinä varmistuu, että paketti on järjestyksessä, koossa, vähillä erroreilla.

- 4. Sovelluskerros (Application layer -> HTTP, FTP, HTTPS, IMAP, SSH, Telnet jne)

- ![image](https://github.com/user-attachments/assets/cf45be9f-7f05-4365-b816-018f44276568)

- Valitsin tähän tehtävään juuri sen, jossa konkreettisesti näkyy HTTP:n toiminta. Eli siinä näkyy se, mitä haetaan, miten haetaan, versio, host, josta haetaan, kielet, mitä sille tehdään (keep-alive) ja lopussa mm. koko pyynnön URL, jota seuraamalla saa hienon onnistumis - viestin
- ![image](https://github.com/user-attachments/assets/a54ff5a6-057f-4ad9-b960-1280dfe82053)

## e) Mitäs tuli surffattua? Avaa [surfing-secure.pcap.](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/surfing-secure.pcap). Tutustu siihen pintapuolisesti ja kuvaile, millainen kaappaus on kyseessä. Tässä siis vain lyhyesti ja yleisellä tasolla. Voit esimerkiksi vilkaista, montako konetta näkyy, mitä protokollia pistää silmään. Määrästä voit arvioida esimerkiksi pakettien lukumäärää, kaappauksen kokoa ja kestoa.














### Lähteet
Wireshark - Getting Started, Tero Karvinen (03/2025), Luettavissa: https://terokarvinen.com/wireshark-getting-started/, Luettu 30/03/2025
Network Interface Names on Linux, Tero Karvinen (03/2025), Luettavissa: https://terokarvinen.com/network-interface-linux/, Luettu 30/03/2025
Internet Protocol Suite, wikipedia (edit 21/03/2025), Luettavissa: https://en.wikipedia.org/wiki/Internet_protocol_suite, Luettu 30/30/2025


