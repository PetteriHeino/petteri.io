---
title: Entra ID Identity Governance - Access Review
description: >-
 Access Review
authors: Petteri
date: 2025-08-04 03:00:00 +0200
categories: [M365]
tags: [SC-300, Microsoft, Entra ID, Identity Governance, Access Review]
pin: false
media_subpath: '/assets/media/2025-08-04_access_review'
---

## Johdanto

Tämä kirjoitus on osa blogikirjoitus sarjaa, missä käsittelen Entra ID:n palvelukokonaisuutta nimeltä Identity Governance. Tässä kirjoutuksessä keskityn osa-alueeseen Access Review.

Tämän blogisarjan tarkoitus on antaa lyhyt katsaus Microsoftin M365 teknologiaan Suomen kielellä, niin että siitä saa hetkessä kuvan mistä teknologiassa on kyse ja miten palvelua konfiguroidaan. Jos sitten haluaa perehtyä asiaan syvemmin, niin Microsoftin Learn sivustosta löytyy materiaali siihen.

Identity Governance on osa-alue Microsoftin SC-300 sertifiointitestissä.

---

## Access Review - Organisaation resursseihin kohdistettuja käyttöoikeuksien tarkistuksia

### Yleiskatsaus

Access Review -toiminnallisuuden avulla organisaatiot voivat säännöllisesti tarkistaa ja hallita käyttäjien käyttöoikeuksia eri resursseihin. Tämä on keskeinen osa **Microsoft Entra Identity Governance** -kokonaisuutta ja sen tavoitteena on varmistaa, että oikeudet säilyvät ajan mittaan tarpeellisina, turvallisina ja organisaation käytäntöjen mukaisina.

Tyypillinen legacy teknologia ongelma oli se, että organisaatioilta puuttui ymmärrys siitä, että kenellä on oikeuksia mihinkin resursseihin ja käyttäjien lähtiessä pois organisaatiosta ei ollut ymmärrystä siitä minne kaikkialle käyttäjän oikeudet periytyivät. Access Review M365 pilvipalveluiden vastaus tähän ongelmaan.

Käyttöoikeustarkistusten avulla organisaatiot voivat:
•	Ajoittaa säännölliset tai ad hoc -tarkistukset nähdäksesi selvittääkseen kenellä on pääsy mihinkin resursseihin
•	Delegoida tarkistuksia järjestelmänvalvojille, palveluiden omistajille, ryhmäpääliköille tai vahvistaa itse käyttöoikeuksien jatkuvuus
•	Automatisoida tarkistustulokset, kuten käyttäjien resurssien käyttöoikeuksien poistaminen
•	Seurata access review dataa käyttöstatistiikan tai vaatimustenmukaisuuden saamiseksi

Access Review on tehokas ja joustava työkalu, jolla voidaan varmistaa käyttöoikeuksien ajantasaisuus ja tietoturva. Se on olennainen osa identiteetinhallinnan kokonaisuutta ja erityisen tehokas yhdessä Access Packagien ja PIM:n kanssa. Soveltuu sekä sisäisten että ulkoisten käyttäjien käyttöoikeuksien hallintaan – skaalautuen yrityksen kokoon ja tarpeisiin.

> **Vinkki:** Yhdistä Access Review Microsoft Sentinel -valvontaan saadaksesi kokonaiskuvan käyttöoikeuksista ja poikkeamista reaaliaikaisesti.

---

### Lisenssivaatimukset

Access Review -toiminto on käytettävissä Microsoft Entra -palvelussa seuraavin ehdoin:

| Lisenssi | Tukee Access Review -toimintoa |
|---------|-------------------------------|
| **Microsoft Entra ID P2** | ✅ Täysi tuki |
| Microsoft Entra ID P1 | ❌ Ei sisällä Access Review -toimintoa |
| Microsoft 365 E5 / EMS E5 | ✅ Sisältyy (koska sisältää Entra ID P2:n) |
| Microsoft 365 E3 / EMS E3 | ❌ Ei sisällä |
| Ilmainen Entra ID | ❌ Ei sisällä |

> **Huom:** Access Review edellyttää lisenssiä kaikille osallistujille – sekä käyttöoikeuksien tarkistettaville että tarkistajille!

B2B Collaboration käyttäjien osalta lisenssisääntö 50:1 sääntö:
→ 1 P2-lisenssi kattaa 50 ulkoista käyttäjää.
Lisenssointi perustuu Monthly active users (MAU) malliin, eli 1 lisenssi per 50 käyttäjää tulee olla vapaana.

---

### Keskeiset roolit ja niiden oikeudet

