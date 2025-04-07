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

