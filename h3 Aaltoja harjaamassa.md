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

Lähteet: Hubacek @hubmartin, 2019, Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs, Katsottavissa:  https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s, Katsottu 15/04/2025
