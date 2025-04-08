# Kotitehtävät

Kotitehtävät ovat kurssilta "Verkkoon Tunkeutuminen ja Tiedustelu - Network Attacks and Reconnaissance" ja löytyvät osoitteesta https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#verkkojen-perusteet 

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 30/03/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 30/03/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)

# h2
> h2 Taustaa

> Tässä taustaosiossa olevia linkattuja artikkeleita ei tarvitse lukea tai tiivistää, ellei tehtävissä erikseen pyydetä.

> Tämän tehtävän tavoite on tunnistaa hyökkäystyökaluja; ja välttää tunnistamista.

> Mitään ulkopuolisia koneita ei saa porttiskannata. Irrota tarvittaessa koneet Internetistä testien ajaksi. Kohdista kaikki hyökkäykset ja porttiskannaukset vain annettuihin harjoitusmaaleihin. Tehtävän saa aloittaa vasta, kun on hyväksynyt kurssin säännöt.

> Kybertappoketjusta (kuva, artikkeli) opimme, että hyökkääjä altistuu tunnistamiselle hyökkäyksen jokaisessa vaiheessa. Työkalut jättävät jälkiä, joista hyökkäyksen voi tunnistaa.

> Itse hyökätessämme voimme asentaa laboratorioon puolustajan järjestelmää muistuttavan ympäristön. Silloin voimme säätää hyökkäystyökalut vaikeasti tunnistettaviksi.

> Tässä harjoituksessa

    Tunnistetaan hyökkääjän porttiskannaus
    Kilpavarustellaan - hyökkääjä piiloutuu, mutta riittäkö se?
    Muokataan hyökkäystyökalua väistämään tunnistusta
    Arvioidaan omia hyökkäystyökaluja kahdesta näkökulmasta
        Verkkoliikenne - yleinen tapa
        Kohdejärjestelmän loki - erityinen, tiettyyn puolustajaan sovellettu

> Läheiset käsitteet (taustatietoa, ei tarvitse tiivistää eikä lukea kokonaan)

    Tuskan pyramidi (Bianco 2013: Pyramid of Pain)
    Kybertappoketju (Hutchins et al 2011: Cyber Kill Chain)
    Timanttimalli (Caltagirone et al 2013: Diamond Model)
    MITRE ATT&CK (Mitrellä jäi caps-lockki JUMIIN)

# h2 Tehtävänannot
Kaikissa kohdissa edellytetään analyysia ja selitystä. Selvitä ja selitä siis komentojen ja komentoriviparametrien merkitys. Selitä, mitä tulokset tarkoittavat; ja mitä niistä tulisi ymmärtää. Lue vinkit, ennenkuin aloitat.

    x) Lue ja vastaa lyhyesti kysymyksiin. Tässä alakohdassa x ei tällä kertaa tarvitse lukea artikkeleita kokonaan, ei tarvitse tiivistää niitä, eikä tehdä testejä koneella.
        Selitä tuskan pyramidin idea 1-2 virkkeellä. Bianco 2013: Pyramid of Pain. (Katso eritoten pyramidin kuvaa.) https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html
        Selitä timanttimallin (Diamond Model) idea 1-2 virkkeellä. Tekijä esittelee sen aika juhlallisesti, voit myös etsiä yksinkertaisempia artikkeleita hakukoneella tai kelata suoraan timantin kuvaan. Caltagirone et al 2013: Diamond Model

Selitä tuskan pyramidin idea 1-2 virkkeellä. Bianco 2013: Pyramid of Pain. (Katso eritoten pyramidin kuvaa.) https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html
- Käytännössä tuskan pyramidi tarkoittaa sitä, että mihin asioihin kannattaa kiinnittää huomiota, jotta aiheuttaisi hyökkääjälle kaikista eniten päänvaivaa ja täten ostaa itselle aikaa tai vaihtoehtoisesti saada hyökkääjä luopumaan tavoitteistaan kokonaan.
- On huomattavasti tehokkaampaa lamauttaa hyökkääjä kohdistamalla turvamenetelmät niin, että hyökkääjä joutuu keksimään vaihtoehtoisia tapoja ja käyttää aikaansa siihen, että hän joutuu keksimään jotain uutta sen sijaan, että puolustaa niitä asioita vastaan, joita on helppo valvoa ja johon valmiita työkaluja löytyy laajalti.

