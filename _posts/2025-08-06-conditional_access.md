---
title: Microsoft Entra ID –Conditional Access (CA)
description: CA lisenssit, rakenne, templates, nimeämiskäytännöt, hyvät käytännöt, testaaminen ja käyttöönotto
authors: Petteri
date: 2025-08-07 09:02:00 +0200
categories: [M365]
tags: [SC-300, Microsoft, Entra ID, Conditional Access, CA]
pin: false
media_subpath: '/assets/media/2025-08-07_conditionalaccess'
---
## Johdanto

Tämä blogikirjoitus on osa kirjoittamaani Microsoft SC-300 testi sarjaa, missä käsittelen kevyellä tasolla testissä käsiteltäviä asioita. Tarkoituksena on että kirjoituksista saa nopeasti ymmärryksen mistä asiassa on kyse ja kuinka perus konfigurointi tapahtuu. Kun haluaa syventää ymmärtämistä, niin kannattaa lukea jatkoksi Microsoft Learniä, mistä samat asiat löytyvät huomattavasti kattavammin ja tarkemmin (mutta ovat myös raskaampia lukea jos asia on täysin uusi).

Conditional Access on erittäin tärkeä osa Microsoft pilvipalveluiden tietoturva palveluita. Se on yksi pieni SC-300 sertifiointitestiä. Olen käsitellyt tässä kirjoituksessa jonkun verran asioita, joita ei testissä kysytä, mutta mitkä ovat olennaisia Conditional Access ylläpidon kannalta.

---

## Conditional Access (CA) - Mikä on Conditional Access?

**Conditional Access (ehdollinen käyttöoikeus)** on Microsoftin pilvipalveluissa (kuten Azure AD:ssä) käytettävä tietoturvaominaisuus, jonka avulla voidaan hallita ja suojata käyttäjien pääsyä sovelluksiin ja tietoihin. Ajatuksena on yksinkertainen: **pääsy myönnetään tai estetään ehtojen perusteella**.

Perinteisesti käyttäjän tunnistaminen perustui vain käyttäjätunnukseen ja salasanaan. Conditional Access lisää tähän joukon älykkäitä ehtoja, kuten:

- Mistä sijainnista käyttäjä kirjautuu
- Mitä laitetta hän käyttää
- Mikä on kirjautumisen riskiarvio
- Mikä sovellus on kyseessä
- Kuuluuko käyttäjä tiettyyn ryhmään

Näiden tietojen perusteella voidaan määrittää, pitääkö esimerkiksi:

- Vaatia monivaiheinen tunnistautuminen (MFA)
- Estää pääsy kokonaan
- Sallia pääsy vain tietyin rajoituksin (esim. vain hallituilla laitteilla)

**Conditional Access yhdistää turvallisuuden ja käytettävyyden.** Sen avulla organisaatio voi suojata tietonsa ilman, että se vaikeuttaa loppukäyttäjän arkea – ja mikä tärkeintä, se reagoi älykkäästi muuttuviin olosuhteisiin.

## Conditional Access -lisensointi

**Conditional Access (CA)** on Microsoft Entra ID:n (ent. Azure AD) tarjoama toiminto, jolla voidaan hallita pääsyä resursseihin ehtojen perusteella.  
Käytettävissä olevat ominaisuudet riippuvat käytössä olevasta lisenssitasosta.

### Perusperiaate

- **Conditional Access -käytännöt eivät ole käytettävissä ilmaisessa Entra ID -versiossa.**
- Perustoiminnallisuus edellyttää vähintään **Microsoft Entra ID P1** -lisenssiä (tai sitä sisältävää pakettia).
- Kehittyneet toiminnot, kuten riskiperusteinen pääsynhallinta, vaativat **Entra ID P2** -lisenssin.

> ⚠️ Jokaisella käyttäjällä, johon käytäntö kohdistuu, tulee olla vaadittu lisenssi.

### Microsoft Entra ID P1

#### Sisältyvät ominaisuudet:

- Conditional Access -käytäntöjen luonti ja käyttö
- Perustason käyttöehdot:
  - Käyttäjäryhmät
  - Sovellukset
  - Sijainti (IP/location)
  - Laitetyypit ja -tilat
- Multi-Factor Authentication (MFA) vaatiminen
- Laitteen vaatimukset (hyväksytty tai hybrid-joined)
- Istunnon hallinta (esim. sign-in frequency)
- **Use app enforced restrictions**
- **Use Conditional Access App Control** (vaatii lisäksi Defender for Cloud Apps -lisenssin)

#### Mukana seuraavissa lisenssipaketeissa:

- Microsoft Entra ID P1 (itsenäinen)
- Microsoft 365 E3
- Enterprise Mobility + Security (EMS) E3
- Microsoft Business Premium (osittain – katso huomautus alla)

> ℹ️ *Microsoft 365 Business Premium -paketissa on rajoitettu Conditional Access -tuki, mutta ei kaikkia P1-ominaisuuksia.*

### Microsoft Entra ID P2

#### Sisältyvät lisäominaisuudet:

- **Riskipohjaiset ehdot:**
  - User risk
  - Sign-in risk
- **Identity Protection** -integraatio
- **Continuous Access Evaluation (CAE)** täydellä tuella
- **Custom authentication context** -käytöt
- **Require token protection for sign-in sessions**
- Insider risk -ehtojen käyttö (esim. käyttäjän käyttäytymisen analysointi)
- Käytännön vaikutusten simulointi (report-only -tila) ja analytiikka

#### Mukana seuraavissa lisenssipaketeissa:

- Microsoft Entra ID P2 (itsenäinen)
- Microsoft 365 E5
- EMS E5

### Muita lisensseihin liittyviä huomioita

| Toiminto | Vähimmäisvaatimus |
|----------|--------------------|
| Conditional Access -käytännöt yleensä | Entra ID P1 |
| Sign-in risk / User risk ehdot | Entra ID P2 |
| Authentication context | Entra ID P2 |
| Use Conditional Access App Control | Entra ID P1 + Defender for Cloud Apps |
| Token protection for sessions | Entra ID P2 |
| Global Secure Access -integraatio | GSA + erillinen lisenssi (esim. Entra Internet Access) |
| Insider risk -ehdot | Entra ID P2 (tai Microsoft Purview riippuen käytöstä) |

> ⚠️ Käytännön voi kohdistaa vain käyttäjiin, joilla on vaadittu lisenssi. Microsoft tarkistaa lisenssien vaatimustenmukaisuuden aktiivisesti.

---

## Conditional Access rakenne

Conditional Access -käytäntö koostuu eri osista, joita kutsutaan **elementeiksi**. Käytännössä kyseessä on kuin *"IF–THEN"-tyyppinen sääntö*, jossa määritellään **ehtoja** (IF) ja niiden perusteella **toimintoja** (THEN).