| Rooli | Kuvaus |
|-------|--------|
| **Global Administrator / Identity Governance Administrator** | Voi luoda ja hallita kaikkia Access Review -prosesseja. |
| **Group Owner** | Voi tarkistaa jäsenten oikeuksia ryhmässä, jonka omistaja hän on. |
| **Access Package Manager** | Voi luoda käyttöpaketteja ja liittää niihin Access Review -tarkistuksia. |
| **Reviewer (tarkistaja)** | Käyttäjä, jolle on delegoitu oikeus tarkistaa toisten käyttöoikeuksia (esim. tiiminvetäjä). |
| **Self-review target user** | Käyttäjä, joka tarkistaa omia käyttöoikeuksiaan. |

> Kaikki roolit edellyttävät P2-lisenssiä. Ryhmän omistajalla voi olla oikeus tarkistaa omien ryhmiensä käyttöoikeuksia, mutta tämäkin vaatii lisenssin.

---

### Tuetut resurssityypit

| Resurssi | Roolien/ominaisuuksien tuki |
|----------|-----------------------------|
| **Entra ID -ryhmät** | Tukee omistajien tai nimettyjen tarkistajien suorittamia tarkistuksia. |
| **Entra ID roolit (PIM)** | Mahdollistaa roolien tarkistuksen käyttöoikeuksien hallinnassa (PIM). |
| **Access Packages** | Tukee automaattisia käyttöoikeustarkistuksia osana käyttöpakettien elinkaarta. |
| **Sovellukset** | Voidaan tarkistaa käyttäjäkohtaisia käyttöoikeuksia (esim. SSO-sovellukset). |
| **Ulkoiset käyttäjät (B2B collaboration)** | Mahdollistaa ulkoisten yhteistyökumppaneiden oikeuksien tarkistamisen. |

> Joissain tapauksissa (esim. roolit PIM:n kautta) tarkistus vaatii lisäkonfiguraatioita ennen näkyvyyttä Access Reviewissa.

---

### Access Review + Access Package – yhteiskäyttö

Access Packages ovat käyttöpaketteja, jotka sisältävät resursseja ja määrittelevät, miten käyttäjät voivat niitä hakea.

- Access Review voidaan **liittää suoraan käyttöpakettiin**.
- Käyttäjien käyttöoikeudet tarkastellaan automaattisesti tietyin väliajoin.
- Tarkistuksen jälkeen voidaan määrittää esimerkiksi:
  - **Automaattinen poisto**, jos tarkistus epäonnistuu.
  - **Oikeuden säilyttäminen**, jos tarkistus jää tekemättä.

Yhdistelmä mahdollistaa käyttöoikeuksien hallinnan ilman jatkuvaa manuaalista valvontaa.

---
### Käyttöoikeus tarkistusten suunnittelu

Käyttöoikeuksien tarkistusten suunnittelu ennen niiden toteuttamista on välttämätöntä, jotta organisaatiosi työntekijöille ja vieraskäyttäjille voidaan saavuttaa haluttu hallintostrategia. Halutun strategian saavuttamiseksi:
- Sitouta oikeat sidosryhmät.
- Suunnittele pilotti.
- Luettele tarkistettavat resurssityypit.
- Tunnista tarkistajat käyttöoikeuksien tarkistamista varten.

#### Sitouta oikeat sidosryhmät

Oikeiden sidosryhmien sitouttaminen on ratkaisevan tärkeää halutun vaikutuksen, tulosten ja vastuiden saavuttamiseksi. Käyttöoikeustarkistuksissa on todennäköisesti edustajia seuraavista tiimeistä:
- IT-järjestelmänvalvoja – joka hallinnoi IT-infrastruktuuria, pilviinvestointeja ja SaaS (Software as a Service) -sovelluksia
- Kehitystiimit – jotka rakentavat ja ylläpitävät sovelluksia organisaatiollesi
- Liiketoimintayksiköt – jotka hallinnoivat projekteja ja omia sovelluksia
- Corporate governance – kuka varmistaa, että organisaatio noudattaa sisäisiä käytäntöjä ja säädöksiä

#### Suunnittele pilotti

On suositeltavaa suunnitella pilottikäyttöoikeuksien arvioinnit kohdennetulla pienellä ryhmällä ei-kriittisiä resursseja. Pilotin tulosten perusteella voit muokata prosesseja ja viestintää tarpeen mukaan.
- Suosituksia pilotin suunnitteluun ovat:
- Aloita arvioinneista, joissa tuloksia ei sovelleta automaattisesti.
- Varmista, että kaikilla käyttäjillä on kelvollinen sähköpostiosoite, jotta he voivat vastaanottaa sähköpostiviestejä asianmukaisten toimien suorittamiseksi.
- Dokumentoi kaikki pilotin aikana tekemäsi muutokset, kuten käyttöoikeuden poistaminen.
- Valvo valvontalokeja varmistaaksesi, että kaikki tapahtumat kirjataan oikein.

