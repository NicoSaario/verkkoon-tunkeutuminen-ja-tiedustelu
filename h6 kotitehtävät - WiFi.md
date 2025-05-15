# Kotitehtävät 


848002000000
Kotitehtävät ovat kurssilta "Verkkoon Tunkeutuminen ja Tiedustelu - Network Attacks and Reconnaissance" ja löytyvät osoitteesta https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#verkkojen-perusteet 

- Nämä tehtävät suoraan Lari Iso-Anttilalta https://github.com/lisoant

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 14/05/2025 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+3) 14/05/2025 (pvm/kk/v)

Kaikissa testaukseen liittyvässä: Oracle VM VirtualBox ja Kali Linux Point release live image (2025.1a)
- Tässä tehtävässä [WiFiChallenge-Virtuaalikone](https://lab.wifichallenge.com/) Oracle VM - kautta
- Valitettavasti mitään Walkthrough tai selityksiä tehtäville ei voi jakaa, sillä se on tekijän mukaan kiellettyä: 

> Sharing, publishing, or distributing walkthroughs, solutions, or any detailed explanations of this lab is strictly forbidden unless you have received explicit written permission from the project maintainers. This includes all formats such as text, video, or interactive content. Violations may result in revocation of access and other legal actions.

## h6 kotitehtävät

### a) Tutustu wifi challenge lab 2.1 harjoitus ympäristöön ja käytä tarvittaessa hyväksesi jo olemassa olevia ohjeita.
- Sain viisi ensimmäistä tehtävää suoritettua niin, että selailin manuaaleja ja Larin kalvoja kuin viimeistä päivää ja hakkasin samalla päätä seinään. Kivojen harjoitusten parissa kuluttaa mielellään vähän enemmän aikaa ja hermoja.

![image](https://github.com/user-attachments/assets/347a4f6c-aeb3-40d7-8f3a-1b0be74f48c1)

- Yritin jatkaa siitä vielä eteenpäin, mutta opin parhaiten tutkimalla ja tekemällä - en walkthrough - ohjetta seuraamalla, joten teen niitä hiljalleen.

### b) Kirjoita raportti siitä mitä opit ja mitkä asia yllättivät sinut kun tutustuit harjoitukseen.

- - Aircrack - ng - työkalut tuli itselle kokonaan uusina käyttöön. Niin tehtävässä, kun aikaisemmassa livedemossa näkyy hyvin niiden työkalujen merkityksen ja sen, mitä kaikkea pienellä vaivalla voikaan saada selville. Yllätyin aidosti siitä, kuinka paljon pienellä vaivalla voi saada irti pelkästä "suojatusta" liikenteestä.
- Isoin yllätys oli ESSID löytyminen noinkin helposti. Olen itse asiassa aina välillä miettinyt sitä, kuinka saisi selville kaikki ne piilotetut verkot, joita ympäriltä löytyy. Ja tämän johdosta ajauduin hetkeksi kuoppaan, jota kaivoin syvemmälle ja syvemmälle. Tästä lisää seuraavassa tehtävässä.

### c) Miten suhtautumisesi WLanin turvallisuuteen muuttui sen jälkeen kun teit harjoitukset?

Ennen tälle alalle lähtemistä, olin aina kiinnostunut IoT - laitteista, niiden toiminnasta ja kiinnitin tavallista enemmän yksityisyyteen huomiota, vaikkakin aina en siihen tehnyt tarvittavia toimia.
- Tähän liittyen käytin kotiverkoissa piiloitettua SSID:tä - Kuvittelin "Jes! Se on piilossa - yksi turvakerros lisää".
- Tuosta syvästä kuopasta - Lähdin vihdoin selvittämään tehtävän jälkeen, mitä se oikeasti tekee ja onko siitä sitten mitään hyötyä. Löysin artikkeleita puolesta ja vastaan, mutta lopulta lopputulos oli sitä, mitä en todellakaan toivonut vaikkakin se nyt näyttää näin jälkeenpäin tutkittuna hyvinkin loogiselta.

Sen piilottaminen tekee siis seuraavaa -> Kyllä. Se tosiaan on "piilossa". Miksi se ei ole hyvä juttu? Jos verkkoon on kytkenyt vaikkapa älypuhelimen - se lähettää jatkuvia kyselyitä (probe request) siitä, onko verkko a se piilotettu kotiverkko vai ei -> *Missä* tahansa kantaman ulkopuolella *kuka* tahansa verkkojen skannaaja näkee siis, että teet jatkuvia kyselyitä siitä, onko se piilotettu verkko lähellä vai ei sekä puhelimen MAC - osoitteen jos sitä ei ole satunnaistettu ennalta-arvaamattomasti
   
    - Loogisesti ajateltuna: Piilotettu ID = Piilossa 
    - Todellisuudessa 'Hei Maailma! Täällä puhelin ja sen MAC - osoite, onko KotiverkkoJonkaPitiOllaPiilossa täällä?!'
    
    - Se johtaa siihen, että ensinnäkin se tekee kohteesta kiinnostavamman, massasta poikkeavan ja jokainen sniffailija tietää, että kotoa tai töistä löytyy piiloitettu verkko... jonka senkin saa selville hetkessä. Mahdollistaa myös kohteen seuraamisen ja tunnistamisen.
    
    - Käytännössä siis hankaloitin elämääni muutaman vuoden vain sen takia, etten lukenut aiheesta ja tein olettamuksia. 

    - Onhan se mahdollista pitää paremmin niin, että kytkisi kaikki WiFiin liittyvät pois päältä, käyttää satunnaista MAC - osoitetta ja unohtaisi piilotetun verkon aina lähtiessä, mutta erittäin epäkäytännöllistä ja turhaa

WLANin turvallisuus on mietityttänyt aina. Vaatiihan se hieman motivaatiota selvittää sen turvallisuuden heikentämistä, mutta näin muutaman hakukonekäytön jälkeen ja laajalla kirjolla työkaluja, on se lähtökohtaisesti todella helppo murtaa heikoilla salauksilla.
- Lähes joka kodista löytyy jonkin asteinen reititin. Niiden turvallisuudesta on puhuttu vuosikausia ja pyritty ohjeistamaan ihmisiä niiden konfiguraatioissa - jopa turhaan.
- Tavallinen käyttäjä (kuten itse olin), kytkee kiinni - katsoo, toimiiko netti - jos ei toimi, tekee vain vaaditut toimenpiteet ja unohtaa sen olemassaolon siihen asti, kunnes verkossa tulee jokin ongelma.

Nyt, kun on konkreettisesti nähnyt, miten pienellä vaivalla saa paljon tavaraa ulos asiasta, jonka kanssa on päivittäin tekemisissä ja joka käytännössä ohjaa päivittäistä toimintaa sekä mahdollistaa pääsyn laitteille - on kyllä todettava, että turvallisuutta pitää parantaa ja ottaa aidosti enemmän selvää siitä, miten se tehdään.
Siitä kyllä saa *turvallisemman*, mutta en usko siihen, että siitä ikinä täysin turvallista tulee. Aina on jokin reikä, johon joku hiiri kaivautuu.

Nyt onkin kesä aikaa parantaa omaa osaamista ja kehittää sen turvallisuutta.


## Lähteet: 

Larin @lisoant kalvot


Pari lähdettä niistä lukemattomista hidden SSID, joita lopulta selailin: 

[Wireless Router] Hidden SSID: Few things you should know, Asus, Last Update : 2024/12/03 18:06, Luettavissa: https://www.asus.com/support/faq/1047947/,Luettu 14/05/2025

Debunking Myths: Is Hiding Your Wireless SSID Really More Secure?, Lowell Heddings, Published Aug 15, 2014, Luettavissa: https://www.howtogeek.com/28653/debunking-myths-is-hiding-your-wireless-ssid-really-more-secure/, Luettu 14/05/2025

Should you hide your Wi-Fi SSID?, Published on 22nd December 2020 Author: Samantha Rutledge, Luettavissa: https://fractionalciso.com/should-you-hide-your-wi-fi-ssid/, Luettu 14/05/2025



Aircrack-ng, Aircrack-ng, Luettavissa: https://www.aircrack-ng.org/, Luettu 14/05/2025

WiFiChallenge, README, r4ulcl, Luettavissa: https://lab.wifichallenge.com/README, Luettu 14/05/2025


En varsinaisesti käyttänyt tehtävissä, mutta hyödynsin raportissa:

Walkthrough WiFiChallenge Lab v2.0, r4ulcl, August 9, 2023  22-minute read, Luettavissa: https://r4ulcl.com/posts/walkthrough-wifichallenge-lab-2.0/, luettu 14/05/2025
