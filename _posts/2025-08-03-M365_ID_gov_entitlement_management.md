---
title: Entra ID Identity Governance - Entitlement Management osa 1
description: >-
 Entitlement Management - katalogi, resurssit, käyttöoikeuspaketit, käyttöoikeuspyynnöt
authors: Petteri
date: 2025-08-03 09:00:00 +0200
categories: [M365]
tags: [SC-300, Microsoft, Entra ID, Identity Governance, Entitlement Management]
pin: false
media_subpath: '/assets/media/2025-08-03-entitlement_management'
---

## Johdanto

Tämä kirjoitus on osa blogikirjoitus sarjaa, missä käsittelen Entra ID:n palvelukokonaisuutta nimeltä Identity Governance. Tässä kirjoutuksessä keskityn Identityn Governancen osa-alueeseen nimeltä Entitlement Management. Kirjoitan Entitlement Management asiat kahdessa eri blogi kirjoituksessa:

1. Katalogi, resurssit, acccess package ja käyttöoikeuspyyntöjen hallinta
2. Käyttöehtojen hallinta, ulkoisten käyttäjien elinkaaren hallinta, Yhdistettyjen organisaatioiden määrittäminen, henkilökohtaisten oikeuksien tarkistaminen käyttöoikeuksien hallinnan avulla [Linkki kirjoitukseen](../M365_id_gov_entitlement_management2)  

Tämän blogisarjan tarkoitus on antaa lyhyt katsaus Microsoftin M365 teknologiaan Suomen kielellä, niin että siitä saa hetkessä kuvan mistä teknologiassa on kyse ja miten palvelua konfiguroidaan. Jos sitten haluaa perehtyä asiaan syvemmin, niin Microsoftin Learn sivustosta löytyy materiaali siihen.

Identity Governance on osa-alue Microsoftin SC-300 sertifiointitestissä.

## Entitlement Management Microsoft Entra ID:ssa

Entitlement Management on osa Microsoft Entra ID:n (entinen Azure AD) Identity Governance -ratkaisua. Sen avulla organisaatiot voivat hallita käyttäjien pääsyä resursseihin hallitusti ja itsepalveluna – sekä sisäisille että ulkoisille käyttäjille.

Kun työntekijät liittyvät organisaatioon, vaihtavat tiimiä tai aloittavat työskentelyn uudessa roolissa he tarvitsevat pääsyn erilaisiin resursseihin. Esimiesten on tunnistettava, mitä resursseja työntekijä tarvitsee ja kuinka kauan. Resurssien käyttöoikeuksien myöntäminen yksitellen on hankalaa ja aikaa vievää. Kun käyttäjä siirtyy pois tiimistä tai organisaatiosta pitää myös poistaa käyttöoikeudet.

### 🎯 Entitlement Management hyödyt

- **Automaattinen ja hallittu pääsynhallinta**: vähemmän manuaalista työtä IT:lle.
- **Auditointi ja jäljitettävyys**: kaikki pyynnöt ja hyväksynnät tallennetaan.
- **Tuki ulkoisille käyttäjille (B2B)**: yhteistyökumppanit voivat hakea pääsyä hallitusti.
- **Vanhentumisen ja uusinnan hallinta**: ei jää "ikuisia" käyttöoikeuksia.

## Käyttöoikeuksien hallinnan delegointi ja roolit

Ylläpitäjät voivat luoda ja hallita käyttöoikeuksia, he eivät kuitenkaan välttämättä tiedä kaikkia skenaarioita, joissa käyttöoikeuspaketteja tarvitaan. Asianomaisten osastojen käyttäjät tietävät, mitä resursseja työn suorittamiseen tarvitaan ja kuinka kauan. Käyttöoikeuksien hallinnan delegointi loppukäyttäjille varmistaa, että oikeat henkilöt hallitsevat käyttöoikeuksia.

Entitlement Managementissa käyttöoikeushallinta perustuu **rooleihin**, jotka määrittävät, kuka voi hallita mitäkin osaa – esimerkiksi katalogeja, access packageja tai pyyntöprosesseja.