CA-käytännön logiikka rakentuu näiden elementtien alle konfiguroitavista asetuksista. Näin saadaan aikaan joustavia ja tarkasti kohdennettuja sääntöjä, jotka määrittävät, milloin pääsy sallitaan, estetään tai esimerkiksi edellytetään monivaiheista tunnistautumista.

Alla olevassa kuvassa on esitetty CA-käytännön päätaso, jossa näkyvät kaikki käytettävissä olevat elementit.


![Kuva]({{ site.baseurl }}/ca01.png)

### CA assignments - Users elementti

![Kuva]({{ site.baseurl }}/ca02_users_a_2.png)

**Users**-elementti määrittelee, keitä käyttäjiä Conditional Access -käytäntö koskee. Tämä on yksi käytännön tärkeimmistä osista, sillä sen avulla kohdennetaan käytäntö oikeille henkilöille tai järjestelmäidentiteeteille. Käytännön voi kohdistaa koko organisaatioon, valittuihin ryhmiin tai yksittäisiin käyttäjiin – ja vastaavasti poissulkea käyttäjiä tarpeen mukaan.

#### Include (Sisällytä käyttäjät)

**Include**-välilehdellä määritetään, keihin käytäntöä sovelletaan. Tämä tehdään valitsemalla jokin seuraavista vaihtoehdoista:

##### None ja All Users

Nämä vaihtoehdot ymmärrettävät ja itsensä selittävät.

##### Select users and groups

Valittavina ovat seuraavat kolme alikategoriaa:

##### **1 Select Users and groups - Guest or external users and groups**

Kun valitaan **Guest or external users and groups**, Conditional Access -käytäntö kohdistetaan organisaation ulkopuolisiin käyttäjiin. Tämä on tärkeä osa-alue erityisesti ympäristöissä, joissa käytetään **Azure AD B2B -yhteistyötä** eli jaetaan resursseja ulkopuolisten tahojen kanssa.

![Kuva]({{ site.baseurl }}/ca02_users_b_2.png)

Valittavissa olevat käyttäjätyypit:

- **B2B collaboration guest users**  
  Vieraskäyttäjät, jotka on kutsuttu Azure AD -hakemistoon roolilla *Guest*. He käyttävät omaa organisaatiotunnustaan, mutta heille on luotu käyttäjäobjekti isäntäorganisaation hakemistoon. Nämä ovat yleisimpiä B2B-käyttäjiä.

- **B2B collaboration member users**  
  Ulkopuolisia käyttäjiä, joille on poikkeuksellisesti annettu *Member*-rooli *Guest*-roolin sijaan. Tämä voi olla tarpeen, jos halutaan tarjota heille enemmän oikeuksia tai pääsyä sisäisiin palveluihin.

- **B2B direct connect users**  
  Käyttäjiä, jotka muodostavat suoran yhteyden toiseen organisaatioon esimerkiksi **Teamsin jaettujen kanavien** (shared channels) kautta. He eivät ole varsinaisesti kutsuttuja vieraita hakemistossa, mutta silti ulkoisia käyttäjiä.

- **Local guest users**  
  Käyttäjät, jotka on luotu paikallisesti Azure AD -hakemistoon ilman ulkoista B2B-kutsua. Näitä voi syntyä esimerkiksi erikoistapauksissa tai manuaalisesti. Ne muistuttavat sisäisiä käyttäjiä, mutta niillä on *Guest*-rooli.

- **Service provider users**  
  Käyttäjät, jotka liittyvät palveluntarjoajiin ja ovat osa **Microsoft Entra cross-tenant management** -ratkaisuja. Näillä käyttäjillä voi olla erityisiä oikeuksia hallita, tukea tai ylläpitää toisen tenantin ympäristöä. Erottelu mahdollistaa hallitun pääsyn ulkopuolisille palvelutoimittajille.

- **Other external users**  
  Kaikki muut ulkoiset käyttäjät, joita ei suoraan luokitella yllä oleviin ryhmiin. Tämä voi sisältää tunnistautumattomia, tilapäisiä tai muita ei-standardimuotoisia ulkopuolisia identiteettejä.

##### **2 Select users and groups - Directory roles**

![Kuva]({{ site.baseurl }}/ca02_users_a_3.png)

Tämän avulla käytäntö voidaan kohdistaa tiettyihin Azure AD -hakemistoroolin käyttäjiin (esimerkiksi **Global Administrator**, **Security Reader**, **Helpdesk Administrator** jne.). Tämä mahdollistaa erityisen tarkat suojaukset hallintakäyttöön.

##### **3 Select users and groups - Users and groups**

![Kuva]({{ site.baseurl }}/ca02_users_a_4.png)

Tässä osiossa voit valita tietyt käyttäjät ja/tai ryhmät, joihin Conditional Access -käytäntö kohdistuu.

#### Exclude (Poissulje käyttäjät)

**Exclude**-välilehdellä voidaan määrittää poikkeuksia siihen, keihin Conditional Access -käytäntö *ei* kohdistu, vaikka he muuten sisältyisivät Include-valintojen piiriin.

Tätä käytetään esimerkiksi tilanteissa, joissa halutaan jättää tietyt ylläpitäjät, testikäyttäjät tai järjestelmätilit käytännön ulkopuolelle.

Valintavaihtoehdot ovat pitkälti samat kuin **Include**-välilehdellä: voit poissulkea yksittäisiä käyttäjiä, ryhmiä, ulkoisia käyttäjiä tai hakemistoroolien perusteella. Exclude toimii ikään kuin suodattimena Include-ryhmän sisällä.

> 💡 *Huom:* Jos käyttäjä sisältyy sekä Include- että Exclude-listalle, **Exclude voittaa** — käytäntöä ei sovelleta kyseiseen käyttäjään.


### CA assignments - Target resources (Kohderesurssit) elementti

**Target resources** -elementillä määritellään, mihin resursseihin tai toimintoihin Conditional Access -käytäntö kohdistuu. Tämä on keskeinen osa käytäntöä, sillä sen avulla voidaan rajata, mitkä sovellukset, palvelut tai toiminnot vaativat lisäsuojausta.

![Kuva]({{ site.baseurl }}/ca03_resources_a_0.png)

Target resources -asetukset on jaettu alasvetovalikon alle kohdassa **Select what this policy applies to**, jossa on kolme päävälilehteä:

#### 1. Select what this policy applies to: Resources

Tässä valikossa määritellään, mihin resursseihin Conditional Access -käytäntö kohdistuu. Vaihtoehtoja on useita, joilla voidaan hallita käytännön vaikutusalaa tarkasti:

