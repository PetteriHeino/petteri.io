---
title: Entra ID Identity Governance - Entitlement Management osa 2
description: >-
 Entitlement Management - Terms Of Use (ToU), Yhdistetyt organisaatiot ja ulkoisten käyttäjien elinkaaren hallinta
authors: Petteri
date: 2025-08-04 03:00:00 +0200
categories: [M365]
tags: [SC-300, Microsoft, Entra ID, Identity Governance, Entitlement Management]
pin: false
media_subpath: '/assets/media/2025-08-04-entitlement_management2'
---
## Johdanto

Tämä kirjoitus on osa blogikirjoitus sarjaa, missä käsittelen Entra ID:n palvelukokonaisuutta nimeltä Identity Governance. Tässä kirjoutuksessä keskityn Identityn Governancen osa-alueeseen nimeltä Entitlement Management. Kirjoitan Entitlement Management asiat kahdessa eri blogi kirjoituksessa:

1. Katalogi, resurssit, acccess package ja käyttöoikeuspyyntöjen hallinta. [Linkki kirjoitukseen](../M365_ID_gov_entitlement_management)  
2. Käyttöehtojen hallinta, ulkoisten käyttäjien elinkaaren hallinta, yhdistettyjen organisaatioiden määrittäminen, henkilökohtaisten oikeuksien tarkistaminen käyttöoikeuksien hallinnan avulla

Tämän blogisarjan tarkoitus on antaa lyhyt katsaus Microsoftin M365 teknologiaan Suomen kielellä, niin että siitä saa hetkessä kuvan mistä teknologiassa on kyse ja miten palvelua konfiguroidaan. Jos sitten haluaa perehtyä asiaan syvemmin, niin Microsoftin Learn sivustosta löytyy materiaali siihen.

Identity Governance on osa-alue Microsoftin SC-300 sertifiointitestissä.

---

## Terms of Use -ehtojen käyttöönotto osana Entitlement Managementia

**Terms of Use (ToU)** on Microsoft Entran tarjoama ominaisuus, jonka avulla organisaatiot voivat edellyttää, että käyttäjät **hyväksyvät tietyt käyttöehdot** ennen kuin heille myönnetään pääsy resursseihin – kuten **access packageihin**.

ToU-ehdot ovat erityisen hyödyllisiä silloin, kun pääsyä tarjotaan **ulkopuolisille käyttäjille** tai tilanteissa, joissa organisaation resurssien käytölle halutaan määrittää selkeät pelisäännöt.

### Mitä käyttöehdot voivat sisältää?

- Tietoturvakäytännöt
- Tiedonluokitteluun liittyvät ohjeet
- GDPR- tai sopimusehdot ulkopuolisille yhteistyökumppaneille
- Organisaation sisäiset hyväksymisvelvoitteet

### Esimerkki: kun käyttäjä pyytää access packagea

1. Käyttäjä aloittaa pyynnön portaalissa
2. Ennen hyväksyntäprosessia tai käyttöoikeuden myöntämistä käyttäjälle näytetään käyttöehdot
3. Käyttäjän on hyväksyttävä ehdot ennen kuin hänelle voidaan antaa pääsy resursseihin

### Hyödyt lyhyesti

| Hyöty | Kuvaus |
|-------|--------|
| Suojaa resurssit | Edellyttää ehtojen hyväksymistä ennen pääsyä |
| Dokumentointi | Käyttäjän hyväksyntä kirjataan audit-lokeihin |
| Uusiminen | Käyttöehdot voidaan asettaa vanhenemaan automaattisesti |
| Monikielisyys | Voit ladata eri kieliversioita käyttöehdoista |

### Terms of Use -ehdon määrittäminen

#### Vaihe 1: Luo Terms of Use
A Global administrator, Security administrator ja Conditional Access administrator rooleilla on oikeus luoda Terms Of Use.