| Rooli                     | Päävastuut                                         | Tarvitsee Entra-roolin? |
|---------------------------|---------------------------------------------------|--------------------------|
| **Identity Governance Administrator** | Hallitsee kaikkea Identity Governance -kokonaisuudessa | ✅ Kyllä |
| **Catalog Owner**         | Hallitsee tietyn katalogin sisältöä ja access packageja | ❌ Ei (määritetään katalogissa) |
| **Approver**              | Hyväksyy pääsypyynnöt tai uusinnat               | ❌ Ei (määritetään paketin asetuksissa) |
| **Requester**             | Tekee access package -pyyntöjä                   | ❌ Ei |
| **Access Package Manager**| Luo ja ylläpitää access packageja (Catalog Ownerin tehtävänä) | ❌ Ei |


## 🔄 Entitlement management kokonaisuuden rakenne

Entitlement Management rakentuu neljästä pääkomponentista:

### 1. **Katalogi (Catalog)**
- Katalogi toimii säiliönä, johon yhdistetään kaikki resurssit, jotka ovat käytettävissä access package -pakettien kautta.
- Yksi organisaatio voi luoda useita katalogeja esimerkiksi eri osastoille tai liiketoiminnoille.
- Jokaisella katalogilla on omat ylläpitäjänsä (Catalog owners), jotka hallinnoivat sen sisältöä.

![Kuva]({{ site.baseurl }}/catalog.png)

### 2. **Resurssit (Resources)**
- Resurssit ovat kohteita, joihin pääsyä voidaan hallita. Näitä voivat olla:
  - Microsoft 365 -ryhmät
  - Microsoft Teams -tiimit
  - SharePoint Online -sivustot
  - Azure AD -sovellukset ja roolit
- Resurssit lisätään ensin katalogiin, jonka jälkeen ne voidaan sisällyttää access packageihin.

### 3. **Access Packages (Pääsypaketit)**
- Access package on "paketti" resursseja, joiden käyttöoikeus voidaan myöntää yhdellä hyväksyntäpyynnöllä.
- Paketti määrittää:
  - Mitä resursseja siihen kuuluu
  - Kuka voi pyytää pääsyä (sisäiset tai ulkoiset käyttäjät)
  - Tarvitaanko hyväksyntää ja kuka hyväksyy
  - Kuinka kauan pääsy on voimassa (automaattinen vanhentuminen, uusintapyynnöt)

### 4. **Hyväksyntäprosessi (Approval Workflow)**
- Pääsypyynnöt voidaan hyväksyä:
  - Automaattisesti
  - Yksivaiheisella hyväksynnällä (esim. esihenkilö)
  - Kaksivaiheisella hyväksynnällä (esim. esihenkilö + tietoturva)
- Hyväksyjille voidaan määrittää aikaraja ja ilmoitukset
- Myönnetyt käyttöoikeudet kirjataan ja niitä voidaan tarkastella myöhemmin

---

## 📊 Esimerkki: Uuden työntekijän pääsy markkinointitiimiin

1. Markkinointiosastolla on oma kataloginsa.
2. Katalogiin lisätään resurssit:
   - "Marketing Team" -Microsoft 365 -ryhmä
   - SharePoint-sivusto "Marketing Assets"
   - Käyttöoikeus sovellukseen "Canva for Enterprise"
3. Luodaan access package "Marketing Starter Kit".
4. Uusi työntekijä pyytää pakettia itsepalveluportaalista.
5. Esihenkilö hyväksyy pyynnön.
6. Työntekijä saa automaattisesti tarvittavat käyttöoikeudet.

---
## Katalogin luominen

1. Kirjaudu entra.microsoft.com
2. Klikkaa ID Governance - Entitlement Management - Catalogs + New Catalog
3. Anna katalogille kuvaava nimi ja description kenttään kuvaus katalogista.
4. Valitse vaihtoehdot Käytössä- (Enabled) ja Käytössä ulkoisille käyttäjille (Enabled for external users) ja luo sitten luettelo valitsemalla Create. 
   - Käytössä: Valitse Kyllä, jos haluat ottaa luettelon käyttöön välitöntä käyttöä varten. Valitse Ei, jos haluat, että luettelo ei ole käytettävissä, ennen kuin aiot käyttää sitä. 
   - Käytössä ulkoisille käyttäjille: Valitse Kyllä, jos haluat sallia valitun ulkoisen hakemiston käyttäjien pyytää käyttöoikeuspaketteja tässä luettelossa. Valitse Ei, jos haluat, että luettelo ei ole ulkoisen hakemiston käyttäjien käytettävissä.