- **None**  
  Käytäntöä ei kohdisteta mihinkään resurssiin. Tämä valinta tarkoittaa, ettei käytäntö aktivoidu missään resurssissa.

- **All internet resources with Global secure access**  
  Tämä vaihtoehto kohdistaa käytännön kaikkiin internetin kautta saavutettaviin resursseihin, jotka tukevat *Global secure access* -mallia. Se on laaja kohdistus, joka kattaa kaikki verkkoon pääsyä vaativat sovellukset ja palvelut, jotka on integroitu Microsoftin turvallisuusmalliin.

- **All resources**  
  Valitse tämä, jos haluat, että käytäntö vaikuttaa kaikkiin organisaation hallinnoimiin resursseihin ilman poikkeuksia.

- **Select resources**  
  Tämän valinnan kautta voidaan tarkasti rajata, mihin yksittäisiin resursseihin käytäntö sovelletaan.  
  Kun valitset **Select resources**, saat käyttöösi kaksi lisäasetusta:

  - **Edit filter**  
    Tällä asetuksella voit määritellä suodattimia, joiden avulla voit rajata resurssit esimerkiksi sovellustyypin, julkaisumuodon tai muun metatiedon perusteella. Filterin avulla voidaan luoda dynaamisia kohdistuksia ilman, että jokaista resurssia tarvitsee valita erikseen.

  - **Select**  
    Tässä osassa voit valita yksittäiset resurssit listalta, esimerkiksi tietyt pilvisovellukset tai palvelut, joihin käytäntö halutaan kohdistaa.

Näiden vaihtoehtojen avulla Conditional Access -käytännön kohdistaminen voidaan tehdä joko hyvin laajasti tai erittäin tarkasti, tarpeen mukaan.


#### 2. Select what this policy applies to: User actions

![Kuva]({{ site.baseurl }}/ca03_resources_a_3.png)

Tässä valinnassa määritellään, mihin käyttäjän suorittamiin toimintoihin Conditional Access -käytäntö kohdistuu. Käytännössä tämä tarkoittaa, että käytäntö aktivoituu vain, kun käyttäjä yrittää suorittaa tiettyjä valittuja toimenpiteitä.

Valittavissa olevat käyttäjätoiminnot ovat:

- **Register security information**  
  Tämä koskee käyttäjän toimenpiteitä, joissa hän rekisteröi tai päivittää turvallisuustietojaan, kuten monivaiheisen tunnistautumisen asetuksia (MFA), varmistussähköpostia tai puhelinnumeroa. Käytännöllä voidaan esimerkiksi vaatia lisäturvaa näissä kriittisissä toiminnoissa.

- **Register or join devices**  
  Tämä kattaa käyttäjän toimet, joissa hän rekisteröi laitteen Azure AD:hen tai liittyy organisaation hallintaan (esim. liittää laitteen Intuneen). Tämän asetuksen avulla voidaan suojata laitehallintaan liittyvät prosessit ja varmistaa, että vain hyväksytyt käyttäjät ja laitteet voivat rekisteröityä.

Näiden asetusten avulla voidaan kohdistaa Conditional Access -käytäntö kriittisiin käyttäjätoimintoihin ja siten vahvistaa organisaation turvallisuutta entisestään.

#### 3. Select what this policy applies to: Authentication context

![Kuva]({{ site.baseurl }}/ca03_resources_a_4.png)

**Authentication context** eli todennusympäristö tarkoittaa erityisiä olosuhteita tai tunnisteita, jotka kuvaavat, millaisessa tilanteessa käyttäjä kirjautuu tai käyttää palvelua.

Se toimii lisäkeinona hallita ja suojata pääsyä kriittisiin resursseihin tai toimintoihin. Käytännössä authentication contextilla voidaan esimerkiksi:

- Vaadita vahvempaa todennusta (kuten monivaiheista tunnistautumista) vain tietyissä tilanteissa, kuten riskialttiissa kirjautumisissa  
- Erottaa tavallinen kirjautuminen kriittisestä toiminnosta, kuten talousjärjestelmään kirjautumisesta  
- Soveltaa erilaisia pääsynhallinnan sääntöjä riippuen siitä, mistä sovelluksesta tai verkosta käyttäjä kirjautuu

Azure AD:ssä nämä authentication contextit määritellään erikseen, ja ne toimivat ikään kuin "tunnisteina" tai "tiloina", joiden perusteella Conditional Access -käytännöt voidaan aktivoida tai jättää käyttämättä.

💡 *Huom:* Jos tenantissasi näkyy viesti **"No authentication contexts have been configured"**, se tarkoittaa, että organisaatiossasi ei ole vielä luotu tai otettu käyttöön tällaisia tunnisteita. Tällöin et voi käyttää tätä asetusta ennen kuin authentication contextit on määritelty.

Authentication contextin luominen vaatii lisäkonfiguraatiota Azure AD:ssa, ja sen käyttöönotto kannattaa suunnitella osana laajempaa turvallisuusstrategiaa.


### CA assignments - Network elementti

![Kuva]({{ site.baseurl }}/ca04__network_a_1.png)

**Network**-elementti määrittää, mistä verkko-osoitteista tai sijainneista Conditional Access -käytäntö aktivoituu. Tällä voidaan rajata käytännön vaikutus vain tietyille verkoille, kuten organisaation sisäverkoille tai luotetuille sijainneille, parantaen näin turvallisuutta ja hallittavuutta.

#### Network - Include (Sisällytä):

- **Any Network and location**  
  Käytäntö koskee kirjautumisia ja toimintoja *kaikista verkko-osoitteista* ja sijainneista. Tämä on laajin mahdollinen kohdistus.

- **All trusted networks and locations**  
  Käytäntö kohdistuu vain ennalta määriteltyihin luotettuihin verkkoihin ja sijainteihin. Näihin voi kuulua esimerkiksi yrityksen toimipisteiden IP-osoitteet tai tunnetut VPN-yhteydet.

- **All Compliant Network locations** (harmaana)  
  Tämä vaihtoehto tarkoittaa verkkoja tai sijainteja, jotka on todettu *vaatimusten mukaisiksi* (compliant) esimerkiksi Microsoft Endpoint Managerin (Intunen) hallinnan kautta.  
  Jos tämä asetus on harmaana, tarkoittaa se yleensä, ettei tenantissasi ole määriteltynä compliant network location -kohteita tai sinulla ei ole tarvittavia lisenssejä tai oikeuksia tämän käytön aktivoimiseen.

- **Selected networks and locations**  
  Voit valita tarkasti ne verkot ja sijainnit, joihin käytäntö sovelletaan. Tämä mahdollistaa käytännön kohdistamisen esimerkiksi vain tiettyihin IP-alueisiin tai maantieteellisiin sijainteihin.

