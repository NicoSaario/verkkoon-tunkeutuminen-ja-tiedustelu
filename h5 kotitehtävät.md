# Kotitehtävät 

Kotitehtävät ovat kurssilta "Verkkoon Tunkeutuminen ja Tiedustelu - Network Attacks and Reconnaissance" ja löytyvät osoitteesta https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#verkkojen-perusteet 

- Nämä tehtävät suoraan Lari Iso-Anttilalta https://github.com/lisoant

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 07/05/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 07/05/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)
- Tässä myös Debian 12 Bookworm, koska tuo Kali ajoi hetkeksi sopimuksen irti

## a) Tutustu seuraavaan työkaluun

    https://github.com/kgretzky/evilginx2
    Vastaa seuraaviin kysymyksiin
        Asensitko työkalun, jos asensit niin kirjoita miten sen teit.
        Mitä teit työkalun kanssa?
        Onnistuitko huijaamaan liikennettä
- Puuhastelen vanhan kurssin Debian 12 Bookwormilla Oracle VM - kautta
-  Latasin sen ensin ```git clone https://github.com/kgretzky/evilginx2```, jonka jälkeen hypin vähän kansioiden sisällä ja tutkin, mitä kaikkea siellä on
- Tämän jälkeen katsoin open-source - dokumentaatiosta [täältä](https://help.evilginx.com/community) sen, että siihen tarvii riippuvuudet asentaa -> Asensin Golang -> ```sudo apt-get install golang```
- Asennuksen jälkeen komento ```make```
- Löin sen vain testimuodossa käyntiin ![image](https://github.com/user-attachments/assets/5136616d-9cd9-461d-97f8-eb538f3a06d7)

Edit = 07/05/2025 klo 20:00 - Nyt jatkuu tämän kanssa pähkäily, kun on tuo mininet saatu hoidettua pois alta

Ilmeisesti tätä on hieman hankala testata pelkällä localhostilla ja se vaatii yhteyden verkkoon, joten käyn tutkimassa, josko saisin jonkun palvelimen pystyyn

## b) Sinulla on käytössäsi mininet ympäristö. Luo ympäristö, jossa voit tehdä TCP SYN-Flood hyökkäyksen.

    Kirjoita miten loit mininet ympäristön ja miten toteutit hyökkäyksen.
Olin siis ladannut jo virtuaalikoneen ja luonut aloitusympäristön ennen tunnin aloitusta.


Ensin piti saada labrakansiot imuroitua mininettiin hostilta:
- Käytin Git Bashia Windowsilla ja otin ssh - yhteyden mininettiin ```ssh mininet@-ip-```
- Kun SSH oli otettu, avasin uuden terminaalin, menin hakemistoon, jossa nuo "labs" sijaitsevat ja käytin komentoa ```scp labs mininet@-ip-:/home/mininet/Downloads```

![image](https://github.com/user-attachments/assets/f82b5b24-b3bb-4005-8937-e9415007def0)

Siellä ne ovat!

- Aloitetaan. Käytän tässä lisoantin kalvoa ja ChatGPT varmistamaan komentojen tarkoituksen
- Ensimmäisenä käynnistetään verkko-ohjain ja mininet. Tässä käytetään siis Mininetin omaa topologiaa eikä ole yhteyksiä ulkopuolelle ```ruy-manager ruy.app.simple_switch_13``` ```sudo mn --topo single,3 --mac --switch ovsk --controller remote```
- Sitä ennen pitääkin ajaa ```sudo loadkeys fi```, jotta saan näppäimistöasettelun oikeaksi
- Hetken piti ihmetellä, että miten ihmeessä saan toisen terminaalin auki, kun nyt on vain näkyvissä tämä

![image](https://github.com/user-attachments/assets/a061233c-723d-4595-b723-7c668798a574)

- Naputtelin kaikkia nappeja vuoron perään ja lopputuloksena ```Ctrl + Alt + F2``` avaa uuden ja vastaavasti ```Ctrl + Alt + F1```palauttaa takaisin
- Hyvä esimerkki siitä, ettei kannata yrittää tehdä demon perässä juttuja, vaan katsoa, kuunnella ja tehdä muistiinpanoja
- Nyt suoritettiin tuo seuraava komento

![image](https://github.com/user-attachments/assets/b4d54c31-54f0-40da-adf2-806047682eb3)

- Ilman tuota ruy-manager - komentoa kaikki pingit dropattiin
- Se on siis esimerkkisovellus, joka käyttäytyy Ethernet-kytkimen tapaan (ChatGPT)

![image](https://github.com/user-attachments/assets/e7ae362e-72bd-413f-8ebe-e948eba1a8ed)

- Törmäsin ongelmaan, jossa en voi käyttää ```xterm h1```, koska "Cannot connect to display". Eli se tarvitsee graafisen käyttöliittymän (?)
- Tästä oli itseasiassa jo puhetta aikaisemmin, joten lähden selvittelemään, mistä sen saa korjattua
- Lari oli tehnyt ohjeet sen ratkaisemiseen -> Ajetaan scripti ./get_xauth.sh, josta saadaan haetaan Magic-Cookie ja lyödään seuuraavan komennon perään ```sudo -s xauth add mininet-vm/unix:10```
- Vaihdoin samalla takaisin SSH - yhteyteen. Ei tuosta mininetin käytöstä tullut mitään VMwaressa
- Tein kaikki moneen kertaan, mutta xterm herjaa silti "Error: Cannot connect to display"
- Noh.. tähän hätään sain ChatGPT:ltä ohjeeksi käyttää komentoa ```h1 hping3 -S --flood -p 80 10.0.0.2```, joka kaatoi ruyn tuolta taustalta

- Siinä siis ```h1```suorittaa sen h1 hostilla, ```hping3```on työkalu, jolla lähetetään paketteja käsin,  ```-S``` lähettää SYN-paketteja, ```--flood```lähettää niin nopeasti, kuin mahdollista ja ```-p 80``` tarkoittaa porttia
- 

![image](https://github.com/user-attachments/assets/107b698b-7588-4e54-915c-78b83bc5f129)

- Tehdään vielä uudelleen ja seurataan, mitä tapahtuu

![image](https://github.com/user-attachments/assets/227d4e2f-b713-409a-9363-8d888da03b50)

- Tässä siis pingit mennyt läpi.

- Ja jälleen floodin jälkeen ruy alhaalla

![image](https://github.com/user-attachments/assets/af3afafb-e7a0-4f70-bb20-8dfe3202fa14)

Sanoisin tässä kohtaa, että testi on onnistunut. Pitää kyllä koittaa vielä säätää tuo Xterm kuntoon jossain kohtaa.


## Lähteet

ChatGPT - komentojen analysoijana

Larin kalvot

How to Perform TCP SYN Flood DoS Attack & Detect it with Wireshark - Kali Linux hping3, firewall.cx, Luettavissa: https://www.firewall.cx/tools-tips-reviews/network-protocol-analyzers/performing-tcp-syn-flood-attack-and-detecting-it-with-wireshark.html, Luettu 07/05/2025

Getting Started, Evilginx Documentation, Evilginx, Luettavissa: https://help.evilginx.com/category/getting-started, Luettu 07/05/2025
