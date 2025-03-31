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
- Ja avaamalla varuiksi vielä FireFox ja yrittämällä avata sillä jokin sivu

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

## e) Mitäs tuli surffattua? Avaa [surfing-secure.pcap.](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/surfing-secure.pcap) Tutustu siihen pintapuolisesti ja kuvaile, millainen kaappaus on kyseessä. Tässä siis vain lyhyesti ja yleisellä tasolla. Voit esimerkiksi vilkaista, montako konetta näkyy, mitä protokollia pistää silmään. Määrästä voit arvioida esimerkiksi pakettien lukumäärää, kaappauksen kokoa ja kestoa.

- Pintapuolisesti tutkittuna, mentiin googlesta terokarvinen.com ja samalla aktivoitui sivuston seuranta gc.zgo.at eli GoatCounter - niminen analytiikkatyökalu, jolla tutkitaan sivuston kävijätietoja ja statistiikkaa.
- Kun menee Statistics - välilehdelle ja siitä "Capture File Properties", näkee tietoja kaapatusta paketista
  
- ![image](https://github.com/user-attachments/assets/861c7ccc-a2c4-447e-9b53-5b316aa6642c)

- Tässä esimerkiksi näkyy se, montako pakettia löytyy (283), kauanko kesti (7,536s), Bytes (122445) jne.
- Endpoints - välilehdeltä Ethernet - kohdassa on vain 2 MAC - osoitetta, joten uskoisin, että tässä on vain 2 fyysistä laitetta.
- ![image](https://github.com/user-attachments/assets/03f6317b-766c-454d-a0b7-a9baefd3a381)
- Protokollat QUIC, ARP, DNS, TCP, TLSv1.3
- Koska tarkoitus on oppia, lähdin selvittämään tuota QUIC ja TLSv1.3 lisää.

    *QUIC on siis vähän kuin "päivitetty" versio TCP - protokollasta, joka   vähentää kättelyitä ja täten nopeuttaa yhteyksien muodostamista. Se myös estää "head-of-line blockingia" sillä, että se tukee useita yksittäisiä streameja yhdessä yhteydessä, kun TCP käsittelee ne peräkkäin. Se on suorituskyvyltään parempi, kun TCP, mutta siinäkin on pari ongelmaa.
    1. Pakettien virheellinen saapumisjärjestys - tulkinta: pakettihävikki
    2. Suorituskyvyn heikentyminen mobiililaitteilla kuormituksen
       vaikutuksen johdosta
  * TLSv1.3 on tietenkin TLS - protokolla ja se on päivitetty, turvallisempi     vaihtoehto 1.2 verrattuna.
  * ![image](https://github.com/user-attachments/assets/ad134b90-366d-45b5-814d-67f65f5a8d7a) (Kuva 1)

## f) Vapaaehtoinen, vaikea: Mitä selainta käyttäjä käyttää? surfing-secure.pcap (Päivitys 2025-03-31 w14 ma - muutin tehtävän vapaaehtoiseksi Giang:n suosituksesta)

/// Lähdin tekemään ennen tuota vapaahetoista lisäystä, mutta koin sen itse hyödyllisenä oppimisen kannalta ///
  
- Tätä on tullu pähkäiltyä nyt vähän jo liian pitkään. Kokeilin ```tls.handshake.type == 1```, suodattaa ```dns``` ja vaikka mitä muuta löytämättä kuitenkaan oikeaa vastausta.
  
- Tässä kohtaa lähdin todella syviin uriin etsimään tuota selainta. Niin syvälle, että lähteiden merkitseminen ja jokaisen vaiheen kirjaus tuottaisi tästä koko paketista niin pitkän, ettei sitä kukaan jaksaisi lukea. Lopputuloksena päädyin uudelleen tuohon QUIC - kohtaan ja sain selville, että lähtökohtaisesti Google Chrome sekä Edge käyttävät QUIC - protokollaa ja päädyn Chromeen, sillä käyttäjän ensimmäinen Query on www.google.com. Tässä toki vähän ristiriidassa on se, että voihan käyttäjä vain lisätä selaimen ensimmäiseksi sivuksi googlen ja tätä kautta ensimmäinen haku on siihen. Myös FireFoxilla ja Safarilla on siihen jo tuki, mutta se ei ole defaulttina päällä.


## g) Minkä merkkinen verkkokortti käyttäjällä on? surfing-secure.pcap

- Lähdin liikkeelle tästä ARP - kyselystä
  ![image](https://github.com/user-attachments/assets/56bda1f2-4634-4dfc-8cf8-bca78a488d55)
- Siitä saatiin MAC - osoite. Verkkokorttihan oli 3 ensimmäistä tavua
- Kyseessä on Oracle VirtualBox 5.2 + Vagrant
- https://macaddress.io/faq/how-to-recognise-an-oracle-virtual-machine-by-its-mac-address

## h) Millä weppipalvelimella käyttäjä on surffaillut? surfing-secure.pcap Huonoja uutisia: yhteys on suojattu TLS-salauksella.
Käyttäjä on surffaillut ```terokarvinen.com```, ```gc.zgo.at``` ja ```terokarvinen.goatcounter.com```
  
![image](https://github.com/user-attachments/assets/18b76a1d-fa9a-4039-8430-60f98ff5d06b)


## i) Analyysi. Sieppaa pieni määrä omaa liikennettäsi. Analysoi se, eli selitä mahdollisimman perusteellisesti, mitä tapahtuu. (Tässä pääpaino on siis analyysillä ja selityksellä, joten liikennettä kannattaa ottaa tarkasteluun todella vähän - vaikka vain pari pakettia. Gurut huomio: Selitä myös mielestäsi yksinkertaiset asiat.)

- Tein jo aika teknistä analyysiä aiemmin, joten ajattelin nyt enemmän keskittyä näihin perusasioihin
- Tein niin, että valmiista duckduckista kirjoitin hakusanaksi "what is happening" ja stoppasin tallentamisen, kun sivu oli valmis.

- ![image](https://github.com/user-attachments/assets/c0d43e6e-1aaa-4c02-9c13-3a416e63c566)

- Tarkkailin samalla sitä, mitä Wiresharkin sisällä tapahtuu ja jokaista kirjoitusta kohtaan tuli yksi tapahtuma. Eli se käytännössä kirjasi ylös jokaisen syöttämäni asian omalle rivilleen (siksi niitä on paljon). Alleviivattu kohta myös näyttää sen, että Hyper Transfer Protocollaa on käytetty, joka vahvistaa osaltaan sen, että kyseessä oli tekstinsyöttöä.
- Samalla näkyy se, mistä osoitteesta lähdetään ja mihin mennään, eli 10.0.2.15 -> 20.43.161.105, joista ensimmäinen IP on VirtualBoxin ja toinen DuckDuckGon
- Näkyy portit 34730 ja vastaanotto 443
- TLS 1.2 versio ja IPv4

- ![image](https://github.com/user-attachments/assets/8666630d-0412-469c-9556-fa0926ade54f)
- Tästä kuvasta taas näkee hyvin sen, koska paketti on lähtenyt, kauanko sillä on kestänyt ja 
aika, milloin se on tapahtunut. Mielenkiintoinen yksityiskohra tuo kellonaika, sillä se nähtävästi ottaa huomioon myös aikaerot.
- Interface id on se, jota kaappaamme eli eth0
- Ja 1184 bittiä lähti lentoon

![image](https://github.com/user-attachments/assets/49990c15-f6d9-4e7f-9f61-6f54e8c41722)

- Tässä kohdassa Serveri on vastaanottanut yhteyspyynnon ja aloittaa kättelyn. Se sisältää tarvittavat salaukset, jotta jatkossa käynnistyvä viesti salataan
- Se myös Hellolla kertoo näkevänsä tuon syötetyn haun. Itse asiassa jouduin tekemään tämän uudelleen, sillä olin malttamaton ja katkaisin aiemmmin tuon kaappauksen, jonka takia voi näkyä pientä muutosta IP - osoitteissa.

- ![image](https://github.com/user-attachments/assets/e1700a08-5655-41bb-ab9a-544a3f17c3f6)
- Tässä puolesatan näkyy se, miten serveripuolelta lähtee tieto eteenpäin takaisin käyttäjälle, identifikaatiot jne.
-  Tässä vielä selvennettynä serveriltä koneelle:
-  ![image](https://github.com/user-attachments/assets/de5207fa-94a3-49d0-9c9d-d1672bebd016)



### Miten meni?
- Ihan nopeasti tähän loppuun omia ajatuksia. Oli aika kuluttava harjoitus, mutta hyvä sellainen. Ei oikeastaan aikaisemmin ole tullut käsiteltyä Wiresharkkia näin laajasti ja sen käyttö on muutenkin ollu todella vähäistä. Tuntu, että meni moni asia vähän ehkä aiheen vierestä tai, ettei kaikki vastaukset välttämättä ollut ihan täysin sitä, mitä oli haettu. Tehtävät oli kuitenkin hyvät ja näiden tiimoilta tuli käytyä kyllä todella monta eri lähdettä läpi ja "pakottaa" samalla oppimaan asioita lisää. Verkkojen toiminnassa tarvitsen itse vielä lisää teorian osaamista, jota pyrin tässä jatkuvasti samalla kehittämään.
- Eteenpäin siis!

### Lähteet

- Verkkoon tunkeutuminien ja tiedustelu, Tero Karvinen (kotitehtävät), Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/, Luettu 31/03/2025
  
- Wireshark - Getting Started, Tero Karvinen (03/2025), Luettavissa: https://terokarvinen.com/wireshark-getting-started/, Luettu 30/03/2025

- Network Interface Names on Linux, Tero Karvinen (03/2025), Luettavissa: https://terokarvinen.com/network-interface-linux/, Luettu 30/03/2025

- Internet Protocol Suite, wikipedia (edit 21/03/2025), Luettavissa: https://en.wikipedia.org/wiki/Internet_protocol_suite, Luettu 30/30/2025

- Easy Web analytics. No tracking of personal data, GoatCounter, Luettavissa: https://www.goatcounter.com/, Luettu 31/03/2025

- QUIC, Wikipedia (edit 26/03/2025), Luettavissa: https://en.wikipedia.org/wiki/QUIC, Luettu 31/03/2025

- QUIC vs. TCP-Development and Monitoring Guide, catchpoint, Luettavissa: https://www.catchpoint.com/http2-vs-http3/quic-vs-tcp, Luettu 31/03/2025

- Why use TLS 1.3?, Cloudflare, Luettavissa: https://www.cloudflare.com/learning/ssl/why-use-tls-1.3/, Luettu 31/03/2025

- Kuva 1: https://www.a10networks.com/wp-content/uploads/differences-between-tls-1.2-and-tls-1.3-full-handshake.png