![Kuva]({{ site.baseurl }}/uusikatalogi.png)

### Lisää tarvittavat resurssit katalogiin

1. Klikkaa aikaisemmin luotua katalogia ja Resources
![Kuva]({{ site.baseurl }}/uusikatalogi2.png)
2. Klikkaa +Add resources
3. Lisää resurssit ja klikkaa Add
![Kuva]({{ site.baseurl }}/resurssit.png)

Resurssien lisääminen katalogiin: Voit sisällyttää katalogiin resursseja, kuten ryhmiä, sovelluksia ja SharePoint-sivustoja. Resurssien on oltava katalogissa, jotta ne voidaan sisällyttää käyttöoikeuspakettiin

Resurssit: Kuten kuvassa näkyy, Lisää resursseja -kohdassa on vaihtoehtoja luetteloon lisättävien resurssien valitsemiseen.
- Ryhmät: Ryhmät voivat olla Azure AD käyttöoikeusryhmiä tai Microsoft 365 -ryhmiä. Ryhmiä, jotka ovat peräisin paikallisesta tai Exchange Onlinesta jakeluryhminä, ei voi muokata Azure AD:ssä. 
- Sovellukset: Azure AD -yrityssovellukset, jotka sisältävät sekä SaaS- että toimialakohtaisia sovelluksia, jotka on integroitu Entra ID:hen.
- Sivustot: SharePoint Online -sivustot tai SharePoint Online -sivustokokoelmat.


## Luo ja määritä käyttöoikeuspaketit (access package)

Sen sijaan, että käyttöoikeuspaketit myöntäisivät käyttöoikeuden yksittäisille resursseille, ne auttavat myöntämään tai poistamaan käyttöoikeuksia sopivammalla tavalla seuraaville resurssityypeille:
- Azure AD -käyttöoikeusryhmien jäsenyys
- Microsoft 365 -ryhmien ja Teamsin jäsenyys
- Määritys Azure AD -yrityssovelluksiin (mukaan lukien SaaS ja mukautetut sovellukset)
- SharePoint-verkkosivustojen jäsenyys

Käyttöoikeuspaketit ovat sopivampia, kun:
- Työntekijät tarvitsevat rajoitetun käyttöoikeuden.
- Käyttöoikeudet, jotka edellyttävät esimiehen tai nimetyn henkilön hyväksyntää.
- Tietyn osaston resurssit on ryhmitelty yhteen.
- Yhden organisaation käyttäjien on käytettävä toisen organisaation resursseja.