💡 *Huom:* Microsoft on uudistanut Conditional Access -kokonaisuutta, ja **Network**-elementti korvaa vähitellen aiemmin käytetyn **Conditions**-osion **Locations**-asetukset. Tämä tuo uusia toiminnallisuuksia ja parempaa hallintaa verkkoehtojen määrittelyyn.

#### Network - Exclude (Poissulje):

**Exclude**-välilehdellä määritellään ne verkot tai sijainnit, joista Conditional Access -käytäntö *ei* koske käyttäjiä, vaikka nämä verkot sisältyisivät Include-valintoihin.

Valintojen perusteet ovat samankaltaiset kuin Include-välilehdellä, mutta niiden merkitys on päinvastainen: ne toimivat poikkeuksina, eli käytäntöä ei sovelleta näissä verkoissa tai sijainneissa oleviin kirjautumisiin.

💡 *Huom:* Jos käyttäjä kirjautuu verkosta, joka on sekä Include- että Exclude-listalla, **Exclude**-asetus menee voimaan eli käytäntöä ei sovelleta.


### CA assignments - Conditions elementti

**Conditions**-elementin avulla Conditional Access -käytäntö voidaan kohdistaa tarkemmin käyttäjän kontekstiin perustuen. Täällä määritellään erilaisia ehtoja, kuten riskiarvioita, laitetyyppejä tai maantieteellisiä sijainteja, jotka vaikuttavat siihen, aktivoituuko käytäntö.

![Kuva]({{ site.baseurl }}/ca05__conditions_0_1.png)

Conditions toimii käytännössä "IF"-osuutena: jos jokin ehto täyttyy, määritetty toimenpide (esim. MFA-vaatimus) käynnistyy.

Seuraavassa käydään läpi käytettävissä olevat ehtotyypit:

#### User risk

Tämä ehto perustuu Microsoftin tekemään arvioon siitä, onko käyttäjätili vaarantunut. Riskitasot voivat olla matala, keskitaso tai korkea, ja ne perustuvat mm. epätavalliseen toimintaan tai tietovuotoihin.

Esimerkiksi: *Vaadi MFA, jos käyttäjän riski on keskitasoinen tai korkeampi.*

#### Sign-in risk

Tämä ehto liittyy nimenomaan yksittäiseen kirjautumistapahtumaan, ei käyttäjään yleisesti. Riskit arvioidaan mm. seuraavien perusteella:

- Kirjautuminen uudelta laitteelta tai sijainnista
- Epätavallinen käyttäytyminen
- Tunnetuista uhkalähteistä kirjautuminen

Tällä voidaan esimerkiksi estää pääsy kokonaan, jos kirjautumisriski on korkea.

#### Insider risk

Tämä ehto perustuu Microsoft Purview Insider Risk Management -toimintoihin ja vaatii erillisen konfiguroinnin. Ehto tunnistaa tilanteita, joissa käyttäjän toiminta viittaa sisäiseen uhkaan, kuten tietojen vuotamiseen tai tarkoitukselliseen haitantekoon.

Useimmissa ympäristöissä tämä asetus ei ole oletuksena käytettävissä.

#### Device platforms

Täällä voidaan määrittää, koskeeko käytäntö tiettyjä käyttöjärjestelmiä, kuten:

- Windows
- macOS
- iOS
- Android
- Linux

Tämän avulla voidaan esimerkiksi sallia pääsy vain hallituilta Windows-laitteilta tai estää tietyntyyppiset laitteet.

#### Locations

Tämä asetus mahdollistaa käytännön kohdistamisen sijaintien (esim. IP-alueet, maat tai luotetut verkot) perusteella.  
> 💡 *Huom:* Tämä on sama toiminto, jonka **Network**-elementti on vähitellen korvaamassa.  
Location-asetuksia käytetään edelleen monissa ympäristöissä, mutta uudet ominaisuudet löytyvät jatkossa Network-elementin alta.

#### Client apps

Tämä asetus rajaa käytännön sovellettavaksi käytetyn sovellustyypin perusteella:

- Selain (browser)
- Modernit sovellukset (modern authentication)
- Perinteiset sovellukset (legacy authentication, esim. vanhat Outlook-versiot)
- Mobiili- ja työpöytäsovellukset

Tällä voidaan esimerkiksi estää perinteisten sovellusten käyttö tai vaatia MFA vain selainpohjaisessa kirjautumisessa.

#### Filter for devices

Tämä mahdollistaa käytännön kohdistamisen laiteominaisuuksiin perustuen (esim. Intunen tai Entra ID:n hallinnoimat ominaisuudet). Käytetään yleensä yhdessä **device compliance** -ehtojen kanssa.

Esimerkiksi: *Salli pääsy vain, jos laitteessa on tietty tagi, tai se on suojattu BitLockerilla.*

#### Authentication flows

Tämä on uudempi ehto, jolla voidaan kohdistaa käytäntö tiettyihin kirjautumisprosessin vaiheisiin tai menetelmiin, kuten:

- Passwordless-kirjautuminen
- FIDO2
- Windows Hello for Business
- Temporary Access Pass

Tällä voidaan hallita tarkemmin, millaisia todennustapoja hyväksytään eri tilanteissa.


### CA access controls - Grant elementti

**Grant controls** -osio määrittää, mitä ehtoja käyttäjän on täytettävä, jotta hän saa pääsyn valittuun resurssiin. Tämä on käytännön *"THEN"* -osa: jos käyttäjä täyttää asetetut **Conditions**-ehdot, **Grant**-asetukset määrittävät, myönnetäänkö pääsy ja millä lisävaatimuksilla.

![Kuva]({{ site.baseurl }}/ca06__grant_1_1.png)

Käytössä on kaksi päätapaa:

- **Grant access** (Myönnä pääsy tietyin ehdoin)
- **Block access** (Estä pääsy kokonaan)

#### Grant access -valinnan lisäehdot

Kun valitset *Grant access*, voit lisätä pääsyn ehdoksi yhden tai useamman seuraavista toimenpiteistä:

- **Require multi-factor authentication**  
  Käyttäjän on tunnistauduttava monivaiheisesti (MFA) ennen kuin pääsy sallitaan.

- **Require device to be marked as compliant**  
  Käyttäjän laitteen on oltava *vaatimusten mukainen* Microsoft Intunen hallinnan mukaan (esim. salattu levy, virustorjunta päällä, ajantasainen käyttöjärjestelmä).

- **Require Hybrid Azure AD joined device**  
  Laitteen on oltava liitetty sekä paikalliseen Active Directoryyn että Azure AD:hen – yleistä erityisesti organisaatioissa, joilla on hybridiympäristö.

