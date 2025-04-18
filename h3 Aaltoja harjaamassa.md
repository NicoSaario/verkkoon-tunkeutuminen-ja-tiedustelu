# Kotitehtävät

Kotitehtävät ovat kurssilta "Verkkoon Tunkeutuminen ja Tiedustelu - Network Attacks and Reconnaissance" ja löytyvät osoitteesta https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#verkkojen-perusteet 

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 15/04/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 15/04/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)


## x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

### Hubacek 2019: [Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs](https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s) (Video, alkaen 3:19 ja päättyen 7:40. Yhteensä noin 4 min.)

- Taajuuden oikeaoppisuus on hyvä tarkistaa ensin, esimerkiksi Spectrum Analyzer - työkalulla
- Tarvitaan vain Frequency
- Samaisella työkalulla nähdään se, miten lähetys tallentuu
- Maalaamalla lyhyimmän viestin voi selvittää Bit Lenghtin, jos sitä ei automaattisesti tunnisteta


### Cornelius 2022: [Decode 433.92 MHz weather station data](https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html)

- Tässä tutkitaan sääasemaa, joka näyttää lämpötilan ja kosteuden sisälle sekä jopa kolmeen langattomaan sensoriin. Tarkoituksena myöhemmin rakentaa löydöksistä oma sensori, joka oli hajalla.
- Lähtökohtana ainoastaan taajuus 433.92 MHz.
- Käytetty decoderia (rtl_433), joka decodesi sen lämpötilan ja ilmankosteuden sekä sai selville sen olevan Nexus-TH.

