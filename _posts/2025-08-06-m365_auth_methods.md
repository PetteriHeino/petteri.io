---
title: Microsoft Entra ID – Todennuskäytännöt (Authentication Methods)
description: M365 autentikointi metodit, niiden halllinta ja suositukset
authors: Petteri
date: 2025-08-06 09:00:00 +0200
categories: [M365]
tags: [SC-300, Microsoft, Entra ID, Authentication Methods]
pin: false
media_subpath: '/assets/media/2025-08-06_auth'
---
## Johdanto

Tämä blogikirjoitus on osa kirjoittamaani Microsoft SC-300 testi sarjaa, missä käsittelen kevyellä tasolla testissä käsiteltäviä asioita. Tarkoituksena on että kirjoituksista saa nopeasti ymmärryksen mistä asiassa on kyse ja kuinka perus konfigurointi tapahtuu. Kun haluaa syventää ymmärtämistä, niin kannattaa lukea jatkoksi Microsoft Learniä, mistä samat asiat löytyvät huomattavasti kattavammin ja tarkemmin (mutta ovat myös raskaampia lukea jos asia on täysin uusi).

Todennuskäytännöt (Authentication Methods) ovat yksi pieni osa SC-300 sertifiointitestiä. Oma kokemus testistä on se, että on hyvä tietää mitä kaikkia todennuskäytäntöjä on ja mikä niiden käyttösuositus on. Tarkemmalla tasolla kannattaa tietää Authenticator, Passkey ja Temporary Access Pass (TAP) ja näitä kannattaa tutkia MS Learnistä vähän tarkemmin.

## Microsoft Entra ID – Authentication Methods (Policies)

Microsoft suosittelee salasanattomia todennusmenetelmiä, kuten Windows Helloa, salasanoja (FIDO2) ja Microsoft Authenticator -sovellusta, koska ne tarjoavat turvallisimman kirjautumiskokemuksen. Vaikka käyttäjä voi kirjautua sisään muilla yleisillä menetelmillä, kuten käyttäjätunnuksella ja salasanalla, salasanat tulisi korvata turvallisemmilla todennusmenetelmillä.

Tässä alla on lista todennusmenetelmistä jotka löytyvät Microsoft Entra ID -hallintaportaalin kohdasta **Authentication methods → Policies**. 

![Kuva]({{ site.baseurl }}/auth1.png)

Mukana ovat menetelmän kuvaus, edellytykset, turvallisuustaso, riskit ja suositeltavuus. Joistakin on konfigurointiohjeet mukana (suositellut ja helpot esivaatimuksiltaan kuten Authenticator).

Tarkoituksena tällä listalla on saada todennusmenetelmät haltuun yleisellä tasolla.

---

## ✅ Microsoft Authenticator (push + numerovahvistus)

- Sovellus puhelimessa, joka hyväksyy kirjautumisen ilmoituksella tai numerolla
- **Edellytykset:** Älypuhelin, MFA-rekisteröinti
- **Turvallisuus:** Korkea
- **Suositus:** **Ensisijainen menetelmä**
- **Riskit:** Push bombing (useiden ilmoitusten hyväksyminen vahingossa), SIM-kaappaus fallback-menetelmänä

### Microsoft Authenticator (push + numerovahvistus) – käyttöönotto

Microsoft Authenticator on vahva ja käyttäjäystävällinen todennusmenetelmä, mutta sen käyttöönottoon liittyy sekä ylläpitäjän että loppukäyttäjän toimenpiteitä.

#### Ylläpitäjän toimet

-  **Ota käyttöön "Microsoft Authenticator" -menetelmä**
   - Sijainti: `Microsoft Entra admin center → Authentication methods → Policies`
   - Varmista, että menetelmä on **Enabled** ja kohdistettu sopivaan käyttäjäryhmään. Authenticator on oletuksena päällä, mikäli ei ole tehty muutoksia.

#### Loppukäyttäjän toimet

1. **Käyttäjä lataa ja asentaa Microsoft Authenticator -sovellus**
   - Saatavilla Androidille ja iOS:lle.