- **Require approved client app**  
  Käyttäjän on käytettävä Microsoftin *hyväksyttyä sovellusta*, kuten Outlookia, Teamsia tai OneDrive-sovellusta – ei esimerkiksi kolmannen osapuolen sähköpostiohjelmaa tai selainta.

- **Require app protection policy**  
  Edellyttää, että sovellukselle on määritetty **App Protection Policy** (esim. estetään tiedostojen tallennus laitteen muistiin). Sovelluksen on oltava Intunen hallitsema ja suojattu.

- **Require password change**  
  Käyttäjän on vaihdettava salasanansa ennen pääsyä. Tämä on käytössä tietyissä riskiskenaarioissa (esim. korkea käyttäjäriski).

#### Valintalogiikka: "Require one of the selected controls" vs. "Require all the selected controls"

Kun valitset useita ehtoja, sinun on määritettävä, sovelletaanko:

- **Require one of the selected controls**  
  Riittää, että käyttäjä täyttää *yhden* valituista ehdoista.

- **Require all the selected controls**  
  Käyttäjän on täytettävä *kaikki* valitut ehdot päästäkseen sisään.

#### Block access

Tällä asetuksella pääsy estetään kokonaan valitun käytännön piiriin kuuluvissa tilanteissa. Tämä on selkeä tapa sulkea tietyt käyttäjät, laitteet tai olosuhteet kokonaan pois pääsystä.


### CA access controls - Session elementti

**Session controls** määrittävät, miten käyttäjän istuntoa hallitaan sen jälkeen, kun pääsy on myönnetty. Näillä asetuksilla voidaan rajoittaa käyttäjän toimintaa, hallita kirjautumisen kestoa ja parantaa turvallisuutta istunnon aikana.

![Kuva]({{ site.baseurl }}/ca07__session_1_1.png)

#### **Use app enforced restrictions**

Tämä asetus käyttää Microsoft 365 -sovellusten sisäänrakennettuja rajoituksia.  
Esimerkkejä:

- Outlook Web Access (OWA): estetään liitteiden lataaminen, tulostaminen tai kopiointi.
- SharePoint ja OneDrive: rajoitetaan tiedostojen käyttö vain selaamiseen.

> 🔹 Ei vaadi Defender for Cloud Apps -integraatiota.  
> 🔹 Sovellukset tunnistavat, että Conditional Access on voimassa, ja rajoittavat toimintoja sen mukaisesti.

#### **Use Conditional Access App Control**

Tällä asetuksella ohjataan liikennettä **Microsoft Defender for Cloud Apps** -palvelun kautta. Tämä mahdollistaa istuntopohjaisen valvonnan ja tarkat käytännöt sovellusten käyttöön.  
Kolme toimintavaihtoehtoa:

- **Monitor only** – Seuraa käyttöä ilman rajoituksia.
- **Block downloads** – Estää tiedostojen lataamisen tuetuista sovelluksista.
- **Use custom policy** – Käytä MDCA:ssa määriteltyjä tarkempia sääntöjä.

> ⚠️ Edellyttää Defender for Cloud Apps -lisenssiä ja konfiguroitua MDCA-yhdistämistä.

#### **Sign-in frequency**

Määrittää, kuinka usein käyttäjän on kirjauduttava uudelleen (uudelleenautentikointi).  
Asetus ohittaa Azure AD:n oletustokenien elinkaariasetukset.

- Voidaan asettaa tunneissa tai päivissä (esim. 1h, 12h, 7d, 90d).
- Vaikuttaa sekä salasanaan että MFA:han, riippuen käytännön kohteista.

> 🔹 Esimerkiksi käytäntö: "pakota kirjautuminen kerran vuorokaudessa riskialttiilla käyttäjillä".

#### **Persistent browser session**

Määrittää, jääkö selainistunto pysyväksi:

- **Always persistent** – Istunto säilyy selaimen sulkemisen jälkeen.
- **Never persistent** – Käyttäjän on kirjauduttava uudelleen selaimen sulkemisen jälkeen.
- **Use Azure AD default** – Noudattaa Entra ID:n oletusasetusta (esim. "Keep me signed in").

> 🔹 Vaikuttaa vain selainpohjaiseen kirjautumiseen.

#### **Customize continuous access evaluation (CAE)**

CAE mahdollistaa lähes reaaliaikaisen istuntojen hallinnan ilman, että odotetaan tokenin vanhenemista.  
Tällä asetuksella voi määrittää, mitä tapahtumia tulkitaan istunnon katkaisua aiheuttavina.

- Esimerkiksi: käyttäjä poistetaan ryhmästä, tili estetään, IP-osoite muuttuu → istunto päättyy heti.

> 🔹 Tukee Microsoft 365 -palveluita (esim. Exchange, SharePoint) ja jatkuvasti laajenevaa sovellusvalikoimaa.

#### **Disable resilience defaults**

Tällä asetuksella voidaan poistaa käytöstä Microsoftin oletuskäytäntöjä, jotka mahdollistavat kirjautumisen jatkumisen lyhyen katkoyhteyden tai tunnistautumishäiriön aikana (resilience).  
Jos käytäntö on **päällä**, kirjautuminen voi epäonnistua nopeammin, mutta se lisää turvallisuutta, erityisesti poikkeustilanteissa.

> ⚠️ Käyttö vaatii huolellista suunnittelua ja testausta – voi vaikuttaa loppukäyttäjän kokemukseen.

#### **Require token protection for sign-in sessions**  
*(Generally available for Windows, preview for macOS ja iOS)*

Tällä asetuksella otetaan käyttöön **Token Protection**, joka sitoo kirjautumiseen käytetyn tokenin tiettyyn laitteeseen.  
Estää tokenin varastamisen ja käytön toisella laitteella.

- Suojaa erityisesti selaimessa tai Office-sovelluksissa käytettyjä kirjautumisia.
- Toimii vain tuetuilla käyttöjärjestelmillä ja sovellusversioilla.

> 🔐 Parantaa merkittävästi istunnon turvallisuutta – suositeltava korkean suojaustason ympäristöihin.

#### **Use Global Secure Access security profile**

Tämä valinta käyttää Microsoftin **Global Secure Access** -ratkaisun (osa Microsoft Entra Internet Access / Private Access) turvallisuusprofiilia.  
Sitä käytetään kun halutaan yhdistää Conditional Access -hallinta Entra Network Access -ratkaisuihin.

> 🔹 Edellyttää Global Secure Access -konfiguraatiota ja lisenssejä.

---

## Conditional Access Templates