1. Siirry Microsoft Entra -portaaliin: [https://entra.microsoft.com](https://entra.microsoft.com)
2. Navigoi kohtaan **Identity Governance > Terms of use**
3. Klikkaa **+ New terms**
4. Täytä seuraavat kentät:
   - **Name**: esim. "Yhteistyökumppanin käyttöehdot"
   - **Display name**: nimi, joka näytetään loppukäyttäjälle
   - **Terms of use document**: valitse PDF-tiedosto, jossa ehdot on kuvattu (pdf), määritä kieli ja display name
   - **Require users to expand**: voit edellyttää, että käyttäjä avaa ehdot ennen hyväksymistä
   - **Require users to consent on every device**: Jos haluat loppukäyttäjien hyväksyvän käyttöehtosi jokaisella laitteella, jolla he käyttävät järjestelmää, aseta Vaadi käyttäjien suostumus jokaisella laitteella -asetus arvoon Käytössä . Käyttäjien on ehkä asennettava muita sovelluksia, jos tämä asetus on käytössä. Microsoft Entra B2B -käyttäjiä ei tueta.
   - **Expire consents**:Järjestelmänvalvojat voivat määrittää käyttöehtojen käytäntöjen vanhentumispäivän ja -tiheyden käyttämällä vanhentuvia suostumuksia. Jos Vanhene alkaen on määritetty tämän päivän päivämäärään ja tiheys on kuukausittain, käyttäjien on hyväksyttävä käyttöehtokäytäntö ja hyväksyttävä se uudelleen joka kuukausi. Jos Mike esimerkiksi hyväksyy käyttöehdot 1. tammikuuta, ensimmäinen vanhentumispäivä on 1. helmikuuta ja toinen vanhentumispäivä on 1. maaliskuuta. Jos Mike hyväksyy käyttöehdot 15. tammikuuta, ensimmäinen vanhenemispäivä on 1. helmikuuta ja toinen vanhenemispäivä on 1. maaliskuuta
   - **Duration before re-acceptance required (days)** (valinnainen) Määritä Kesto ennen uudelleenhyväksyntää -kentässä, kuinka monta päivää käyttäjän on hyväksyttävä käyttöehtokäytäntö uudelleen. Käyttäjät voivat noudattaa omaa aikatauluaan. Jos esimerkiksi määrität kestoksi 30 päivää ja jos Mike hyväksyy käyttöehtokäytännön 1. tammikuuta, ensimmäinen vanhentumispäivä on 1. helmikuuta ja toinen vanhentumispäivä on 1. maaliskuuta. Jos Mike hyväksyy käyttöehdot 15. tammikuuta, ensimmäinen vanhenemispäivä on 14. helmikuuta ja toinen vanhenemispäivä on 16. maaliskuuta. 
   - **Conditional access/enforce with conditional access policy templates**: Conditional Policya ei ole pakko määrittää Terms of Use asetusta luodessa -> Create conditional access policy later. Mukautetun käytännön (Custom policy) valitseminen avaa uuden Conditional Access policy mallin. 

![Kuva]({{ site.baseurl }}/tou1.png)

5. Valitse **Create**

#### Vaihe 2: Luo conditional access policy

Microsoft Entra -ympäristössä käyttöehtojen hyväksyntä toteutetaan nykyisin **Conditional Access -politiikan avulla**, ei suoraan access package -asetuksissa. Tämä mahdollistaa selkeämmän hallinnan, monipuolisemmat ehdot ja yhtenäisen käyttökokemuksen kaikissa sovelluksissa.

#### Käyttöönoton vaiheet:
1. Luo Terms of Use -ehdot PDF-muodossa ja lataa ne Entra-portaaliin (tehtiin edellisessä esimerkissä)
2. Luo dynaaminen ryhmä, johon access package lisää hyväksytyt käyttäjät
3. Luo Conditional Access -käytäntö, jossa:
   - Kohdistat politiikan tähän ryhmään
   - Määrität resurssit (esim. Teams, SharePoint)
   - Valitset käyttöehdot, jotka on hyväksyttävä. Aikaisemmin luotu Terms Of Use näkyy Grant osiossa valittavana pääsykontrollina.
    
    ![Kuva]({{ site.baseurl }}/capolicy1.png)

Näin käyttäjältä edellytetään käyttöehtojen hyväksymistä ennen pääsyä resursseihin – ja hyväksyntä kirjautuu audit-lokeihin.

> Terms of Use lisää tärkeän kerroksen vastuullista käyttöä ja sääntelyvaatimusten täyttämistä Entitlement Managementin yhteyteen – erityisesti B2B-skenaarioissa.

---

## Pääsynhallinta ja organisaation ulkopuoliset käyttäjät

Microsoft Entra Identity Governance tarjoaa tehokkaita välineitä resurssien käytön hallintaan ja valvontaan – myös silloin, kun yhteistyötä tehdään organisaation ulkopuolisten toimijoiden kanssa.

Entitlement Management toiminnallisuudessa ulkopuolisten käyttäjien pääsynhallinta määritellään yhdistetyllä organisaatiolla (connected organization).

### Mikä on yhdistetty organisaatio?

Yhdistetty organisaatio (Connected organization) on ulkoinen Entra ID -tenant tai muu identiteettilähde (esim. Microsoft-tilit), joiden käyttäjät voivat pyytää pääsyoikeuksia määritettyihin **access packageihin** (pääsykokonaisuuksiin). Näiden avulla voidaan antaa turvallisesti ja hallitusti pääsy esimerkiksi sovelluksiin, ryhmiin tai SharePoint-sivustoihin ilman jatkuvaa manuaalista hallintaa.

Yhdistettyjä organisaatioita voidaan määrittää neljällä eri skenaariolla: 
- Käyttäjät toisessa Entra ID hakemistossa
- Toisen ei Entra ID-hakemiston käyttäjät, jotka on määritetty suoraa liittoutumista varten
- Toisen ei Entra ID-hakemiston käyttäjät, joiden sähköpostiosoitteilla on sama toimialuenimi
- Microsoft tilin käyttäjät, joilla ei ole yhteistä organisaatiota

![Kuva]({{ site.baseurl }}/connectedorganization1.png)

Kuva esimerkissä Contosolla on kaksi yhdistettyä organisaatiota: Fabrikam ja Woodgrove. 

Fabrikam käyttää Entra ID:tä, ja Woodgrove on muu kuin Entra ID hakemisto. Yhdistettyjen organisaatioiden käyttäjät voivat pyytää käyttöoikeuspaketteja. Käyttäjät, joiden täydellinen käyttäjätunnus on woodgrove.com tai fabrikam.com vastaa Contosoon yhdistettyjä organisaatioita, ja he voivat pyytää käyttöoikeuspaketteja. 

Koska Fabrikam käyttää Entra ID:tä, käyttäjät voivat pyytää pääsypakettaja (Access Package) tunnuksen ollessa määritettynä mihin tahansa Fabrikam tenantiin liitettyyn vahvistettuun toimialueeseen, esim. "fabrikam.in".

Jos kertakäyttöinen salasanatodennus (OTP) on käytössä, näiden toimialueiden käyttäjät, joilla ei vielä ole Azure AD tilejä, todentavat OTP:n avulla, kun he käyttävät Contoson resursseja.

### Miksi yhdistettyjä organisaatioita tarvitaan?

- Mahdollistetaan **B2B (Business-to-Business)** -yhteistyö suojatusti ja hallitusti.
- Automaattinen pääsyn hallinta ulkopuolisille käyttäjille.
- Tukee itsepalvelupohjaista pääsynpyyntöä ja hyväksyntäprosesseja.
- Mahdollistaa käyttöoikeuksien elinkaaren hallinnan myös ulkopuolisille käyttäjille.

### Yhdistetyn organisaation määrittäminen

- Microsoft Entra -hallintaportaalissa: https://entra.microsoft.com
- Navigoi kohtaan **Identity Governance > Entitlement Management > Connected organizations**
- Valitse **Add connected organization**
- Anna organisaation nimi ja valinnainen kuvaus
![Kuva]({{ site.baseurl }}/connectedorganization2.png)

Yhdistetyillä organisaatioilla on kaksi tilaa: määritetty (Configured) ja ehdotettu (Proposed). Configured tilassa organisaatio on täysin määritetty ja käyttövalmis. Proposed tilassa yhdistetyn organisaation määritys on kesken, eikä sitä voi vielä käyttää pääsyhallinnassa.

- Klikkaa Next: Directory + Domain -painiketta ja avautuvalla välilehdellä klikkaa Add directory + domain
![Kuva]({{ site.baseurl }}/connectedorganization3.png)
  - Select directories + domains ruudussa voit kirjoittaa hakukenttään domain nimen etsiäksesi Entra ID hakemistoa. Voit lisätä tässä myös verkkotunnuksia, mitkä eivät kuulu mihinkään hakemistoon. (Huomioi B2B hyväksytyt ja estetyt domainit asetusten vaikutus.)
- Klikkaa Next: Sponsors ja lisää sponsori(t).  
  Sponsorit ovat hakemistossasi jo olevia sisäisiä tai ulkoisia käyttäjiä, jotka toimivat yhteyshenkilöinä suhteessa tähän yhdistettyyn organisaatioon. Sisäiset sponsorit ovat hakemistossasi olevia jäsenkäyttäjiä. Ulkoiset sponsorit ovat yhdistetyn organisaation vieraskäyttäjiä, jotka on aiemmin kutsuttu ja jotka ovat jo hakemistossasi. Sponsoreita voidaan käyttää hyväksyjinä, kun tämän yhdistetyn organisaation käyttäjät pyytävät pääsyä tähän käyttöoikeuspakettiin.

![Kuva]({{ site.baseurl }}/connectedorganization4.png)

- Klikkaa Next: Review + Create painiketta, tarkasta asetukset ovat kunnossa ja klikkaa Create

### Hallinta ja käyttö

#### Yhdistettyjen organisaatioiden hyödyntäminen access packageissa

Kun yhdistetty organisaatio on määritetty (Configured), voit käyttää sitä **access package** -kokonaisuuden määrittelyssä:
![Kuva]({{ site.baseurl }}/connectedorganization5.png)

- Käyttäjät yhdistetystä organisaatiosta voivat pyytää pääsyä
- Käyttöoikeuden hyväksyntä voi vaatia ulkopuolisen hyväksyjää tai sisäisen valvojan hyväksynnän
- Käyttöoikeuden kesto, uusiminen ja poistaminen voidaan automatisoida

##### Esimerkki access package -asetuksista

Access package: Project X – External collaborators  
Target resources: Microsoft Teams, SharePoint, Security Group  
Request policy: External users from "Fabrikam"  
Approval: Requires approval from internal sponsor  
Access duration: 180 days, with optional renewal

---
## Käyttäjäkohtaisten oikeuksien tarkistaminen käyttöoikeuksien hallinnan avulla

Oikeuksien hallinta auttaa tarkastelemaan, kenelle käyttöoikeuspaketit, käytäntö ja tila on määritetty. Jos käyttöoikeuspaketilla on asianmukainen käytäntö, voit määrittää käyttäjän suoraan käyttöoikeuspakettiin.

Yleinen järjestelmänvalvoja, käyttäjätietojen hallinnan järjestelmänvalvoja, käyttäjän järjestelmänvalvoja, luettelon omistaja, käyttöoikeuspaketin hallinta tai käyttöoikeuspaketin määritysten hallinta voi tarkastella, lisätä tai poistaa käyttöoikeuspakettien määrityksiä.

Jos haluat tarkastella, kenellä on määritys, kirjaudu sisään Azure-portaaliin, siirry kohtaan Käyttäjätietojen hallinta, valitse Käyttöoikeuspaketit ja avaa käyttöoikeuspaketti. Kuten kuvassa 4-18 näkyy, valitse vasemmasta navigointivalikosta Tehtävät, niin näet luettelon aktiivisista tehtävistä.

- Navigoi "ID Governance -> Access Packages" ja klikkaa haluttua access packagea -> assignments

![Kuva]({{ site.baseurl }}/assignments1.png)
- Ylläpitäjä voi lisätä assignmentin käyttäjälle klikkaamalla "+ New assignment" ilman että käyttäjän tarvitsee hakea käyttöoikeutta itse. Lisäksi politiikasta riippuen ylläpitäjä voi ohittaa hyväksyntä prosessin.

---
## Ulkoisten käyttäjätunnusten elinkaari asetukset

Microsoft Entra Identity Governance tarjoaa mahdollisuuden hallita **ulkoisten käyttäjien elinkaarta** automaattisesti. Kun ulkopuolinen käyttäjä (esim. yhteistyökumppani tai toimittaja) ei enää tarvitse pääsyä organisaation resursseihin, voidaan hänen tililleen määrittää toimenpiteitä – kuten pääsyn estäminen tai tilin poistaminen – automaattisesti **Control configuration** -asetusten avulla.

Tämä on erityisen hyödyllistä, kun käyttäjät saavat pääsyn esimerkiksi Entitlement Managementin kautta access packageilla ja pääsy vanhenee automaattisesti.

### Control configuration -asetukset

Control configuration -asetukset löytyvät Entra-portaalista kohdasta:

**ID Governance > Control configurations > Lifecycle of external users**
- Klikkaa view settings

![Kuva]({{ site.baseurl }}/lifecycle_ext.png)

Seuraavat asetukset koskevat ulkoisten käyttäjien elinkaarta:


#### **Remove external user from the directory**

- **Kuvaus:**  
  Jos käyttäjä on **ulkopuolinen (guest)** ja hänellä **ei ole enää voimassa olevia access package -pääsyjä**, hänen tilinsä voidaan poistaa kokonaan tenantista automaattisesti.

- **Toiminto:**  
  Poistaa käyttäjän Microsoft Entra ID:stä.

- **Hyödyt:**  
  - Siivoaa vanhat ja tarpeettomat B2B-käyttäjätilit
  - Vähentää tietoturvariskiä
  - Tukee Zero Trust -mallia

#### **Block external user from signing in to the directory**

- **Kuvaus:**  
  Jos ulkoinen käyttäjä ei enää tarvitse pääsyä (esim. kaikki access package -oikeudet on poistettu), voidaan estää hänen kirjautumisensa ilman että käyttäjätiliä poistetaan heti.

- **Toiminto:**  
  Asettaa käyttäjän `SignInBlocked = true`.

- **Käyttötarkoitus:**  
  - Antaa siirtymäajan ennen tilin lopullista poistamista
  - Soveltuu ympäristöihin, joissa yhteistyö voi jatkua myöhemmin


#### **Number of days before removing external user from the directory**

- **Kuvaus:**  
  Määrittää kuinka monta päivää käyttäjän kaikki access package -pääsyoikeudet voivat olla poissa, ennen kuin **käyttäjätilille suoritetaan valittu toimenpide** (esim. estäminen tai poistaminen).

- **Oletusarvo:**  
  30 päivää

- **Käyttäytyminen:**  
  Kun käyttäjällä ei ole enää pääsyä mihinkään access packageen, laskuri alkaa. Kun määritetty määrä päiviä täyttyy:
    - Käyttäjä joko estetään (sign-in block), tai
    - Poistetaan (delete), riippuen asetuksista


### Esimerkki käytöstä

Asetukset:
- Block external user from signing in: ✅
- Remove external user: ✅
- Number of days: 14

→ Käyttäjällä ei ole enää access package -oikeuksia.
→ 14 päivän kuluttua: käyttäjän kirjautuminen estetään.
→ Poistetaan kokonaan tenantista seuraavana päivänä.