#### Luettelo tarkastettavista resurssityypeistä

Resursseja, kuten käyttäjiä (sekä sisäisiä että ulkoisia käyttäjiä), sovelluksia ja ryhmiä, voidaan hallita ja tarkastella Access Review toiminnallisuuden avulla.
Käyttöoikeuksien tarkistusten tyypillisiä kohteita ovat:
- Käyttäjien pääsy sovelluksiin – SaaS- tai toimialakohtaiset sovellukset, jotka on integroitu Azure AD:hen kertakirjautumista varten
- Ryhmän jäsenyys – ryhmät, jotka synkronoidaan Azure AD:hen tai jotka on luotu Azure AD:ssä tai Microsoft 365:ssä, mukaan lukien tiimit
- Käyttöpaketit – resurssit, kuten ryhmät, sovellukset ja sivustot
- Azure AD roolit ja Azure-resurssiroolit – roolit, jotka on määritetty Privileged Identity Management (PIM)

#### Tunnista tarkistajat käyttöoikeuksien tarkistamiseksi

Käyttöoikeustarkistuksia luodessaan tekijä päättää, kuka suorittaa tarkistuksen. Käyttöoikeustarkistuksen luoja voi muokata tätä asetusta milloin tahansa, vaikka tarkistus olisi aloitettu. Kolme henkilöä edustaa yleensä arvioijia:
- Resurssin omistajat – resurssin yrityksen omistaja
- Yksilöllisesti valittujen edustajien joukko – käyttöoikeuksien tarkistuksen luojan valitsema
- Loppukäyttäjä – itse varmentaa jatkuvan käytön

Ennen kuin otat käyttöoikeuksien tarkistukset käyttöön, suunnittele organisaatiollesi merkitykselliset tarkistustyypit. Käyttöoikeuksien tarkistuskäytännön luomiseen tarvitaan seuraavat tiedot:
- Mitä resursseja on tarkistettava?
- Kenen käyttöoikeuksia tarkistetaan?
- Kuinka usein tarkistus tulisi tehdä?
- Kuka suorittaa arvioinnin?
- Miten arvioijalle ilmoitetaan tarkistamisesta?
- Mitkä ovat tarkistuksen määräajat?
- Mitä tapahtuu, jos arvioija ei vastaa ajoissa?
- Mitä automaattisia toimia tulisi panna täytäntöön?
- Mitä manuaalisia toimia tehdään tarkistustulosten perusteella?
- Mitä viestejä tulisi lähettää tehtyjen toimenpiteiden perusteella?

### Access Reviewin luominen – Vaiheittainen ohje

1. **Avaa Azure-portaali**  
   Siirry: *Microsoft Entra ID > Identity Governance > Access Reviews*

2. **Luo uusi tarkistus**  
   Valitse **"New access review"**

3. **Valitse tarkistettava kohde (Review type)**  
   Esim. ryhmä, käyttöpaketti, rooli tai sovellus

   ![Kuva]({{ site.baseurl }}/accessreview1.png)

4. **Määritä tarkistajat (Reviews) ja tarkistuksen toistuvuus**
   
   Asetukset:
   - Multistage review
   - Select Reviewers. Vaihtoehdot:
     - Group Owner  
     - Nimetty henkilö  
     - User’s manager  
     - Self-review (käyttäjät itse)
   - Duration (in Days)
   - Review recurrence
     - One time
     - Weekly
     - Monthly
     - Quarterly
     - Semi-annually
     - Annually
   - Start Date
   - End
     - Never
     - End On Specfic date
     - End after number of occurences
![Kuva]({{ site.baseurl }}/accessreview2.png)
6. **Toiminta tarkistuksen jälkeen (Settings)**
   Asetukset:
   - Auto apply results to resource  
   - If reviewers don't respond  
   - At end of review, send notification to
   - No sign-in within 30 days
   - User-to-Group Affiliation
   - Justification required
   - Email notifications
   - Reminders
   - Additional content for reviewer email
![Kuva]({{ site.baseurl }}/accessreview3.png)
7. **Tarkista asetukset ja anna access review nimi(Review + Create)**  
   - Anna kuvaava nimi ja kuvaus. Tarkista asetukset ja klikkaa Create painiketta
![Kuva]({{ site.baseurl }}/accessreview4.png)

---