URH - [Universal Radio Hacker](https://github.com/jopohl/urh)

- Voi analysoida, muuttaa ja uudelleenlähettää mitä tahansa signaalia

1. Spectrum Analyzer
   - SDR device - aseta vastaanottava taajuus ja "gain"
   - Odota signaalia
   - Nosta "gain", jos ei hetkeen näy mitään ja laite lähellä sekä lähettämässä RF
   - Ota ylös Frequency ja Gain, kun näkyy
   - Käytä spectrogrammia löytääksesi oikean signaalin taajuuden

2. Luo uusi projekti
   - Bandwith ja Sample rate voi nostaa 1M, koska signaali on pienikaistainen
   - Vain yksi osallistuja ("Participant") tai poista molemmat

3. Nauhoita käyttäen File - menua
  - Taajuus vähän ohi, eli 20..100 kHZ siitä signaalista, joka saatiin aikaisemmin
  - Tärkeää nauhoittaa tarpeeksi pitkään, jotta saadaan ainakin kaksi rypästä
  - Tallenna, tyhjennä jos ei mitään
  - Amplitudesignaalin pitäisi olla vähintään 2x noise amplitude
  - Drägää nauhoitus vasemmalta Interpretatiton - lehdelle
  - Kannattaa katsoa ensin aika kahden onnistuneen lähetyksen välillä tai se, kuinka usein sensori lähettää dataa
  - Create signal from selection

4. Demodulointi
   - AM = ASK (amplitude shift keying)
   - HUOMIO: URHissa näkyvä frequency ei ole oikea signaalin taajuus! Tärkeä ainoastaan FSK - lähetyksille
   - Samples/Symbol = Bitti modulaatiossa -> Bitti dataa edustaa symbolien määrä -> Useimmilla PDM järjeselmillä kaantoaaltopurkauksen kesto vastaa yhtä symbolia, kun seuraavat välit ovat sen monikertoja
   - Jos lähetyksesä tauko - modulaatio-yhdistelmäruudun viereinen asetuspainike -> Pause threshold
   - Tarkista noise level

5. Digital analysis
   - Edit -> Decoding
   - Signaali pitäisi kääntää -> Morse Code



### Vapaaehtoinen, vaikeahko: Lohner 2019: Decoding ASK/OOK_PPM Signals with URH and rtl_433

- Tässä decodetaan rtl_433 Signal I/Q Sample Files käyttäen URHia ja rtl_433 decoderia

URH
- Signal view -> Spectrogram -> Korosta signaali, oikeeklikkaus -> Apply Bandpass Filter (kohinan vähentäminen)
- Bit lenght -> joku pieni, esim. 10
- Error Tolerance vielä pienempi, esim. 1
- Valitse läjä ykkösiä ja nollia valitaksesi monta kokonaista symbolia
- Jos on vaikkapa 1512 on näytteiden pituus ja 6 symbolia valittu -> 1512/6 = 252, josta tulee Bit Lenght -> Error Tolerance pieni, esim. 10 -> Pause Threshold 4 (URH pilkkoo signal framet, kun on isoja rypäkkeitä nollia)
- Kehykset, joilla on symbolinen rakenne -> Decodetaan -> Analysis ja ainoastaan se signal
- Eli koska "11111001" on spekulaatiota ja arvailua, luodaan asetuksista se helpommin ymmärrettäväksi. Poistetaan "1111001" - alku ja käännetään se peilikuvaksi. Morse Code ymmärtää signaalia toisinpäin ja säädetään se niin, että lyhyt signaali tarkoittaa yhtä asiaa, pitkä toista
- Sen jälkeen URH näyttää tulokseksi "000101001000" tai 0x148 hex

rtl_433

- URH - analyysistä saatiin pulssien pituusdeksi 252 -> 250k näytettä sekunnissa -> Pulssi tai väli on 1008 mikrosekuntia
- OOK_PPM eli Pulse Position Modulation pitää tietää juuri se arvo. Syötetään tieto nollavälin oletusleveydestä, ykkösvälin oletusleveydestä, nollauskestosta (arvioidaan vähän ylöspäin) ja lopputulos: ```rtl_433 -R 0 -r g001_433.92M_250k.complex16u -X n=SAMPLE1,m=OOK_PPM,s=1008,l=3024,r=3200```
- Sen jälkeen hienosäätöä toleranssin suhteen, jotta saadaan tarkempaa tulosta


### a) WebSDR. Etäkäytä WebSDR-ohjelmaradiota, joka on kaukana sinusta ja kuuntele radioliikennettä. Radioliikenne tulee siepata niin, että radiovastaanotin on joko eri maassa tai vähintään 400 km paikasta, jossa teet tätä tehtävää. Käytä esimerkkinä julkista, suurelle yleisölle tarkoitettua viestiä, esimerkiksi yleisradiolähetystä. Kerro löytämäsi taajuus, aallonpituus ja modulaatio. Kuvaile askeleet ja ota ruutukaappaus. (Tehtävässä ei saa ilmaista sellaisen viestin sisältöä tai olemassaoloa, joka ei ole tarkoitettu julkiseksi. Voit sen sijaan kuvailla, miten sait julkisen radiolähetyksen kuulumaan kaiuttimistasi. Julkisten, esimerkiksi yleisradiolähetysten sisältöä saa tietysti kuvailla.)

- Tutkin sivustoa aika pitkään ihan vain omastakin mielenkiinnosta. Päädyin lopulta http://remoteradio.changeip.org:8073/ - osoitteeseen, jossa painoin ensin "pop-up" - ilmoituksen, jossa kysyttiin, halutaanko audio päälle - painoin laatikon aktiiviseksi. Sivusto sijaitsee siis Bedford, England, Uk, jonne matkaa lähes 3000 km.
- Sen jälkeen aukesi tällainen näkymä: ![image](https://github.com/user-attachments/assets/f8ab06d4-c6a4-4f85-b102-20c519557e61)
- Sisältää siis hyvin paljon liikennettä. Huomiota herätti erityisesti tuo 900 - taajuus, jossa näyttää olevan paljon liikennettä
- Zoomasin siis lähemmäs, asetin itseni sinene (ihan vain klikkaamalla), painoin Modeksi AM ja levitin filtteriä "filter shift - wider", koska ääni vähän pätki välillä. Lopulta asettelin sen manuaalisesti.

![image](https://github.com/user-attachments/assets/ea717fbe-e090-4a00-9b59-f8568149bc53)

- Livessä kuuluu tällä hetkellä jalkapallo - ottelu PSG vastaan Aston Villa. Varmistin sen vielä [sivuilta](https://www.bbc.co.uk/sounds/schedules/bbc_radio_five_live), että näin on

![image](https://github.com/user-attachments/assets/a0556065-64bf-4ca1-955a-6a893f940d66)

- Taajuus 909.04 MHz
- Aallonpituus on 300/909.04 MHz ~ 0,33m pyöristettynä, eli Aallonpituus = 300/Aallonpituus MHz
- Ja modulaatio AM

![image](https://github.com/user-attachments/assets/b8da9069-d306-4b4e-85c1-2857cf3fbcd2)

## b) rtl_433. Asenna rtl_433 automaattista analyysia varten. Kokeile, että voit ajaa sitä. './rtl_433' vastaa "rtl_433 version 25.02 branch..."

- Yritin heti ```sudo apt-get install rtl_433```, mutta se ei toiminut, joten menin https://github.com/merbanan/rtl_433 ja katsoin, että se on ```rtl-433```, joten asensin sen sieltä

- Asennuksen jälkeen katsoin, missä se sijaitsee "whereis" - komennolla, navigoin sinne ja suoritin.
- Vastasi Versiolla, joten se toimii.

![image](https://github.com/user-attachments/assets/89a569b6-3fb6-4556-8258-22428542cf41)


## c) Automaattinen analyysi. Mitä tässä näytteessä tapahtuu? Mitä tunnisteita (id yms) löydät? Converted_433.92M_2000k.cs8. Analysoi näyte 'rtl_433' ohjelmalla.

- Latasin sen Teron sivuilta, suoraan Downloads - kansioon se menee, joten sieltä se on helppo myös ajaa suoraan

![image](https://github.com/user-attachments/assets/7e2248da-48a8-4c87-ac00-b4718ec148cc)

1. Siitä löytyy id, eli 875315
2. House Code, joka on sama.
3. Suojaustoimintoja, kuten Nexa-Security ja Proove-Security sekä ilmeisesti jonkinlainen Plug-ray-osoitin kaukopistokkeella [löytyi täältä](https://klikaanklikuit.nl/product/stekkerdimmerset-100-watt-led/).
4. Group call = No tai OFF
5. Unit 3
6. Dim
7. Kanavat
8. Eri aikoja

![image](https://github.com/user-attachments/assets/aa7b17cb-4847-4e18-bf78-f075b7add6fb)


![image](https://github.com/user-attachments/assets/dbaed6d0-5ef2-4121-9573-0c1870349c1a)

## d) Too compex 16? Olet nauhoittanut näytteen 'urh' -ohjelmalla .complex16s-muodossa. Muunna näyte rtl_433-yhteensopivaan muotoon ja analysoi se. Näyte Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s

- Eli ensin latasin sen [täältä](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/). Koitin ensin suoraan ajaa sitä:

![image](https://github.com/user-attachments/assets/1c95fe79-ea36-45a3-a0bd-4b6d0d826248)

Mitään ei kuitenkaan tapahtunut. Mietin, että riittääkö, jos muuttaa tiedostoformaattia. Hetken yritin etsiä ratkaisua, mutta törmäsin lopulta Teron vinkkeihin, jossa complex16s vaihtuu cs8.

Tehdään siis se:

![image](https://github.com/user-attachments/assets/12350e18-25ce-44f9-bed4-b74ab84003d1)

Tämähän on tismalleen sama tiedosto, mitä analysoitiin aikaisemmin? 

![image](https://github.com/user-attachments/assets/866d161b-18e5-4791-9022-5f2b41b2c039)

Jäi vähän hämmentämään tämä lopputulos, joten aloin varmistelemaan sitä eri tavoilla

![image](https://github.com/user-attachments/assets/da0b73ad-62a7-4f72-a8b4-e7f57c42a2ab)

Näyttää siis, että kyseessä sama tiedosto jota analysoitiin aiemmin.

## e) Ultimate. Asenna URH, the Ultimate Radio Hacker.
[Ohjeen](https://github.com/jopohl/urh?tab=readme-ov-file#Installation) mukaan suositeltu tapa on käyttää pipx, jolla tulee kaikki natiivit laajennukset valmiina. 

Eli ```sudo apt-get install pipx```
Ja ```pipx install urh```
Sain kuitenkin virheilmoituksen, ettei sitä voi asentaa.

![image](https://github.com/user-attachments/assets/30c19b6c-f01c-41bf-b745-438a3d717a4e)

Varmistin vielä ```sudo apt-get upgrade```, että kaikki on varmasti hyvin

Kaikki näyttää olevan kunnossa:

![image](https://github.com/user-attachments/assets/44c8c18d-5a2b-46ca-af07-3d50f63e1eff)

Löysin error - logeista, että Cython puuttuu

![image](https://github.com/user-attachments/assets/2ad6b35d-80a1-4053-af98-d021b308c7c5)

```python3 -m pip install cython```
Yritin myös ```sudo apt-get install cython3```

![image](https://github.com/user-attachments/assets/c3439e2d-4c0f-4b92-8df9-3ca8803bd0f8)


Update: Tästähän tuli melkoinen soppa. Tehtävä näyttää hyvinkin yksinkertaiselta, mutta joskus toteutus voi olla ihan toinen. Noin 2h lokien, surffailun ja pyörimisen jälkeen ollaan vieläkin samassa tilanteessa. Rikoin jopa Kalin paketit käyttämällä ```--break-system-packages``` - komentoa. Ilmeisesti uusi päivitys Kalissa estää suoraa pipx käytön "externally-managed-environment" ja yritän asentaa sen nyt sitten virtualenv kautta.

- Mitä siis kokeilin?
- Asentaa pipx uudelleen, pip jne. python3, siirtelin py3 kirjastoja, yritin asentaa sen käyttämällä ```--break-system-packages```, joka lopulta rikkoi Kalin hetkeksi (Reboot CLI käynnistys, update ei toiminut jne), asensin mahdolliset puuttuvat riippuvuudet (libxml2-dev, libxslt1-dev, zlib1g-dev) ja *vaikka mitä muuta*.

- Tässä kävi nyt niin, että kokeilin myös virtualenvin kautta ja sekään ei toiminut.
- Tein testin Tunkeutumistestaus - koneella, jossa myös Kali Linux. Sama ongelma
- Meni hermo - Siirryin Debian 12 - koneelle, jota käytin aikaisemmalla "Sovellusten hakkerointi ja haavoittuvuudet" - kurssilla ja se lähti laakista toimimaan.

- Homma jatkuu siis nyt vihdoin turhien ponnisteluiden jälkeen jonkun kolmen tunnin venkslaamisen jälkeen
- Käytin siis

   ```
  sudo apt-get install pipx
  pipx install urh
  pipx ensurepath
  ```

![image](https://github.com/user-attachments/assets/6d1a6fd5-1fa2-43d8-8cdb-81bed360334f)

- Siinä lukeekin, että pitää aukaista uusi terminaali tai "re-log"

- Ja siinä on urh auki

![image](https://github.com/user-attachments/assets/cbb9ebeb-941d-4380-9a0e-45649329461f)


## Tarkastele näytettä 1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s. Siinä Nexan pistorasian kaukosäätimen valon 1 ON -nappia on painettu kolmesti. Käytä Ultimate Radio Hacker 'urh' -ohjelmaaa

- Aukeaa siis valikosta "File - Open"

![image](https://github.com/user-attachments/assets/4da5da38-54ca-48e4-972e-683e90555843)


### f) Yleiskuva. Kuvaile näytettä yleisesti: kuinka pitkä, millä taajuudella, milloin nauhoitettu? Miltä näyte silmämääräisesti näyttää?

![image](https://github.com/user-attachments/assets/6d97a288-1c26-4b84-919a-ddc6681630f0)

Tähän siis aukesi seuraava näkymä. Ensimmäisenä tuli mieleen tarkistaa tuolta vähän tietoja lisää. 

Ensimmäisenä lueskelin tuota tiedostonimeä, josta saa selville sen, milloin näyte on tallennettu eli 12/04/2025 ja taajuus, joka näyttäisi olevan 433.912MHz. 

Tämän jälkeen tulee tiedoston koko (10,47MB), Oma tallennusaika eli milloin latasin sen järjestelmään, Samplet joita on huikea 5 491 580 - kappaletta, sample rate 1,0MHz ja aika eli 5,49s. Ihmettelen vain, miksi se on tiedostossa 2MSps ja valittuna 1,0 Sps.

![image](https://github.com/user-attachments/assets/b87d1abf-de3c-414b-a452-7ec00415dcad)

Muutetaan vielä tuo Sample Rate samaan, kuin mitä tiedostossa oli annettu valmiiksi, eli 2,0M Sps.

Kuten näkyy, kesto puolittui: 

![image](https://github.com/user-attachments/assets/e85aa1ba-e609-4abb-b04d-2a65ae937241)

![image](https://github.com/user-attachments/assets/31cc6836-d6cd-4442-9ac7-044d4e5a271e)

Näyte näyttää hyvinkin siltä, joka oli kuvattu - jokainen painallus näkyy omana bittijononaan ja hyvin pitkälti sama rakenne


Jokainen painallus tekee lisäksi samaa asiaa ja on yhtä pitkä

![image](https://github.com/user-attachments/assets/d666befa-5949-42cf-bbc2-ab8f9fcd0fbf)

![image](https://github.com/user-attachments/assets/2a738e1f-8fa4-4f24-8381-2addd6c27e83)



### g) Bittistä. Demoduloi signaali niin, että saat raakabittejä. Mikä on oikea modulaatio? Miten pitkä yksi raakabitti on ajassa? Kuvaile tätä aikaa vertaamalla sitä johonkin. (Monissa singaaleissa on line encoding, eli lopullisia bittejä varten näitä "raakabittejä" on vielä käsiteltävä)

![Näyttökuva 2025-04-16 221135](https://github.com/user-attachments/assets/bbd29c82-27c7-4780-8ca0-aed89bb9cd8f)



- Eli yksi pulssi on 1 bitteinä. Sen kesto on 262,00 µs (262,00 mikrosekuntia [Wikipedia](https://en.wikipedia.org/wiki/Microsecond))
  
- Ei tullut muuta mieleen, joten tein testin: Kuinka kauan kestää täyttää vesilasi, jonka korkeus on  10cm ja pohjan halkaisija 5.5cm ja yläreunan halkaisija 8cm -> tilavuus n. 3.6 (dl). Laskurina toimi ChatGPT, sillä en siihen jaksanut enää aikaa tuhlata.
- Testi toimi niin, että laitoin hanan laakista täysille  vesilasin ollessa suunnattuna suoraa suihkun alla, kunnes se oli kokonaan täynnä. Sitten pysäytin sekuntikellon.
- Eli reaktiovirhemarginaali huomioon ottaen, prosessi kesti noin 1.60s (1.63s kellon mukaan)
- Tässä taas ChatGPT toimi laskurina ja 1.60s / 262 mikrosekuntia pyöristyy 6106.
- Yksi aalto ehti siis toistaa itseään 6106 kertaa sinä aikana, kun täysin vesilasia

- En mitään tarkkoja mittauksia tehny, mutta lähellä oltiin! [Tässä](https://www.stockmann.com/hobstar-libbey-hobstar-dof--lasi-350-ml/1170457753-1.html) siis lasi vielä, jos jotain jostain ihmeellisestä syystä kiinnostaa kokeilla.
- Tarkka tilavuus olisi siis ollut 350ml, mutta näillä mennään.

- Jos sen kääntää matkaksi, vesipisara tippuu noin 10m/s nopeudella ([lähde](https://gpm.nasa.gov/resources/faq/how-fast-do-raindrops-fall))
- Vesipisara ehtii tuossa ajassa tippua 2,62 millimetriä eli hyvin hyvin hyvin vähän (ChatGPT jälleen laskurina)

- Voidaan vielä zoomata lähemmäs ja tarkistaa

![image](https://github.com/user-attachments/assets/12018737-cbd4-47f4-a380-370c7a911ff2)

Kuten näkee, aallot liikkuvat samassa tahdissa ja samalla taajuudella, kyseessä on ASK - modulaatio, koska bitit 1 ja 0 toimii päälle/pois

Jos alhaalta valitsee "Signal view: Demodulated" - näkee sen viivoina. Eli rakenne on molemmissa tismalleen sama. 1 --> Tapahtuu 0 --> Pieni tauko, toisto, toisto, toisto

![image](https://github.com/user-attachments/assets/36756f36-f3ed-44cd-b71d-0ec124ed2467)


## h) Vapaaehtoinen: Sdr++. Kokeile sdr++ -sovellusta ja esittele sillä jokin "hei maailma" -tyyppinen esimerkki.

## i) Vapaaehtoinen, vaikeahko: GNU Radio. Asenne GNU Radio ja tee sillä yksinkertainen "Hei maailma".

Ilmeisesti se asentui jo samalla, kun tein tämän aiemmin: 

Homma lähti käyntiin ```sudo apt-get install gqrx```
- Virheilmoitus, ettei sellaista ole, vaan se on ```gqrx-sdr```

![image](https://github.com/user-attachments/assets/af6c0717-0ce7-4c4a-b598-38a39ba71142)

![image](https://github.com/user-attachments/assets/4a8dec0c-8d0a-45e3-9e9d-5b92da288a5b)

Se on asenneltu ja pääsee "Applications" - gnuradio

Seuraillaan vähän ohjeita 
Eli id = sineWaveFlowgraph
Titleksi se Hello World
- Apply -> Ok
![image](https://github.com/user-attachments/assets/9a05e87b-c9d3-4611-81ea-f96af2bf3b31)

Ctrl + F ja Signal Source

![image](https://github.com/user-attachments/assets/8af81104-18c9-409b-9e61-7b690eb30bfb)

Seuraavaksi Throttle

![image](https://github.com/user-attachments/assets/522abe0e-45d2-4239-a5ce-52d4d4aceee9)

Sitten QT GUI Frequency Sink

![image](https://github.com/user-attachments/assets/764d5b0f-46a7-4cc3-9f73-83481248dc4c)


Lopuksi QT GUI Time Sink

![image](https://github.com/user-attachments/assets/5ecba2cc-8ca3-48f0-92ed-56a00dc14961)

Ainiin ja kaikki pitää lisätä työtilaan

Eli Signal Source luo "kompleksisen siniaallon"
QT GUI Frequency sink näyttää spektrin amplitudin
QT GUI Time Sink näyttää signaalin käyttäytymisen ajan muodon
Throttle säätää virtausnopeutta

Yhdistellään ne kuvan mukaisesti Out ja IN

![image](https://github.com/user-attachments/assets/7fc02ae3-998d-439a-8e8f-160abeb48414)

Se näytti lopulta siis tältä:

![image](https://github.com/user-attachments/assets/91e6c1b7-7101-4ce7-bf5d-df558e9ab6ef)

Kyseessä ei kai varsinaisesti se "Hei maailma" ole, mutta toistaiseksi joudun jättämään testailut kesken. Aika loppui kesken 17/04/2025


![image](https://github.com/user-attachments/assets/ab27173b-b709-4d2e-b65b-d08eac48c79c)

- Olisin lähtenyt seuraavaksi rakentamaan viestiä siitä


## Lähteet:

Verkkoon tunkeutuminen ja tiedustelu, Tero Karvinen, Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/, Luettu 16/04/2025

Decoding ASK/OOK_PPM Signals with URH and rtl_433, @klohner, Luettavissa: https://github.karllohner.com/SDR/Decoding/Example_2019-01-24/, Luettu 16/04/2025

Decode 433.92 MHz weather station data, Posted by:  Cornelius, Updated 1/2024, Luettavissa:  https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html, Luettu 15/04/2025
 
Hubacek @hubmartin, 2019, Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs, Katsottavissa:  https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s, Katsottu 15/04/2025

ur, Dr.-Ing.Johannes Pohl, @jopohl, Luettavissa: hhttps://github.com/jopohl/urh?tab=readme-ov-file#Installation, Luettu 15/04/2025

Using ‘diff’ in Linux: A Comparison Command Guide, By Gabriel Ramuglia On December 11, 2023, Luettavissa: https://ioflood.com/blog/diff-linux-command/, Luettu: 16/04/2025

rtl_433, Benjamin Larsson @merbanan ja hyvin moni muu, Luettavissa: https://github.com/merbanan/rtl_433, Luettu 16/04/2025

Gqrx SDR, Open source software defined radio by Alexandru Csete OZ9AEC, Luettavissa: https://www.gqrx.dk/, Luettu 16/04/2025

How fast do raindrops fall?, Nasa, Luettavissa: https://gpm.nasa.gov/resources/faq/how-fast-do-raindrops-fall, Luettu 16/04/2025

Modulation and Different Types of Modulation, March 22, 2024
By Ravi Teja, Luettavissa: https://www.electronicshub.org/modulation-and-different-types-of-modulation/, Luettu 16/04/2025

Stekkerdimmerset – 100 Watt led | ACC2-250R, KlikAanKlikUit,  Katsottavissa: https://klikaanklikuit.nl/product/stekkerdimmerset-100-watt-led/, Katsottu 16/04/2025

Hobstar Libbey Hobstar DOF -lasi 350 ml, Stockmann, Katsottavissa: https://www.stockmann.com/hobstar-libbey-hobstar-dof--lasi-350-ml/1170457753-1.html, Katsottu 16/04/2025 

You need Cython to build URH's extensions!, @tomcass240, 2023, vastattu @jopohl, Luettavissa: https://github.com/jopohl/urh/issues/1064, Luettu 16/04/2025

ChatGPT matematiikan professorina

Microsecond, Wikipedia, Luettavissa: https://en.wikipedia.org/wiki/Microsecond, Luettu 16/04/2025

quick and dirty tutorial on how to use #Universal #Radio, Dmitry Janushkevich @InfoSecDJ, Luettavissa: https://devsforgood.com/thread/quick-and-dirty-tutorial-using-universal-radio-hacker-urh-for-practical-applications, Luettu 16/04/2025

pip install -r requirements.txt is failing: "This environment is externally managed" [duplicate], (koko ketju), luettavissa: https://stackoverflow.com/questions/75602063/pip-install-r-requirements-txt-is-failing-this-environment-is-externally-mana, Luettu 16/04/2025

Barney's online radios, websdr, Katsottavissa: http://remoteradio.changeip.org:8073/, Katsottu 15/04/2025

Radio 5 Live Schedule, BBC, Katsottavissa: https://www.bbc.co.uk/sounds/schedules/bbc_radio_five_live, Katsottu 15/04/2025

