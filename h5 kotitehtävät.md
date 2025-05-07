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
- Tota ruy-manager - komentoa ei ilmeisesti tarvitse ajaa. 
