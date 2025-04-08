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


## b) Nmapped. Porttiskannaa oma weppipalvelimesi käyttäen localhost-osoitetta ja 'nmap -A' päällä. Selitä tulokset. (Pelkkä http-portti 80/tcp riittää)
- Napataan ensin netti pois päältä. Apache toimii silti paikallisesti

- ![image](https://github.com/user-attachments/assets/508889f4-3b89-4a92-b23c-da61a804cf4c)

- Suoritetaan komento ```nmap -A localhost```
- Näkyy, että ollaan haettu localhost osoitetta 127.0.0.1
- 80/tcp portti auki http yhteydelle ja, että se kuuluu Apachelle
- Näkyy myös se, että mitä title ja header sisältää, eli voi tehdä jo havaintoja siitä, mitä siellä on
- Network hops 0, eli varmistaa sen, että toimitaan paikallisesti
- Lopussa vielä siitä varmistus, että yksi osoite skannattiin ja se kesti 9.06 - sekuntia

## c) Skriptit. Mitkä skriptit olivat automaattisesti päällä, kun käytit "-A" parametria? (Näkyy avoimien porttinumeroiden alta, http-blah, http-blöh...).
- Komennolla ```nmap man```löytyy selitys, joka kaikessa yksinkertaisuudessaan skannaa OS, version, scriptit ja tracerouten
- ![image](https://github.com/user-attachments/assets/8adfd2aa-8877-4429-bc2f-f75c51938338)
- Vähän avasin sitä jo edellisessä tehtävässä, mutta päällä olivat se Apachen "tervetulosivu" eli Default Page, joka toimii ja serverin otsikko
- Eli http-title ja http-server-header - skriptit

## d) Jäljet lokissa. Etsi weppipalvelimen lokeista jäljet porttiskannauksesta (NSE eli Nmap Scripting Engine -skripteistä skannauksessa). Löydätkö sanan "nmap" isolla tai pienellä? Selitä osumat. Millaisilla hauilla tai säännöillä voisit tunnistaa porttiskannauksen jostain muusta lokista, jos se on niin laaja, että et pysty lukemaan itse kaikkia rivejä?

- Olen jälleen /var/log/apache2 - kansiopolussa ja suoritan komennon ```cat access.log```.
- nmap löytyy monesta kohtaa ja näkyy myös se, mitä pyyntöjä se on tehnyt
- Pisti silmään ensimmäisinä nuo /robots.txt, /.git/HEAAD, /favicon.ico
- Ilmeisesti nmap käyttää /robots.txt - tiedostoa hakemaan tiedot siitä, mihin paikkoihin on pääsy ["some may even use the robots.txt as a guide to find disallowed links and go straight to them"](https://en.wikipedia.org/wiki/Robots.txt) ja tässä tapauksessa, kuten Wikipediassa mainitaan, voi käyttää tuota tiedostoa löytääkseen linkkejä, joihin ei ole pääsyä ja hakea niihin tietoa
- Aluksi luulin, että tuo "nmaplowercheck" liittyisi jotenkin siihen, että se hakisi tietoa joko osoitteista tai sivuston tiedoista pienillä kirjaimilla, mutta siinä nyt ei ollut kauheesti järkeä. Päädin etsimään, mitä se oikeasti tarkoittaa. Lopulta en oikeastaan löytänyt mitään muuta, kuin sen, että se tekee pyyntöjä esimerkiksi tuon 404 - tarkistuksen ja katsoo, mitä sivusto palauttaa.
- [/.git/HEAD](https://nmap.org/nsedoc/scripts/http-git.html) - tarkistaa Git repon sivuston juuridokumentin ja hakee siitä niin paljon tietoa, kuin mahdollista
- [/favicon.ico](https://nmap.org/nsedoc/scripts/http-favicon.html) - Hakee "faviconin", eli yleensä sen pienen kuvakkeen selaimen välilehdellä, vaikkapa GitHubin pienen kissan ![image](https://github.com/user-attachments/assets/cb9a0e64-aa73-427e-850a-dd8753a0c7c3)
, vertaa sitä tietokantaan tunnetuista verkkosivujen kuvakkeista ja tunnistaa täten sen nimen

- Saman olisi nähtävästi helpommin saanut Teron vinkeillä, eli komennolla ```grep -ir "nmap"```
- Käsittääkseni se on myös vastaus tuohon, että millä voisit tunnistaa porttiskannauksen jostain muusta lokista ja sen voi putkittaa vaikka | less
- Toinen vaihtoehto olisi suodattaa esim. IP:n perusteella tai säätää palomuuriasetuksia
- Tähän on myös valvontatyökaluja


## e) Wire sharking. Sieppaa verkkoliikenne porttiskannatessa Wiresharkilla. Huomaa, että localhost käyttää "Loopback adapter" eli "lo". Tallenna pcap. Etsi kohdat, joilla on sana "nmap" ja kommentoi niitä. Jokaisen paketin jokaista kohtaa ei tarvitse analysoida, yleisempi tarkastelu riittää.

Käynnistellään Wireshark komentoriviltä ```wireshark```
- Valitaan Loopback adapter ![image](https://github.com/user-attachments/assets/f8f17d3f-b171-47b5-b558-214608379129)
- Uusi komentorivi ja siihen ```sudo nmap -A localhost```
- Wiresharkissa alkaa tapahtua
- ![image](https://github.com/user-attachments/assets/e39c6984-99a2-451b-abe2-e8c05aa0ccf1)
- Tässä etsin saman esimerkin aikaisemmasta, eli tuosta ./git/HEAD - kohdasta, jossa näkyy elvästi User-Agenttina tuo Nmap Scripting Engine
- Seurasin samalla tuota URLia, joka vei tänne
- ![image](https://github.com/user-attachments/assets/7af3e655-ef7f-4c60-bfbe-f0d832640b28)
- Mietin siis, että tunnistaako nmap pelkästään jo tuosta ilmoituksesta, että kyseessä Apache?
- Mutta hakee kuitenkin niitä Gitin hakemistoja löytämättä kuitenkaan mitään
- User - Agent käytännössä edusstaa ihmistä, mutta Web - kontekstissa. Se antaa nettisivulle tietoon, kuka hakee tietoa.
- Päätin tässä välissä tallentaa sen, ettei tule ihmeempiä tapahtumia
![image](https://github.com/user-attachments/assets/283792fc-57b4-4bdd-9011-df29341f92fe)

- Laitoin filtterin kohtiin, jossa on "nmap". Se siis etsii kaikki paketit, joka sisältää tekstin "nmap"
- ![image](https://github.com/user-attachments/assets/69ab20e0-6eb0-4676-8014-4eb8465efa4a)
- Täällä siis näkyy jo aiemmin tarkastellut kohdat, jossa ajetaan scriptejä ja joka sisältää tekstin "nmap"

f) Net grep. Sieppaa verkkoliikenne 'ngrep' komennolla ja näytä kohdat, joissa on sana "nmap".
- Tähän Tero oli ystävällisesti vinkannut jo komennon ```sudo ngrep -d lo -i nmap```
- Piti kuitenkin manuaalista tutkita, mitä se tekee
- ```-i``` ei kiinnosta, onko sana TERO, tero vai TeRo, se hakee sen joka tapauksessa. Tässä tapauksessa sitä ei kiinnosta, onko nmap MiTeN kirjoitettu
- ```-d lo``` pistää ngrepin kuuntelemaan loopback interfacea
- Ajoin komennon sisään, toisella terminaalilla jälleen ```sudo nmap -A localhost```ja pistin huvikseen vielä Wiresharkin päälle

![image](https://github.com/user-attachments/assets/b097004f-976a-4ce9-87bf-e96a828cacc0)

![image](https://github.com/user-attachments/assets/7cc7046c-810f-421e-8988-1e0a75252dad)


- Paketteja tuli siis 2348 ja 25 niistä vastas noihin arvoihin, jotka syötettiin eli sisälsi nmap.
- Ihan mielenkiinnosta Wiresharkilla tuli sama tulos

![image](https://github.com/user-attachments/assets/9073ce9e-7b98-4afe-bf1c-ebfec3962509)

## g) Agentti. Vaihda nmap:n user-agent niin, että se näyttää tavalliselta weppiselaimelta.
- Hetken meni miettiessä, että mitä tässä tarkoitetaan... Onneksi törmäsin tohon jo aikaisemmin, kun selailin enemmän noista User-Agenteista. 

## Lähteet:

-User-Agent, mdn web docs_, Luettavissa: https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/User-Agent, Luettu 08/04/2025
- robots.txt, Wikipedia, (Rewieved 16/03/2025), Luettavissa: https://en.wikipedia.org/wiki/Robots.txt, Luettu: 08/04/2025

wireshark-filter(4) Manual Page, Luettavissa: https://www.wireshark.org/docs/man-pages/wireshark-filter.html, Luettu 08/04/2025