**Conditional Access Templates** ovat Microsoftin tarjoamia valmiita mallipohjia, joiden avulla voidaan nopeasti luoda tietoturvaa parantavia Conditional Access -käytäntöjä.  
Ne tarjoavat suositeltuja määrityksiä yleisimpiin skenaarioihin ja ovat hyödyllisiä niin aloitteleville kuin kokeneillekin ylläpitäjille.

### Miksi käyttää CA-templateja?

- Nopea ja helppo tapa ottaa käyttöön Microsoftin suosittelemia käytäntöjä
- Johdonmukaisuus eri ympäristöissä
- Toimivat hyvänä lähtökohtana manuaalisille käytännöille
- Mukautettavissa organisaation tarpeisiin ennen käyttöönottoa

> 💡 Template ei ole automaattisesti aktiivinen – voit tarkastella ja muokata sitä ennen kuin otat sen käyttöön.

### Templaten kategoriat (2025)

Microsoft jakaa CA-templatet seuraaviin **viitekehykseen perustuviin kategorioihin**:

#### Secure Foundation

Perustason suojauskäytännöt, jotka tulisi olla käytössä lähes kaikissa ympäristöissä.

**Esimerkkejä:**

- Estä perinteiset kirjautumismenetelmät (Block legacy authentication)
- Edellytä MFA kaikille käyttäjille
- Istuntojen hallinta (esim. sign-in frequency)
- Salli vain suojatut sovellukset (app protection)

#### Protect Administrator

Kriittisten roolien (kuten Global Administrator tai Privileged Role Administrator) suojaukseen tarkoitettuja käytäntöjä.

**Esimerkkejä:**

- MFA aina vaatimuksena admin-käyttäjille
- Rajoita kirjautumiset vain luotettuihin sijainteihin
- Estä pääsy ei-hallituilta laitteilta

#### Zero Trust

Käytännöt, jotka tukevat **Zero Trust -arkkitehtuuria**: pääsy myönnetään vain, jos konteksti on hyväksyttävä (ei sijainnin tai verkon perusteella).

**Esimerkkejä:**

- Edellytä laitteen olevan hyväksytty tai hallittu
- Käytä authentication context -ehtoja tiettyihin toimintoihin
- Käytä Continuous Access Evaluation -ominaisuuksia

---

#### Remote Network

Skenaariot, joissa käytetään pilviresursseja etäverkoista tai halutaan valvoa pääsyä tiettyjen verkkoyhteyksien kautta.

**Esimerkkejä:**

- Rajoita pääsy vain organisaation tunnetuista verkoista
- Käytä Global Secure Access -integraatiota
- Salli pääsy vain luotetuista sijainneista tai hybrid-verkosta

---

#### Emerging Threats

Käytännöt, jotka perustuvat uhkatilanteiden kehittymiseen ja riskiarviointiin. Vaativat yleensä **Entra ID P2** -lisenssin.

**Esimerkkejä:**

- Käytä Sign-in risk tai User risk ehtoja
- Edellytä lisätodennus riskitilanteissa
- Estä kirjautuminen epäilyttävässä kontekstissa automaattisesti

---

### Templaten käyttö käytännössä

1. Siirry Microsoft Entra -hallintaportaalissa kohtaan:  
   **Protection > Conditional Access > Templates**
2. Valitse haluamasi template
3. Tarkastele ja muokkaa esiasetuksia tarpeen mukaan
4. Tallenna ja määritä käytännön tila: **On**, **Off** tai **Report-only**

---

### Yhteenveto

| Ominaisuus | Kuvaus |
|------------|--------|
| **Käyttötarkoitus** | Nopea ja turvallinen Conditional Access -politiikan luonti |
| **Kategoriaesimerkit** | Secure foundation, Protect administrator, Zero Trust jne. |
| **Mukautettavuus** | Täysin muokattavissa ennen käyttöönottoa |
| **Lisenssivaatimus** | Entra ID P1 tai P2 riippuen käytännön ehdoista |
| **Käyttöönotto** | Entra admin centerissä Templates-välilehdeltä |

CA Templates on erinomainen lähtöpiste, jolla varmistetaan, että tietoturvakäytännöt pohjautuvat Microsoftin suosituksiin ja viimeisimpiin uhkamalleihin.

---

## Nimeämiskäytännöt

Kun Conditional Access politiikkoja ryhdytään hienosäätämään, niin politiikkojen määrä saattaa kasvaa hyvinkin paljon. Noudattamalla hyvää nimeämiskäytöntöä voidaan välttää hämmennystä siitä, mitkä käytännöt koskevat mitäkin. Ilman yhtenäistä nimeämiskäytäntöä voi olla vaikea päätellä, että mitä vaikutuksia yhden käytännön muuttamisella voi olla. Siksi on suositeltavaa suunnitella ja dokumentoida hyvä nimeämiskäytäntö ja noudattaa mallia kaikissa Conditional Access politiikoissa. 

Esimerkki nimeämiskäytäntö mallista: <CAnumber>-<Persona/User type>-< PolicyType>-<App>-<Platform>-<Grant>-<OptionalDescription>

Tuotannossa CA politiikoiden nimet voisivat näyttää esimerkiksi tältä:

- CA001-Global-BaseProtection-AllApps-AnyPlatforms-EntraIdJorCompliant
- CA002-Global-IdentityProtection-AllApps-AnyPlatforms-Block-BlockLegacy
- CA100-Admins-BaseProtection-AllApps-AnyPlatforms-Unmanaged
- CA101-Admins-AttackSurfaceReduction-AllApps-Unknown-Block
- CA200-Internals-AppProtection-O365-Windows10-EntraIdHJ
- CA201-Internals-AppProtection-O365-Windows10-Unmanaged
- CA202-Internals-DataProtection-O365-Windows10-Unmanaged-DLPSessioncontrol
- CA203-Internals-AppProtection-O365-iOSAndroid-EMAppProtection
- CA204-Internals-AppProtection-SPO-iOSAndroid
- CA300-Externals-BaseProtection-AllApps-AnyPlatform
- CA301-Guests-Compliance-AllApps-AllDevices-RequireTOU (TOU = Terms Of Use)
- CA401-GuestAdmins-Compliance-AllApps-AllDevices-RequireTOU (TOU = Terms Of Use)

Nimeämiskäytännässä numerointi pohjautuu seuraavankaltaiseen rakenteeseen:

- Global protection (CA001-CA099)
- Admins protection (CA100-CA199)
- Internals user protection (CA200-CA299)
- Externals user protection (CA300-CA399)
- Guests user protection (CA400-CA499)
- GuestAdmins user admins protection (CA500-CA599)
- Microsoft365ServiceAccounts (CA600-CA699)
- AzureServiceAccounts (CA700-CA799)
- CorpServiceAccounts (CA800-CA899)

