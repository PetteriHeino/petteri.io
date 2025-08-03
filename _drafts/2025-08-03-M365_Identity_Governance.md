---
title: Ide
description: >-
 Tämän on tarkoitus olla blogi-sarjan ei-tekninen aloitus ja jatkan sarjaa blogi postauksina harjoituksista Azure opiskelujen edetessä. Sarjan focus on tarkoitus olla pääasiassa Infrastructure As Codessa.
authors: Petteri
date: 2025-02-22 21:50:00 +0200
categories: [M365]
tags: [SC-300, Microsoft, identity]
pin: false
media_subpath: '/assets/media/2025-02-27-azure_opiskelu'
---

# Johdanto

Mikä on tämän blogi kirjoituksen tarkoitus

# Microsoft 365 Identity Governance

**Microsoft 365 Identity Governance** on kokoelma palveluita ja ominaisuuksia, joiden avulla organisaatiot voivat hallita käyttäjien elinkaarta, käyttöoikeuksia ja varmistaa, että oikeilla henkilöillä on pääsy oikeisiin resursseihin – oikeaan aikaan ja oikeista syistä.

Se auttaa ratkaisemaan seuraavat asiat:
- Kenellä on pääsy mihinkin aineistoon?
- Mitä käyttäjät tekevät käyttöoikeudella?
- Mitä organisaation hallintatoimintoja käyttöoikeuksien hallintaan on käytössä?
- Voivatko tarkastajat (auditors) varmistaa, että hallintatoiminnot (controls) toimii?

Työntekijöiden on käytettävä resursseja, kuten ryhmiä, sovelluksia ja SharePoint-sivustoja, jotta he voivat suorittaa työnsä. Käyttöoikeuksien hallinta ajan mittaan on organisaatiolle haastavaa ja monimutkaistuu, kun uusia ryhmiä tai sovelluksia lisätään tai kun käyttäjät tarvitsevat lisäkäyttöoikeuksia. Siitä tulee myös monimutkaisempaa, kun organisaatiot tekevät yhteistyötä organisaationsa ulkopuolisten käyttäjien kanssa.
---

## Identity Governance palvelut ja ominaisuudet

### 1. Entitlement Management (oikeuksien hallinta)
- Mahdollistaa pääsyn hallittuihin "paketteihin" (Access packages), jotka voivat sisältää ryhmäjäsenyyksiä, sovellusoikeuksia ja SharePoint-sivustoja.
- Käyttäjät voivat hakea pääsyä itsepalveluna hyväksymiskierron kautta.
- Tukee myös ulkoisia käyttäjiä (B2B).

### 2. Access Reviews (käyttöoikeusarvioinnit)
- Auttaa säännöllisesti tarkistamaan, onko käyttäjillä edelleen tarpeellinen pääsy ryhmiin, sovelluksiin tai rooleihin.
- Hyväksyntä voidaan automatisoida tai delegoida vastuuhenkilöille.

### 3. Privileged Identity Management (PIM)
- Hallitsee korkean tason käyttöoikeuksia (esim. Azure AD ja Microsoft 365 -roolit).
- Mahdollistaa tilapäisen, hyväksynnän vaativan pääsyn ("Just-in-Time" käyttöoikeudet).
- Sisältää käyttöoikeuksien aktivointipyynnöt, ilmoitukset ja audit-lokit.

### 4. Lifecycle Workflows (esikatselussa / julkisesti saatavilla)
- Automaatiot käyttäjän elinkaaren vaiheisiin (esim. uusi työntekijä, roolin vaihto, työsuhteen päättyminen).
- Mahdollistaa mm. ilmoitukset, käyttöoikeuksien lisäämisen/poiston ja käyttäjän deaktivoinnin.

## 🔐 Lisenssivaatimukset ja hallintaroolit

### 💳 Identity Governance -komponenttien lisenssivaatimukset

| Komponentti                         | Vaadittu lisenssi                                 |
|-------------------------------------|---------------------------------------------------|
| Entitlement Management              | Microsoft Entra ID Governance (tai P2)           |
| Access Reviews                      | Microsoft Entra ID Governance (tai P2)           |
| Privileged Identity Management (PIM)| Microsoft Entra ID Governance (tai P2)           |
| Lifecycle Workflows                 | Microsoft Entra ID Governance                    |

> 💡 *Microsoft Entra ID Governance* -lisenssi sisältää kaikki yllä mainitut ominaisuudet ja korvaa aiemman **Azure AD Premium P2** -lisenssin käytön tässä yhteydessä.

---

### 👥 Tarvittavat käyttöoikeusroolit

Identity Governance -toimintojen hallintaan tarvitaan tietyt **Azure AD -hallintaroolit** riippuen siitä, mitä osaa halutaan hallita:

| Toiminto                              | Tarvittava rooli                                 |
|---------------------------------------|--------------------------------------------------|
| Access packagesin ja katalogien luonti | **Identity Governance Administrator** tai **Global Administrator** |
| Resurssien lisääminen katalogeihin    | **Catalog owner** (katalogikohtainen)            |
| Käyttöoikeusarviointien hallinta      | **Identity Governance Administrator**            |
| PIM-roolien määrittäminen             | **Privileged Role Administrator**                |
| Lifecycle Workflowsin hallinta        | **Identity Governance Administrator**            |
| Access package -pyyntöjen hyväksyntä  | **Määritelty hyväksyjä** (esim. esihenkilö)      |

> 🛡️ Käytännössä suositellaan, että **Identity Governance Administrator** -roolia käytetään hallintaan, ja delegoidaan hyväksynnät sekä katalogien hallinta omistajille organisaatiorakenteen mukaisesti.








---
# Entitlement Management Microsoft Entra ID:ssa

Entitlement Management on osa Microsoft Entra ID:n (entinen Azure AD) Identity Governance -ratkaisua. Sen avulla organisaatiot voivat hallita käyttäjien pääsyä resursseihin hallitusti ja itsepalveluna – sekä sisäisille että ulkoisille käyttäjille.

## 🔄 Kokonaisuuden rakenne

Entitlement Management rakentuu neljästä pääkomponentista:

### 1. **Katalogi (Catalog)**
- Katalogi toimii säiliönä, johon yhdistetään kaikki resurssit, jotka ovat käytettävissä access package -pakettien kautta.
- Yksi organisaatio voi luoda useita katalogeja esimerkiksi eri osastoille tai liiketoiminnoille.
- Jokaisella katalogilla on omat ylläpitäjänsä (Catalog owners), jotka hallinnoivat sen sisältöä.

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
2. Katalogiin lisätään:
   - "Marketing Team" -Microsoft 365 -ryhmä
   - SharePoint-sivusto "Marketing Assets"
   - Käyttöoikeus sovellukseen "Canva for Enterprise"
3. Luodaan access package "Marketing Starter Kit".
4. Uusi työntekijä pyytää pakettia itsepalveluportaalista.
5. Esihenkilö hyväksyy pyynnön.
6. Työntekijä saa automaattisesti tarvittavat käyttöoikeudet.

---

## 🎯 Hyödyt

- **Automaattinen ja hallittu pääsynhallinta**: vähemmän manuaalista työtä IT:lle.
- **Auditointi ja jäljitettävyys**: kaikki pyynnöt ja hyväksynnät tallennetaan.
- **Tuki ulkoisille käyttäjille (B2B)**: yhteistyökumppanit voivat hakea pääsyä hallitusti.
- **Vanhentumisen ja uusinnan hallinta**: ei jää "ikuisia" käyttöoikeuksia.

---