1. Klikkaa katalogin Access Packages osiota ja avautuvasta näkymästä +New Access Package
![Kuva]({{ site.baseurl }}/AccessPackage1.png)
2. Basic välilehti: Lisää kuvaava nimi ja kuvaus käyttöoikeuspaketille
![Kuva]({{ site.baseurl }}/AccessPackage3.png)
3. Klikkaa Next: Resource roles
4. Valitse resurssit, joihin access package antaa oikeudet ja määritä resurssirooli. Valittavissa on vain resursseja, mitkä ovat aikaisemmin lisätty katalogiin.
![Kuva]({{ site.baseurl }}/AccessPackage4.png)
5. Klikkaa Next: Requests
6. Requests välilehti jakautuu neljään asetusten määrittelyosaan:
   - Users who can request access. Vaihtoehdot:
     - Hakemiston käyttäjät: Tämän vaihtoehdon avulla hakemiston käyttäjät ja ryhmät voivat pyytää käyttöoikeuspakettia. Voit valita vain tietyt hakemiston käyttäjät ja ryhmät tai kaikki hakemiston käyttäjät tai kaikki hakemiston käyttäjät ja vieraskäyttäjät pyytävät käyttöoikeutta tähän käyttöoikeuspakettiin.
     - Käyttäjät, jotka eivät ole hakemistossasi: Tämän vaihtoehdon avulla yhdistettyjen organisaatioiden käyttäjät voivat pyytää tätä käyttöoikeuspakettia. Voit valita tietyt yhdistetyt organisaatiot tai kaikki yhdistetyt organisaatiot tai kaikki käyttäjät kaikista yhdistetyistä organisaatioista, ja kaikki uudet ulkoiset käyttäjät voivat pyytää tämän käyttöoikeuspaketin käyttöoikeutta. 
     - Ei mitään (vain järjestelmänvalvojan suorat määritykset): Tämän asetuksen avulla järjestelmänvalvojat voivat määrittää tietyt käyttäjät suoraan tähän käyttöoikeuspakettiin.
   - Approval: Pyynnöt-välilehden hyväksyntäosa, jossa voit määrittää, tarvitaanko hyväksyntä ja pyynnön perustelu, kun käyttäjät pyytävät tätä käyttöoikeuspakettia. Hyväksyntäprosessissa on vaihtoehtoja yksi- tai kaksivaiheisen hyväksyntäprosessin määrittämiseen. Yksivaiheisella hyväksynnällä on vain yksi hyväksyjä. Hyväksyjä voi olla sisäinen tai ulkoinen sponsori tai valitut hyväksyjät. Kun valitset sisäisen tai ulkoisen sponsorin, voit määrittää varahyväksyjiä. Kaksivaiheista hyväksyntää varten kunkin vaiheen valittujen hyväksyjien on hyväksyttävä pyyntö.
   ![Kuva]({{ site.baseurl }}/AccessPackage5.png)
   - Email Notifications (Kirjoitusta tehdessä vielä preview vaiheessa)
   - Enable: Tässä osiossa voidaan määrittää onko access package ylipäätään käytössä ja tarvitaanko käyttöoikeuden hakemiseen jonkinlaista verified IDtä.
7. Klikkaa Next: Requestor Information
8. Klikkaa Next: Lifecycle
9. Määritä Lifecycle asetukset:
   - Expiration
     - Access package assignment expire
     - Assignments expire after (number of days)
     - Users can request specific timeline
   - Access Reviews
     - Require access reviews: Vaadi (tai älä vaadi) käyttöoikeuksien tarkistuksia Access Review toiminnolla
![Kuva]({{ site.baseurl }}/AccessPackage6.png)
10. Klikkaa Next: Rules
11. Klikkaa Next: Review + Create
12. Klikkaa Create

## Käyttöoikeuspyyntöjen hallinta

Kun käyttöoikeuspaketti on luotu, järjestelmänvalvojat jakavat Omat käyttöoikeudet -portaalilinkin käyttäjien kanssa tai käyttäjät voivat pyytää käyttöoikeuspaketteja kirjautumalla sisään Omat käyttöoikeudet -portaaliin. Kuvassa näkyy My Access -portaalin linkki käyttöoikeuspaketin yleiskatsaussivulla. Kun järjestelmänvalvoja jakaa Omat käyttöoikeudet -portaalilinkin sisäisten tai ulkoisten käyttäjien kanssa, käyttäjät voivat kirjautua sisään Omat käyttöoikeudet -portaaliin (https://myaccess.microsoft.com) nähdäkseen luettelon käyttöoikeuspaketeista, joita he voivat pyytää.

![Kuva]({{ site.baseurl }}/myaccess2.png)
Käyttäjät pyytävät käyttöoikeuspaketin käyttöoikeutta. Järjestelmänvalvojat määrittävät käyttäjille käyttöoikeuspaketin. Kaikki käyttäjät saavat sähköpostiviestin, joka sisältää linkin käyttöoikeuspakettiin, tai he voivat kirjautua sisään My Access -portaaliin (https://myaccess.microsoft.com) nähdäkseen luettelon käyttöoikeuspaketeista, joita he voivat pyytää.

![Kuva]({{ site.baseurl }}/myaccess3.png)

Kun käyttäjät ovat lähettäneet käyttöoikeuspaketin pyynnöt, hyväksyjät saavat sähköpostiviestin, joka sisältää linkin pyyntöjen hyväksymiseen tai hylkäämiseen. Hyväksyjä voi joko napsauttaa linkkiä kirjautuakseen My Access -portaaliin tai kirjautua suoraan My Access -portaaliin (https://myaccess.microsoft.com) ja siirtyä Hyväksynnät-välilehteen.

---