## Hyvät käytännöt

- Käytä report-only tilaa ennen käytännön käyttöönottoa tuotantoympäristössä.
- Testaa sekä positiivisia että negatiivisia skenaarioita
- Käytä muutos- ja versionhallintaa CA-käytännöissä
- Automatisoi CA-käytäntöjen hallinta työkaluilla, kuten Azure DevOps/GitHub tai Logic Apps
- Sovella Zero Trust periaatteita Conditional Access käytäntöihin
- Rajoitta Block tilan käyttöä, vain tarvittaessa
- Varmista, että kaikki sovellukset ja alusta ovat suojattuja (CA:lla ei ole implisiittistä "estä kaikki" -käytäntöä)
- Suojaa käyttäjät, joilla on hallintaoikeuksia, kaikissa M365 RBAC -järjestelmissä
- Vaadi salasanan vaihto ja monitoiminen vahvistus korkean riskin käyttäjille ja kirjautumisille (identity protection)
- Rajoita pääsyä korkean riskin laitteilta (Intune-vaatimustenmukaisuuskäytäntö vaatimustenmukaisuuden tarkistuksella ehdollisessa pääsyssä)
- Suojaa etuoikeutetut järjestelmät (kuten Azure Mgt. Portal, AWS, GCP)
- Estä jatkuvat selainistunnot järjestelmänvalvojilta ja epäluotettavilta laitteilta
- Estä legacy (basic authentication) todennus
- Rajoita pääsyä tuntemattomilta tai ei-tuetuilta laitealustoilta
- Rajoita vahvojen tunnistetietojen rekisteröintiä

**CA-poikkeukset (Exclusions)**

- Käytä suojausryhmiä (security group) poissulkemisille (exclusions) (pilvipohjaiset ryhmät ovat optimaalisia, koska tällaisten ryhmien muutokset otetaan käyttöön nopeasti, toisin kuin on-premises synkronoidussa ryhmässä)
- Luo poissulkemisryhmä (exclusion group) jokaiselle käytännölle tai ehkä vain käytäntötyypille ja henkilölle, joka on normaalisti tyhjä ja jota käytetään väliaikaisten poissulkemisten määrittämiseen. Staattiset poissulkemiset määritetään suorana poissulkemismäärityksenä väliaikaisten poissulkemisten lisäksi.
- Poista hätäkäyttötilit (break glass) kaikista käytännöistä hätäkäyttötilien poissulkemisryhmällä
- Tarkista poissulkemisryhmäsi jäsenet säännöllisesti (esim. Access Review)
- Poista Entra ID Connect -palvelutilisi käytännöistä, jotka estäisivät sitä synkronoitumasta EntraC-palvelutilien poissulkemisryhmän kanssa

## CA käytäntöjen testaaminen - uudet käytännöt ja muutokset

### Testiryhmä-pohjainen testaus

Testiryhmä edustaa annettua käyttäjämäärää, ja ajatuksena on ensin soveltaa CA käytäntöä ryhmään 0, jossa on vain hyvin vähän testikäyttäjiä, ja jos/kun se toimii, ottaa sama käytäntö käyttöön testiryhmässä, jossa on enemmän käyttäjiä, kuten ryhmä 1, ryhmä 2 ja niin edelleen, ennen kuin sitä sovelletaan kaikkiin käyttäjiin.

### Muutoshallinta - vaiheistettu testaus ja käyttöönotto

Kun uusia käytäntöjä tai muutoksia olemassa oleviin käytäntöihin otetaan käyttöön, haluamme pystyä ottamaan ne käyttöön vaiheittain. Tämä tarkoittaa, että sovellamme uusia käytäntöjä rajoitettuun käyttäjäjoukkoon ja testaamme niitä. Palautteen perusteella otamme CA käytännön käyttöön suuremmalle käyttäjäjoukolle ja lopulta sovellamme sitä kaikkiin käyttäjiin, kun kaikki vaiheet on hyväksytty.

Jotkut yritykset saattavat haluta hallita näitä testikäyttäjiä ad hoc -tavalla ja sisällyttää merkitykselliset käyttäjät testiryhmään ja laajentaa jäsenyyttä kyseisessä ryhmässä, kunnes se on täysin testattu ja sovellettu kaikkiin käyttäjiin. CA käytäntöihin on kuitenkin odotettavissa jatkuvia muutoksia, ja on hyödyllistä, että muutosten käyttöönottoon on jäsennelty strukturoidumpi lähestymistapa. Tämä tarkoittaa myös helpompaa ja jäsennellympää viestintää ja yhteistyötä sidosryhmien kanssa yksittäisten persoonien eri ominaisuuksien tarkasta statuksesta.

**CA policy testiryhmät: **
- CA-Persona-Internals-Ryhmä0
  - Vain muutama 1-5 käyttäjää, jotka ovat mukana CA-käytäntöjen kehittämisessä, kuten CA-järjestelmänvalvojat.
    - CA:n järjestelmänvalvojien odotetaan käyttävän omia loppukäyttäjätunnuksiaan Ryhmä 0:n jäseninä, mutta vain Internals pernoonana. Järjestelmänvalvojien on ehkä luotava erilliset testitilit muille persoonille, jotta he voivat testata näiden henkilöiden käytäntöjä ilman, että heidän tarvitsee vaihtaa testiryhmää.
- CA-Persona-Internals-Ryhmä1
  - Muutamia IT-henkilöstön käyttäjä, jotka eivät ole mukana CA-käytäntöjen kehittämisessä, 5–10 käyttäjää
    - Määritä uusi CA-käytäntö tälle ryhmälle vasta, kun Ryhmä 0:n käyttöönotto ja käyttökokemus on hyväksytty.
- CA-Persona-Internals-Ryhmä2
  - Sekoitus IT-henkilöstöä ja loppukäyttäjiä, jotka ovat suostuneet olemaan staattisia CA ryhmä 2 -käyttäjiä ja ymmärtävät vaikutukset, 10-50 käyttäjää (yrityksen koosta riippuen)
    - Määritä uusi CA-käytäntö tälle ryhmälle vasta, kun Ryhmä 1:n käyttöönotto ja käyttökokemus on hyväksytty.
- All Internals CA-Persona-Internals
  - kaikki CA-Persona-Internals jäsenet
  - Tätä persoonaa edustava vakiotuotantoryhmä.

**Uuden CA-käytännön käyttöönottoprosessi on seuraava:**