Selitä timanttimallin (Diamond Model) idea 1-2 virkkeellä. Tekijä esittelee sen aika juhlallisesti, voit myös etsiä yksinkertaisempia artikkeleita hakukoneella tai kelata suoraan timantin kuvaan. Caltagirone et al 2013: Diamond Model https://www.threatintel.academy/wp-content/uploads/2020/07/diamond-model.pdf
- Timanttimalli koostuu neljästä avainkohdasta (adversaries, infrastructure, capabilities and targets) sekä niiden suhteista keskenään. Malli on hyvä visualisointityökalu ymmärtämään eri hyökkäysskenaarioiden vaikutusta ja yhdistämään suhteiden sekä elementtien vaikutusta toisiinsa.
- Vaikka lueskelin koko mallin, käytin täydentävänä lähteenä 
Diamond Model of Intrusion Analysis: What, Why, and How to Learn, David Tidmarsh (11/2023), Ethical Hacking, https://www.eccouncil.org/cybersecurity-exchange/ethical-hacking/diamond-model-intrusion-analysis/

## a) Apache log. Asenna Apache-weppipalvelin paikalliselle virtuaalikoneellesi. Surffaa palvelimellesi salaamattomalla HTTP-yhteydellä, http://localhost . Etsi omaa sivulataustasi vastaava lokirivi. Analysoi yksi tällainen lokirivi, eli selitä sen kaikki kohdat. (Jos Apache ei ole kovin tuttu, voit tätä tehtävää varten vain asentaa sen ja testata oletusweppisivulla. Eli ei tarvitse tehdä omia kotisvuja tms.)

- Kaikki alkaa aina ```sudo apt-get update```
- Tämän jälkeen ```sudo apt-get install -y apache2``` ja ```sudo systemctl start apache2``` ```sudo systemctl status apache2```
- Apache2 valmiiksi Kaliin asennettuna
- ![image](https://github.com/user-attachments/assets/e4fa8e84-b255-4e5a-89e7-1f6a88463314)
- Nähdään tuolla systemctl status - komennolla, ettei se ole päällä. Pistetään se päälle:

- ![image](https://github.com/user-attachments/assets/a1f81d5e-3a5d-4b84-b2a1-21be27c99228)
- Voidaan ottaa ```curl localhost``` ja nopeella vilkaisulla nähdä, että se toimii "It works!" ja Apachen esimerkkisivu on aktiivisena.
- ![image](https://github.com/user-attachments/assets/dc53961f-e0fe-4438-bd0b-1d83f92967e9)
- Tässä surffataan selaimella osoitteeseen ```http://localhost``` eli salaamattomalla HTTP - yhteydellä palvelimelle.
- ![image](https://github.com/user-attachments/assets/2b2adfa1-5bab-4fe6-991d-a41fc6b67f40)
- Seuraavaksi tutkitaan logeja. Logit löytyvät ```var/log/apache2/``` - kansiopolusta
- Loput journalctl, jonka kuvaus ja tarkoitus näyttävästi löytyy ```man journalctl```
- ![image](https://github.com/user-attachments/assets/39adef26-efd4-4090-aa60-021e85602f49)

- Käytännössä tän voi tehdä kahdella tavalla: Itse ensin navigoin kansioon, jossa access.log - tiedosto on ja käytin ```cat acces.log```
- Teron vinkeistä löytyy ```sudo tail -F /var/log/apache2/access.log```. Käytännössä ne tekee muuten samaa, mutta kuten kuvankaappauksesta näkee, päivitin localhost - sivun ja se näyttää "livenä" sen, mitä tapahtuu, eli seuraa sitä logivirtaa jatkuvasti
- ![image](https://github.com/user-attachments/assets/88d8d4a3-98af-445e-a9a6-acc4933612d4)
- Analysoin tätä ensimmäistä riviä ![image](https://github.com/user-attachments/assets/2768dd0c-0881-4971-915f-fac2eaee70b6)
- Aluksi näkee kellonajan, millon logi on kirjattu eli 08/4/2024 kello 14:28:07 ja UTC +3, eli Helsingin aikavyöhykkeellä mennään
- ```*GET / HTTP/1.1*``` on hakupyyntö, jossa pyynto on tehty / eli juurisivulle, ```HTTP/1.1``` on protokolla, jota käytetään
- ```200``` On [statuskoodi](https://umbraco.com/knowledge-base/http-status-codes/) ja tarkoittaa onnistunutta tapahtumaa
- ```10958```on käsittääkseni vastauksen koko
- Ja curl... on se, mitä työkalua on käytetty pyyntöön, eli tässä on se ensimmmäinen testipyyntö, jonka lähetin käyttämällä ```curl localhost```