2. **Rekisteröi tilin sovellukseen**
   - Käyttäjä kirjautuu selaimella M365 palveluun, jossa käyttäjä saa kehotteen, missä häntä pyydetään antamaan lisätietoja ennen kuin tiliä voi käyttää -> next niin pitkään että kehoite näyttää QR koodin
   - Käyttäjä avaa Microsoft Authenticator -sovelluksen
     - valitsee ilmoitusten salliminen (jos sitä pyydetään)
     - valitsee oikeassa yläkulmassa olevasta Customize and control  -kuvakkeesta Add account
     - valitsee sitten Work or school account
     - Skannaa annettu koodi Microsoft Authenticator -sovelluksen QR-koodinlukijalla
     - Authenticator -sovellukseen lähetetään ilmoitus tilisi testaamiseksi. Vastaa ilmoitukseen ja tekee wizardin tehtävät loppuun asti.
  
  ![Kuva]({{ site.baseurl }}/auth4.png)

  **vaihtoehtoisesti** uutena autentikointi metodina
   - Mene osoitteeseen: [https://mysignins.microsoft.com/security-info](https://mysignins.microsoft.com/security-info)
   - Valitse `Security info → Add sign-in method → Microsoft Authenticator `
   - Skannaa QR-koodi sovelluksella.

3. **Käyttää sovellusta kirjautumisen yhteydessä**
   - Kun käyttäjä kirjautuu M365-palveluun, hän saa push-ilmoituksen Authenticator sovellukseen.
   - Jos number matching on käytössä:
     - Ruudulle ilmestyy numero, jonka käyttäjän tulee valita oikein sovelluksesta.

---

## ✅ Passkey (FIDO2 Security Key)

- Fyysinen turva-avain (USB/NFC), toimii ilman salasanaa
- **Edellytykset:** Tuettu selain, rekisteröity avain, käyttöönottopolitiikka sallii
- **Turvallisuus:** Erittäin korkea
- **Suositus:** **Ensisijainen menetelmä (salasanaton)**
- **Riskit:** Fyysisen avaimen katoaminen

Kirjoitan Passkey käyttöönotosta mahdollisesti myöhemmin erillisen kirjoituksen.

---

## ✅ Windows Hello for Business

- PIN- tai biometrinen kirjautuminen laitteeseen
- **Edellytykset:** TPM-laitteisto, Azure AD Join tai Hybrid Join
- **Turvallisuus:** Erittäin korkea
- **Suositus:** **Ensisijainen salasanaless-menelmä**
- **Riskit:** Käytettävissä vain rekisteröidyissä laitteissa

---

## ☑️ Temporary Access Pass (TAP)

- Kertakäyttöinen, aikarajoitettu kirjautumiskoodi
- **Edellytykset:** Entra ID P1/P2, admin määrittää TAPin
- **Turvallisuus:** Korkea
- **Suositus:** **Toissijainen menetelmä** Väliaikainen käyttöoikeustunnus (TAP) on määräaikainen salasana, joka voidaan määrittää kertakäyttöiseksi tai useille kirjautumisille. Käyttäjät voivat kirjautua sisään TAP-tunnuksella ottaakseen käyttöön muita salasanattomia todennusmenetelmiä. TAP helpottaa myös vahvan todennusmenetelmän palauttamista, jos käyttäjä kadottaa tai unohtaa sen.
- **Riskit:** Kertakoodin väärinkäyttö, jos jaetaan epävarmasti

### Ylläpitäjän toimet

**TAP-protokollan määrittäminen**

1. Kirjaudu **Microsoft Entra amdin centeriin**
2. Selaa kohtaan **Entra ID > Authentication methods > Policies**
3. Valitse käytettävissä olevien todennusmenetelmien luettelosta **Temporary Access Pass**
4. Valitse **Enable** ja valitse sitten käyttäjät, jotka haluat sisällyttää käytäntöön tai jättää siitä pois.
5. (Valinnainen) Valitse **Configure** välilehti muokataksesi väliaikaisen käyttöoikeuden oletusasetuksia, kuten enimmäiskäyttöaikaa tai pituutta, ja valitse **Update**.
6. Save

**Luo TAP käyttäjätunnukselle**

1. Kirjaudu **Microsoft Entra admin centeriin**.
2. Selaa kohtaan **Entra ID > Users**.
3. Valitse käyttäjä, jolle haluat luoda TAP-tilin.
4. Valitse **Authentication methods** ja valitse **Add authentication method**.
5. Valitse **Temporary Access Pass**.
6. Määritä mukautettu aktivointiaika tai kesto ja valitse **Add**.
7. Lisäyksen jälkeen TAP:n tiedot näytetään.
8. Valitse **OK**

### Loppukäyttäjän TAP käyttö
Yleisin tapa TAP:lle on rekisteröidä todennustiedot ensimmäisen kirjautumisen tai laitteen asennuksen yhteydessä ilman ylimääräisiä suojauskehotteita. Todennusmenetelmät rekisteröidään osoitteessa https://aka.ms/mysecurityinfo . Käyttäjät voivat myös päivittää olemassa olevia todennusmenetelmiä täällä.

1. Avaa verkkoselain ja siirry osoitteeseen **https://aka.ms/mysecurityinfo**.
2. Anna sen tilin UPN, jolle loit TAP-osoitteen, kuten tapuser@contoso.com.
3. Jos käyttäjä on mukana TAP-käytännössä, hän näkee näytön TAP-tunnuksensa syöttämiseksi.
4. Syötä Microsoft Entra -hallintakeskuksessa näytetty TAP.

---

## ☑️ OATH Software Token (esim. Microsoft tai Google Authenticator)

- Aikaperustainen koodi sovelluksessa
- **Edellytykset:** TOTP-yhteensopiva sovellus
- **Turvallisuus:** Hyvä
- **Suositus:** **Toissijainen menetelmä**
- **Riskit:** Phishing, manuaalinen koodinsyöttö on virheherkkä

---

## ☑️ OATH Hardware Token

- Fyysinen laite, joka näyttää TOTP-koodin
- **Edellytykset:** Admin rekisteröi laitteet
- **Turvallisuus:** Hyvä
- **Suositus:** **Toissijainen menetelmä**, erityistilanteisiin
- **Riskit:** Fyysinen katoaminen, rajoitettu käytettävyys

---

## ⚠️ SMS (tekstiviesti)

- Koodi lähetetään tekstiviestinä
- **Edellytykset:** Rekisteröity puhelinnumero
- **Turvallisuus:** Matala
- **Suositus:** **Vältettävä**, korkeintaan fallback-käyttöön
- **Riskit:** SIM-kaappaus, viestien sieppaus

---

## ⚠️ Puhelusoitto

- Automaattinen puhelu, jossa käyttäjä hyväksyy kirjautumisen
- **Edellytykset:** Rekisteröity puhelinnumero
- **Turvallisuus:** Matala
- **Suositus:** **Vältettävä**
- **Riskit:** Soitonsieppaus, huijaussoitot

---

## ☑️ Certificate-based Authentication (CBA)

- X.509-varmenteisiin perustuva autentikointi (esim. älykortti)
- **Edellytykset:** PKI-infrastruktuuri, CBA-konfiguraatio
- **Turvallisuus:** Erittäin korkea
- **Suositus:** **Erityistapauksiin**, esim. korkean varmuuden vaatimuksissa
- **Riskit:** Monimutkainen käyttöönotto ja hallinta

---

## ✉️ Email OTP

- **Lyhyt kuvaus:**  
  Kertakäyttöinen salasana (OTP) lähetetään käyttäjän ensisijaiseen sähköpostiosoitteeseen. Käytetään vain B2B-vieraskäyttäjien tunnistautumiseen.
- **Edellytykset käyttöönotolle:**  
  - Käytettävissä vain **B2B guest users** -tyyppisille tileille  
  - Oltava sallittu Authentication methods -käytännöissä  
  - Vieraskäyttäjän sähköpostin on oltava saavutettavissa  
  - Ei vaadi MFA-rekisteröintiä tai sovellusta
- **Turvallisuus:**  
  - Soveltuu matalan riskin tilanteisiin, kuten satunnaisiin vieraskäyttöihin  
  - Salaamaton sähköposti ei ole yhtä turvallinen kuin muut MFA-metodit  
  - Ei vahvaa sidosta fyysiseen laitteeseen
- **Suositus:**  
  - Käytä vain vieraskäyttöön silloin, kun vahvempaa todennusta ei voida käyttää  
  - Ei suositella sisäisille käyttäjille
- **Riskit:**  
  - Ei estä käyttäjän sähköpostin kaappausta (phishing, credential stuffing)  
  - Riippuvainen sähköpostin toimitettavuudesta (spam/viive)

---

## 🧾 QR Code Sign-In

- **Lyhyt kuvaus:**  
  Kirjautuminen tapahtuu Authenticator-sovelluksella skannaamalla jaetulla laitteella näkyvä QR-koodi. Soveltuu jaettujen laitteiden nopeaan käyttäjävaihtoon.

- **Edellytykset käyttöönotolle:**  
  - Oltava sallittu Authentication methods -käytännöissä  
  - Käyttäjän on rekisteröitävä Microsoft Authenticator -sovellus  
  - Laite on oltava **shared device mode** -konfiguroitu (Intune tai vastaava)  
  - Vain **Entra ID Premium P1/P2** -ympäristössä (koska vaatii jaettujen laitteiden hallintaa)  
  - QR-sign-in pitää ottaa käyttöön Intune-konfiguraatiossa
- **Turvallisuus:**  
  - Todennus perustuu vahvaan sidokseen laitteeseen ja käyttäjän sovellukseen  
  - QR-koodi vanhenee nopeasti eikä ole uudelleenkäytettävissä  
  - Ei vaadi salasanan syöttämistä → pienentää keylogger-riskiä
- **Suositus:**  
  - Suositellaan vain **shared device** -skenaarioihin (esim. terveydenhuolto, vähittäiskauppa)  
  - Ei sovellu henkilökohtaisten tai pysyvien työasemien kirjautumiseen
- **Riskit:**  
  - Käytön väärinkäyttö mahdollista, jos jaettu laite ei ole lukittuna  
  - Käyttöönotto vaatii tarkkaa hallintaa ja Intune-yhteensopivuuden



## Yhteenveto suosituksista

## ✅ Yhteenveto suosituksista

### 🔐 Ensisijaiset menetelmät
Nämä tarjoavat vahvan suojan, hyvän käyttökokemuksen ja phishing-resistenssin. Suositellaan aina ensisijaisesti, kun mahdollista.

- **Microsoft Authenticator (push + numerovahvistus)**
- **FIDO2-avaimet**
- **Windows Hello for Business**

### 🧩 Hyvät toissijaiset (tilannekohtaisesti suositeltavat)
Käyttöön tilanteissa, joissa ensisijaisia ei voida käyttää, tai osana fallback-strategiaa.

- **Temporary Access Pass** – tilapäinen pääsy esim. uusille tai lukkiutuneille käyttäjille
- **OATH (soft/hard token)** – sopii offline-ympäristöihin tai käyttäjille ilman älypuhelinta
- **CBA (Certificate-based authentication)** – erikoistilanteisiin (esim. hallittu kortti-infra)
- **QR code sign-in** – jaetuille laitteille esim. terveydenhuolto tai kioskiympäristö

### 🔄 Rajatusti hyväksyttävät (vain tietyissä skenaarioissa)
Näitä voidaan käyttää rajoitetuissa tapauksissa, mutta niitä ei suositella ensisijaisiksi metodeiksi.

- **Email OTP** – vain B2B guest -käyttöön, ei sisäiseen todennukseen

### 🚫 Vältettävät menetelmät
Heikko suojaustaso, alttiita kalastelulle tai teknisesti vanhentuneita. Suositellaan poistettavaksi tai rajoitettavaksi.

- **Tekstiviestit (SMS)**
- **Puhelut (Voice call)**
- **Pelkkä salasana (ilman MFA)**

### Mitä tarkoitetaan ensisijaisella ja toissijaisella menetelmällä?

"Ensisijainen" ja "toissijainen" eivät ole Microsoft Entra ID:n virallisia tai teknisesti määritettyjä termejä, vaan ne kuvastavat **suositeltavuutta ja käyttötilannetta** autentikointimenetelmien valinnassa.

- **Ensisijainen menetelmä** tarkoittaa todennusmenetelmää, jota **kannattaa suosia** aina kun se on mahdollista ottaa käyttöön. Se on yleensä:
  - Turvallinen (esim. phishing-resistentti)
  - Käyttäjäystävällinen
  - Microsoftin suosittelema

- **Toissijainen menetelmä** tarkoittaa vaihtoehtoista menetelmää, jota voidaan käyttää jos:
  - Ensisijaista ei voida käyttää (esim. käyttäjällä ei ole tarvittavaa laitetta)
  - Tarvitaan fallback- tai varamenetelmä
  - Kyseessä on erityistilanne (esim. väliaikainen kirjautuminen)

Tavoitteena on tarjota ensisijaisesti paras mahdollinen suojaus ja käyttökokemus, mutta samalla huolehtia siitä, että käyttäjillä on **aina käytettävissä vähintään yksi toimiva kirjautumistapa**

Vältettäviä menetelmiä ei pitäisi käyttää ensinkään, ellei ole jostain syystä pakko niin tehdä.

---