- Luo uusi CA-käytäntö sen persoonaryhmän sisällä, jolle se on tarkoitettu (noudata CA-nimeämisstandardia persoonalle). Noudata nimeämisstandardia käytettävän käytännön tyypin mukaan, eli aseta se vain raportointitilaan, jotta se ei vaikuta tuotantoon ennen testausta. Jos haluat minimoida
loppukäyttäjille aiheutuvat seuraukset vain raportointitilassa, voit valita käytännön soveltamisen vain raportointitilassa ensin CA-Persona-Internals-Ryhmä0, lisätä sitten Ryhmä1:n, Ryhmä2:n ja Ryhmä3:n ja lopuksi asettaa sen CA-Persona-Internalsiksi.
- Anna käytännön toimia päivän tai kaksi - CA-työkirjojen ja kirjautumislokien mahdollisten virheiden perusteella varmistaen, että ne ymmärretään ennen jatkamista, ja tarkista, ovatko loppukäyttäjät valittaneet uusista kirjautumiskehotteista, erityisesti mobiililaitteilla, koska vain raportointi voi aiheuttaa odottamattomia kehotteita joissakin käyttötapauksissa.
- Mahdollisesti voit muokata käytäntöä ja jatkaa sen suorittamista vain raportti -tilassa muutaman päivän ajan ja varmistaa, että ongelmat on ratkaistu tai ymmärretty täysin ennen käytännön käyttöönottoa ensimmäisellä soitolla. 
- Määritä käytäntö CA-Persona-Internals-Ryhmä0:lle ja ota se käyttöön.
- Testaa ja varmista, että kaikki toimii odotetulla tavalla muutaman päivän ajan.
- Määritä käytäntö myös CA-Persona-Internals-Ryhmä1:lle (jotta sekä Ryhmä0- että Ryhmä1-ryhmät on määritetty).
- Testaa ja varmista, että kaikki toimii odotetulla tavalla muutaman päivän ajan.
- Määritä käytäntö myös CA-Persona-Internals-Ryhmä2:lle (jotta sekä Ryhmä0-, Ryhmä1- että Ryhmä2-ryhmät on määritetty).
- Testaa ja varmista, että kaikki toimii odotetulla tavalla muutaman päivän ajan.
- Määritä käytäntö CA-Persona-Internalsille. Uusi käytäntö on nyt täydessä tuotannossa.

**Muutokset tuotannossa oleviin CA-käytäntöihin**

Jos tuotannossa olevaa CA-käytäntöä on muutettava uuden CA-käytännön luomisen sijaan, ehdotettu käyttöönottoprosessi on hieman erilainen. Tällä menettelyllä varmistetaan, että tietoturvaominaisuuksia ei heikennetä uusia käytäntöjä testatessa, eli ei haluta vain poistaa olemassa olevaa käytäntöä käytöstä. 

- Pidä olemassa oleva/alkuperäinen käytäntö käynnissä, kun testaat muutosta kopioidussa käytännössä
vain raportointitilassa alla kuvatulla tavalla.
- Kopioi olemassa oleva/alkuperäinen käytäntö ja nimeä se uudelleen olemassa olevalla nimellä lisäämällä "staged-deployment" tai "test". Yritä olla johdonmukainen siinä, mitä sanamuotoja lisätään olemassa olevaan käytäntöön osoittamaan, että kyseessä on vaiheittainen käyttöönotto. Kopiointia ei vielä tueta portaalissa, jolloin sinun on luotava uusi käytäntö, jolla on sama sisältö kuin olemassa olevalla käytännöllä ja noudatettava nimeämistä. CI/CD:n kautta tehtyjen muutosten osalta CA-käytäntö pitäisi olla helppo kopioida käytäntömääritystiedostoon, kuten json-tiedostoon.
- Määritä kopioitu käytäntö CA-Persona-Internals-Ryhmä0:lle ja aseta se vain raportointitilaan, koska haluamme testata, mitä tapahtuu, jos tätä käytäntöä sovelletaan. 
- Tarkista työkirjat ja kirjautumislokit ja seuraa uuden testikäytännön mahdollisia virheitä ja muuta niitä vastaavasti.
- Lisää CA-Persona-Internals-Ryhmä1 ja CA-Persona-Internals-Ryhmä2 ja myöhemmin samalla, kun tarkistat työkirjat ja kirjautumislokit sekä mahdolliset loppukäyttäjien valitukset.
- Jos kaikki näyttää hyvältä, haluamme nyt ottaa sen käyttöön/pakottaa sen tuotannossa. Aloita sulkemalla CA-Persona-Internals-Ryhmä0 pois alkuperäisestä käytännöstä.
- Muuta kopioitu käytäntö kohdistumaan vain CA-Persona-Internals-Ryhmä0:een, vaihda se "vain raportointi" -tilasta "käytössä" -tilaan.
- Tarkista ja seuraa uuden testikäytännön mahdollisia virheitä ja tee tarvittavat muutokset
- Muuta alkuperäistä käytäntöä niin, että myös CA-Persona-Internals-Ryhmä1 jätetään pois käyttäjien/ryhmien määrityksestä
- Määritä CA-Persona-Internal-Ryhmä1 lisäsisällykseksi CA-Persona-Internals-testikäytäntöön
- Tarkista ja seuraa uuden testikäytännön mahdollisia virheitä ja tee tarvittavat muutokset
- Muuta alkuperäistä käytäntöä niin, että myös CA-Persona-Internals-Ryhmä2 jätetään pois käyttäjien/ryhmien määrityksestä
- Määritä CA-Persona-Internal-Ryhmä2 lisäsisällykseksi CA-Persona-Internals-testikäytäntöön
- Tarkista ja seuraa uuden testikäytännön mahdollisia virheitä ja tee tarvittavat muutokset
- Määritä testikäytäntö CA-Persona-Internalsille
- Muuta alkuperäinen käytäntö käytöstä pois päältä -tilaan
- Tarkista ja seuraa uuden testikäytännön virheitä ja tee tarvittavat muutokset
- Jos kaikki toimii oikein, poista alkuperäinen käytäntö
- Muuta testikäytännön nimi niin, ettei nimessä ole -test-päätettä, jotta sillä on nyt sama nimi kuin alkuperäisellä käytännöllä, mutta se toimii uusien muutosten kanssa.

## CA käytäntöjen ylläpidon automatisointi

Mikäli organisaation automaatio on niin kehittynyttä, niin voidaan käyttää Azure DevOps CI/CD -prosessia CA-käytäntöjen muutosten hallintaan manuaalisen tekemisen sijaan Azure-portaalissa. Tämä vähentää inhimillisten virheiden riskiä CA-käytäntöjä määritettäessä ja mahdollistaa muutosten hyväksyntäprosessien sisällyttämisen
sekä mahdollisuuden palata aiempiin käytäntöihin.

Tässä blogi kirjoituksessa en käsittele automatisointia mainintaa enempää.