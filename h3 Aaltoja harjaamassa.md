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

Lähteet: Hubacek @hubmartin, 2019, Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs, Katsottavissa:  https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s, Katsottu 15/04/2025
