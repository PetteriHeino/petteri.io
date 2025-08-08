---
title: Microsoft Entra ID – Identity Protection osa 1
description: SSPR - käyttöönotto ja testaus
authors: Petteri
date: 2025-08-08 05:01:00 +0200
categories: [M365]
tags: [SC-300, Microsoft, Entra ID, Identity Protection]
pin: false
media_subpath: '/assets/media/2025-08-08_sspr'
---
## Johdanto

Tämä blogikirjoitus on osa kirjoittamaani Microsoft SC-300 testi sarjaa (kirjoitain aiheista, mitkä kuuluvat testialueeseen), missä käsittelen kevyellä tasolla testissä käsiteltäviä asioita. Tarkoituksena on että kirjoituksista saa nopeasti ymmärryksen mistä asiassa on kyse ja kuinka perus konfigurointi tapahtuu. Kun haluaa syventää ymmärtämistä, niin kannattaa lukea jatkoksi Microsoft Learniä, mistä samat asiat löytyvät huomattavasti kattavammin ja tarkemmin (mutta ovat myös raskaampia lukea jos asia on täysin uusi).

Identity Protection on laaja kokonaisuus, koska sen signaalit eivät rajoitu pelkästään Entraan – niitä hyödynnetään myös Microsoft Defenderissä, Sentinelissä ja muissa SIEM-järjestelmissä osana laajempaa uhkien havaitsemista ja reagointia. Tässä kirjoituksessa keskitytään vain sen käyttöönottoon kirjautumisen yhteydessä, kun taas signaalien tulkinta ja operatiivinen käsittely vaativat oman syventävän tarkastelunsa. Kirjoitan myöhemmin jatko-osan tälle blogikirjoitukselle.

## Mikä on Entra ID Protection?

**Microsoft Entra ID Protection** on pilvipohjainen tietoturvapalvelu, joka auttaa organisaatioita suojaamaan käyttäjätunnuksiaan kehittyneiltä uhkilta. Se hyödyntää koneoppimista ja Microsoftin laajaa tietoturvadataverkostoa havaitakseen ja reagoidakseen epäilyttäviin kirjautumistoimintoihin lähes reaaliaikaisesti.

### Riskien havaitseminen

Entra ID Protection tunnistaa automaattisesti riskejä kolmessa eri kategoriassa:

- **Risky sign-ins** – Epäilyttävät kirjautumisyritykset (esim. sijaintihyppäykset, epäluotetut IP-osoitteet)
- **Risky users** – Käyttäjät, joiden tilit näyttävät olevan vaarantuneet
- **Risk detections** – Tarkat tiedot havaituista riskitekijöistä, kuten torjutuista hyökkäyksistä tai epätavallisesta toiminnasta

### Riskien tutkiminen ja korjaaminen

Kun riski havaitaan, Entra ID Protection antaa mahdollisuuden:

- **Tutkia tapahtumia** – Analysoida kirjautumisiin ja käyttäjiin liittyviä tietoja riskin lähteen ymmärtämiseksi
- **Korjata riskit** – Joko **automaattisesti** (esim. pakotettu MFA-tarkistus tai salasanan vaihto) tai **manuaalisesti** (esim. järjestelmänvalvojan tekemä tilin palautus tai tutkimus)

### Datan hyödyntäminen ja integraatiot

ID Protectionin tuottamaa dataa voidaan viedä ja hyödyntää laajasti esimerkiksi:

- **Microsoft Sentinelissa** – Tapahtumien yhdistäminen muuhun lokidataan ja uhkien korelointi
- **SIEM-järjestelmissä** – API- tai logiikkavirtojen avulla tietojen rikastaminen ja keskitetty seuranta
- **Power Automate tai Logic Apps** -ratkaisuissa – Reaktiivisten toimenpiteiden automatisointi organisaation prosesseihin

> Entra ID Protection ei ole pelkkä kirjautumislogien tarkkailija – se toimii aktiivisena osana nykyaikaista Zero Trust -strategiaa.

### Microsoft Defender ja Identity Protectionin signaalit

Useat Microsoft Defender -sovellukset pystyvät hyödyntämään Entra ID Protectionin tuottamia riskisignaaleja osana omaa uhkatunnistustaan, valvontaa tai automaatiota.

#### Microsoft Defender for Cloud Apps (Defender for Cloud Apps / MCAS)

- Kuluttaa signaaleja kuten:
  - `Risky sign-ins`
  - `Risky users`
  - Analysoi käyttäjän istuntoja reaaliaikaisesti (Conditional Access App Control)
- Voi käyttää riskitasoja esimerkiksi ohjaamaan sessiopohjaisia käyttöehtoja (esim. estä tiedoston lataus, vaadi MFA)
- Integroitu suoraan Entra Conditional Accessin kanssa

#### Microsoft Defender for Endpoint (MDE)

- Ei suoraan käytä Identity Protectionin signaaleja, mutta:
  - Voi lähettää käyttäjäkohtaisia uhkatietoja (esim. risk score) takaisin Entraan, jolloin kokonaisriskipiste nousee
- Integraatio Microsoft XDR-ekosysteemin kautta (Microsoft 365 Defender)
- Identity Protectionin päätöksenteko vaikuttaa epäsuorasti pääsynhallintaan (esim. blokkaa kirjautuminen korkean riskin perusteella)

#### Microsoft Defender for Office 365

- Ei suoraan käytä Identity Protectionin riskejä
- Voi vaikuttaa käyttäjän riskitasoon epäsuorasti (esim. huijausviestit voivat laukaista käyttäjän kompromissiepäilyn)
- Signaaleja rikastetaan Microsoft 365 Defenderissä

#### Microsoft Defender for Identity (entinen Azure ATP)

- Havaitsee epänormaaleja AD-käyttäytymisiä, joita voidaan yhdistää Entra ID:n riskeihin
- Ei käytä Identity Protectionin signaaleja suoraan, mutta:
  - Signaalit yhdistetään XDR:ssä osaksi käyttäjän kokonaisturvaprofiilia

#### Microsoft 365 Defender (XDR)

- Kuluttaa signaaleja **kaikista lähteistä**, mukaan lukien:
  - `Risky sign-ins`
  - `Risky users`
  - Defender for Endpointin, Cloud Appsin ja Office 365:n tapahtumat
- Yhdistää käyttäjän kontekstin, uhkatapahtumat ja tunnistautumisriskit yhdeksi kokonaiskuvaksi
- Käytössä myös automaattinen uhkien vastaus (AutoIR)

---

### Yhteenveto signaalien käytöstä

| Defender-tuote | Käyttää Entra ID Protection -signaaleja? | Huomio |
|----------------|------------------------------------------|--------|
| **Defender for Cloud Apps** | ✅ Suoraan | Istuntopohjaiset kontrollit, riskiperustainen käyttö |
| **Defender for Endpoint** | ⚠️ Epäsuorasti | Lähettää riskejä takaisin Entraan |
| **Defender for Office 365** | ⚠️ Epäsuorasti | Riskejä analysoidaan XDR-tasolla |
| **Defender for Identity** | ⚠️ Epäsuorasti | Oma riskianalyysi, yhdistyy XDR:ssä |
| **Microsoft 365 Defender** | ✅ Suoraan | Yhdistää kaikki signaalit analytiikkaan |

> **Huom:** Identity Protectionin signaalit ovat erityisen hyödyllisiä, kun ne yhdistetään Microsoft 365 Defender -ympäristöön, jossa kokonaisvaltainen uhka-analyysi tapahtuu eri puolilta tulleiden tietojen perusteella.

---

## Identity Protection vaatimukset

### Lisenssit

Identity Protectionin eri ominaisuudet edellyttävät Microsoft Entra -lisenssejä.

| Kyvykkyys | Vaadittu lisenssi |
|-----------|-------------------|
| Risky sign-ins -näkymä | **Microsoft Entra ID P2** |
| Risky users -näkymä | **Microsoft Entra ID P2** |
| Risk detection -data | **Microsoft Entra ID P2** |
| Automaattinen riskien korjaus (esim. salasanan nollaus, MFA) | **Microsoft Entra ID P2** |
| Riskipohjaiset ehdollisen käytön säännöt (Conditional Access) | **Microsoft Entra ID P1** *(vain perusehdot)* / **P2** *(riskiperusteinen logiikka)* |
| Riskitiedon vienti Microsoft Sentinel/SIEM:iin | **Microsoft Entra ID P2** + erillinen Sentinel-/SIEM-lisenssi |
| Riskiraporttien tarkastelu (read-only) | **Microsoft Entra ID P2** |
| Riskipohjaisten toimintojen automatisointi (esim. Power Automate) | **Microsoft Entra ID P2** + tarvittavat Power Platform -lisenssit |
| **Sähköposti-ilmoitukset (Notifications)** | **Microsoft Entra ID P2** |
| **MFA Registration Policy** (pakollinen MFA-rekisteröinti tietyille käyttäjille) | **Microsoft Entra ID P2** |
| **Microsoft Graph API -integraatiot Identity Protectionin riskitietoon** | **Microsoft Entra ID P2** |

> **Huom:** Identity Protectionin kaikki riskitietoihin perustuvat toiminnot ja integraatiot, mukaan lukien API-pääsy Microsoft Graphin kautta, vaativat **Entra ID P2** -lisenssin.

### Käyttöoikeudet

Identity Protectionin hallinta edellyttää tiettyjä Microsoft Entra -rooleja. Alla on yhteenveto tärkeimmistä rooleista, niiden kyvykkyyksistä ja rajoituksista:

- **Security Reader**
  - ✅ Voi tarkastella riskiraportteja ja -havaintoja: `Risky users`, `Risky sign-ins`, `Risk detections`
  - ❌ Ei voi määrittää käytäntöjä tai tehdä korjaavia toimenpiteitä

- **Security Operator**
  - ✅ Kuten Security Reader, lisäksi voi merkitä riskejä käsitellyiksi
  - ❌ Ei voi luoda tai muokata käytäntöjä

- **Security Administrator**
  - ✅ Täydet oikeudet Identity Protectionin asetuksiin, käytäntöihin ja korjaaviin toimenpiteisiin
  - ❌ Ei automaattisesti pääsyä muiden palveluiden hallintaan (riippuu rooliasetuksista)

- **Global Administrator**
  - ✅ Täydet oikeudet kaikkiin asetuksiin, mukaan lukien Identity Protection
  - ❌ Ei teknisiä rajoituksia, mutta hallintakäytännöt voivat rajoittaa käyttöä

- **Conditional Access Administrator**
  - ✅ Voi hallita ehdollisen käytön sääntöjä, myös riskeihin perustuvia
  - ❌ Ei pääsyä tarkastelemaan yksittäisiä riskiraportteja ilman lisäroolia
- **Global Reader**
  - ✅ Voi tarkastella kaikkia asetuksia ja raportteja organisaatiossa, myös Identity Protection -dataa
  - ❌ Ei voi tehdä muutoksia tai suorittaa toimintoja
- **User Administrator**
  - ✅ Voi palauttaa käyttäjän tilin, pakottaa salasanan vaihdon ja hallita käyttäjäobjekteja
  - ❌ Ei pääsyä riskiraportteihin eikä oikeuksia muuttaa Identity Protection -käytäntöjä
  - ⚠️ Voi kuitenkin olla mukana riskienhallintaprosessissa esimerkiksi salasanan palauttajana


> **Huom:** Identity Protectionin käyttöoikeudet riippuvat sekä käyttäjän roolista että lisenssitasosta.

## Käyttöönotto: Entra ID Protection

Tässä artikkelissa esitellään vaiheittain, kuinka otat Entra ID Protection -palvelun käyttöön organisaatiossasi.

### Vaihe 0 – Esivalmistelut (Prerequisites)

- Varmista, että sinulla on toimiva Microsoft Entra -vuokraaja ja käytössä **Entra ID P2** -lisenssi tai P2-kokeilu.  
  Lisäksi jotkin riskitapahtumat edellyttävät **Microsoft 365 E5** tai **Enterprise Mobility + Security E5** -lisenssiä.
- Varmista sopivat roolit:
  - **Lukuoikeudet**: Security Reader tai Global Reader  
  - **Hallinta**: Security Operator tai Security Administrator  
  - **Conditional Access -hallinta**: Conditional Access Administrator tai Security Administrator
- Luo testikäyttäjä (ei hallinnon tili) ja ryhmä, joiden avulla voit testata politiikkoja turvallisesti
- Määrittele projektin sidosryhmät ja vastuut selkeästi – onnistumisen edellytys

### Vaihe 1 – Tarkastele nykytilaa

Aloita Identity Protectionin käyttöönotto selvittämällä organisaation nykyinen riskiprofiili.

- **Selaa Entra -> ID Protection -> Dahshboard (tämä vie vanhaan portaali näkymään) valitse Risk detections"**
  - Täällä näet kaikki havaitut riskitapahtumat, joita Microsoftin analytiikka on tunnistanut käyttäjien kirjautumisista tai käyttäytymisestä
  - Kukin havainto sisältää tiedot:
    - Riskityypistä (esim. `Anonymous IP address`, `Impossible travel`)
    - Vakavuusasteesta (Low / Medium / High)
    - Aikaleimasta ja sijainnista
    - Mihin käyttäjään tapahtuma liittyy
    - Onko riski vielä avoin vai jo käsitelty

- **Hyödynnä "Investigate" tai "Details"-toimintoa yksittäisen riskin tarkasteluun**
  - Tutki, onko kyseessä aito uhka vai mahdollisesti väärä positiivinen havainto (esim. matkustaminen VPN-yhteydellä)
  - Tarkastele sign-in historyä, käytettyjä sovelluksia ja päätelaitteita
  - Käytä tietoa päätöksenteon tukena esimerkiksi riskin merkitsemisessä käsitellyksi

- **Analysoi toistuvia kuvioita**
  - Onko tietyillä käyttäjillä jatkuvasti kirjautumisyrityksiä epäilyttävistä IP-osoitteista?
  - Näkyykö paljon "Impossible travel" -tapahtumia, jotka voisivat viitata VPN- tai mobiilikäyttöön?

- **Valmistaudu politiikan luontiin**
  - Tämä vaihe antaa arvokasta tietoa siitä, kuinka aggressiivisia tai sallivia korjauspolitiikoiden tulisi olla
  - Voit myös käyttää tietoa suodattamaan käyttäjäryhmiä, joita ei kannata vielä ottaa politiikkojen piiriin

> Tavoitteena on tunnistaa, missä määrin riskien havaitseminen on jo käynnissä – ja kuinka tarkkaa ja hyödyllistä havaittu data on ennen automaattisten toimenpiteiden käyttöönottoa.


### Vaihe 2 – Suunnittele Conditional Access -politiikat

- Suunnittele politiikat, jotka hyödyntävät signaaleja kuten **sign-in risk** ja **user risk**: esimerkiksi MFA-vaatimus tai salasanan vaihtaminen riskitapauksissa.
- Älä unohda poissulkea:
  - **Break-glass-tilit** (varatilit) lukittumisen estämiseksi
  - **Service principals** / palvelutilit, jotka eivät ole käyttäjäperusteisia.

### Vaihe 3 – Varmista MFA-rekisteröinti

Jotta riskipohjaiset käytännöt toimivat oikein – esimerkiksi kun vaaditaan käyttäjää vahvistamaan henkilöllisyytensä MFA:lla epäilyttävän kirjautumisen jälkeen – on tärkeää varmistaa, että käyttäjät ovat rekisteröineet tarvittavat todennusmenetelmät **etukäteen**.

- **Käytä Entra-portaalin "Identity Protection" -osiota ja ota käyttöön:**
  - **Multi-Factor Authentication registration policy**

Tämä politiikka ohjaa valitut käyttäjät rekisteröimään MFA-tavat (kuten Microsoft Authenticatorin) heti, kun he täyttävät ehdot.

> Vaikka todennustapojen salliminen tehdään Authentication Methods -politiikalla ja rekisteröintisivun käyttöä voi rajoittaa Conditional Access -politiikalla (`Register security information`), vain Identity Protectionin rekisteröintikäytäntö takaa, että riskitilanteissa tarvittava MFA on ennakkoon käytettävissä.

**Tavoite:** Varmistaa, että jokaisella käyttäjällä on vähintään yksi voimassa oleva todennusmenetelmä kirjattuna ennen kuin automaattisia riskinhallintatoimia otetaan käyttöön.

#### Suositellut asetukset:

- **Kohdista käytäntö koko organisaatiolle** (tai ainakin riskiherkille ryhmille)
- Varmista, että seuraavat turvalliset todennusmenetelmät ovat sallittuja ja että vähintään yksi niistä on rekisteröity:
  - **Microsoft Authenticator** -sovellus (ilmoitukset tai vahvistuskoodi)
  - **FIDO2-avain** tai muu vahva todennusväline (esimerkiksi biometrinen tunnistus, jos tuettu)
  - **Windows Hello for Business** (organisaation tuesta riippuen)

- Aseta käytäntö vaatimaan rekisteröintiä heti, kun käyttäjä täyttää ehdot (esimerkiksi ensikirjautumisen yhteydessä tai roolin saannin jälkeen)

> **Huom:** Vältä vähemmän turvallisia menetelmiä kuten tekstiviestit (SMS) ja puhelut. Nämä ovat alttiita sieppauksille ja SIM-hyökkäyksille, eikä Microsoft suosittele niiden käyttöä riskiperusteisessa autentikoinnissa.

**Tavoite:** Varmistaa, että jokaisella käyttäjällä on vähintään yksi **vahva ja suositeltu todennusmenetelmä** kirjattuna ennen kuin riskiperusteiset automaattitoimet (kuten "Require MFA") otetaan käyttöön.

### Vaihe 4 – Seuranta ja jatkuva ylläpito

Riskipohjainen suojaus ei ole "ota käyttöön ja unohda" -ratkaisu. Se vaatii jatkuvaa seurantaa, vianselvitystä ja reagointia. Alla on neljä keskeistä toimenpidettä ja niiden tarkempi kuvaus:

#### Aktivoi Email notifications

> Ilmoitukset varoittavat, kun riskikäyttäjä havaitaan tai riskitilanteita syntyy.

- **Missä:**  
  Entra > Identity Protection > **Notifications**
- **Aseta ilmoitusten vastaanottajat:**  
  - Valitse yksi tai useampi **Global Administrator** tai **Security Administrator** -rooliin kuuluva henkilö.
- **Ilmoitustyypit:**
  - Risky users detected
  - Risky sign-ins detected
- Ilmoitukset lähetetään sähköpostitse Microsoftilta, esim. `alerts@microsoft.com`

> Tämä on erityisen tärkeä asetus SOC-tiimeille, jotka haluavat reagoida nopeasti.

#### Analysoi käyttöä ja trendejä Workbooks-raporteilla

Microsoft tarjoaa valmiita workbookeja riskien ja vaikutusten seurantaan.

##### a) Identity Protection Workbook
- **Missä:**  
  Entra > Identity Protection > **Dashboard > Open workbook**
- **Sisältö:**
  - Risky sign-ins: määrät, lähteet, käyttäjät, maat
  - Risky users: trendit ja yksittäiset käyttäjät
  - Risk detections: tyypit ja kehitys

##### b) Impact Analysis Workbook
- **Missä:**  
  Entra > Conditional Access > Insights & reporting > Workbooks > Impact Analysis
- **Sisältö:**
  - Näyttää, kuinka moni käyttäjä olisi joutunut käytännön piiriin ennen sen varsinaista aktivointia
  - Erittäin hyödyllinen, kun käytetään **report-only** -tilaa ennen politiikan käyttöönottoa

> Workbooks-raportit edellyttävät lokien ohjaamista Log Analytics -työn tilille (Azure Monitor).

---

#### Integroi riskisignaalit Microsoft Defenderiin ja SIEM/SOC-järjestelmiin

> Riskisignaalit eivät jää Identity Protectionin sisälle – niitä voidaan hyödyntää myös muualla.

##### a) Microsoft Defender for Cloud Apps (MCAS)
- Tukee seuraavia Entra Identity Protection -signaaleja:
  - **Risky sign-ins**
  - **Risky users**
- Esimerkkejä tuetuista signaaleista:
  - *Activity from anonymous IP address*
  - *Impossible travel*
  - *Mass access to sensitive files*
  - *New country*
- **Missä:**  
  Microsoft Defender portal > Cloud Apps > Policies tai Activity Log > Signals

##### b) Microsoft Sentinel / muu SIEM
- Ohjaa Entra ID -signaalit Log Analytics -tilille.
- Luo hälytyksiä ja sääntöjä SIEM-työkalussa seuraavista:
  - Risky sign-ins
  - Risky users
  - Risk detections
- **Konfigurointi:**  
  Entra / Azure AD > Diagnostic settings

#### Simuloi uhkia – simulate threats

> Testaus on tärkeää, jotta varmistetaan, että riskikäytännöt todella toimivat odotetulla tavalla.

- **Missä:**  
  Entra > Identity Protection > Risky users > Valitse käyttäjä > **Mark user as compromised**
- **Vaihtoehtoisesti:**  
  Käytä Microsoft Graph APIa simuloidun riskin luomiseen (edistyneempi vaihtoehto)
- Simuloinnin avulla voit:
  - Tarkistaa, laukeaako haluttu riskipolitiikka
  - Varmistaa, että hälytykset tai korjaavat toimet (kuten Require MFA tai block access) toimivat
  - Testata SOC-tiimin vasteen

> Tee testauksesta osa käyttöönottoprosessia ja toista säännöllisesti.

---

## Käyttöönotto: Ilmoitukset (Notifications)

Entra ID Protection voi lähettää kaksi erilaista automaattista ilmoitusta sähköpostitse, jotka auttavat sinua reagoimaan tunnistettuihin riskeihin ja pysymään ajan tasalla riskeistä:

#### **Users at risk detected** – ilmoitus käyttäjän riskitasoista

- Lähetetään heti, kun käyttäjän riskitaso nousee asetetun tason (esim. Medium tai High) yläpuolelle.
- Sisältää linkin *Users flagged for risk* -raporttiin, mikä mahdollistaa välittömän tutkimisen.
- Jos käyttäjän riskitaso pysyy samana, mutta riskisarja päivitetään (esim. uusi riskitapahtuma), uusi ilmoitus lähetetään vain, jos riskin ajankohta on ilmoitusta edeltävän viimeisen ilmoituksen jälkeen.
- Estää sähköpostitulvan: jos useiden käyttäjien riskit nousevat samaan aikaan (huom. 5 sekunnin säteellä), kaikki sisällytetään yhteen sähköpostiin.

> **Huom:**  
> Ilmoitusviestien vastaanottajia ei aina tarvitse määrittää manuaalisesti. Käyttäjät, joilla on **Global Administrator**, **Security Administrator** tai **Security Reader** -rooli, lisätään automaattisesti vastaanottajalistalle, jos heillä on määritettynä kelvollinen **Email**- tai **Alternate email** -osoite.
> 
> Ilmoituksia yritetään lähettää korkeintaan **20 ensimmäiselle käyttäjälle per rooli**. 
>
> Jos käyttäjä hyödyntää **Privileged Identity Managementia (PIM)** ja nostaa käyttöoikeuksiaan tarpeen mukaan (on-demand), hän saa ilmoituksia **vain silloin**, kun roolin nosto (elevation) on aktiivinen ilmoituksen lähetyshetkellä.
> 
> Varmista, että ilmoituksen saavan ylläpitäjän sähköpostiosoite täyttää ilmoitussivun **"custom email validation"** -ehdot – muuten viestiä ei välttämättä voida toimittaa.

##### Konfiguroi:
- **Microsoft Entra admin center** ‑portaalissa: **ID Protection > Dashboard > Users at risk detected alerts**.
- Aseta tavoiteltava riskitaso (esim. Medium tai High), jolloin ilmoitus laukeaa.
- Määritä vastaanottajat – oletuksena ilmoitus menee aktiivisille Global Administrator, Security Administrator ja Security Reader -roolin haltijoille, joilla on kelvollinen sähköpostiosoite.


#### **Weekly digest** – viikkoyhteenveto riskeistä

- Lähetetään kerran viikossa tiivistelmä uusista riskeistä, jotka on havaittu kuluneella ajanjaksolla.
- Yhteenveto sisältää esimerkiksi:
  - Uudet riskikäyttäjät (New risky users)
  - Uudet riskikirjautumiset (New risky sign‑ins)
  - Linkit eri raportteihin ID Protectionissa

##### Konfiguroi:
- Asetukset löytyvät **Microsoft Entra admin center** -portaalista kohdasta **ID Protection > Dashboard > Weekly digest**.
- Voit valita, haluatko ottaa viikkoyhteenvedon käyttöön tai pois ja määrittää vastaanottajat.

####  Yhteenveto

| Ilmoitustyyppi | Milloin lähetetään | Sisältö | Mistä hallitaan |
|----------------|---------------------|---------|-----------------|
| **Users at risk detected** | Kun riskitaso nousee asetetun tason yläpuolelle | Linkki *Users flagged for risk* -raporttiin | **ID Protection > Dashboard > Users at risk detected alerts** |
| **Weekly digest** | Viikoittain | Yhteenveto uusista riskeistä ja raporttilinkit | **ID Protection > Dashboard > Weekly digest** |

> **Muista:** Vain käyttäjät, joilla on kelvollinen sähköpostiosoite ja sopiva rooli (Global Admin, Security Admin, Security Reader), saavat nämä ilmoitukset. Käytännön hallinta kannattaa testata ensin testiryhmällä, jotta tiedät, että viestit tavoittavat oikeat henkilöt.

## Käyttöönotto: Riskipolitiikat Conditional Accessilla

###  Esivalmistelut
- Tarvitaan **Entra ID P2** tai **Entra Suite** -lisenssi.
- Käyttäjällä tulee olla vähintään **Conditional Access Administrator** -rooli.

###  Hyväksyttävien riskitasojen valinta
- **High**: vähiten häiriöitä käyttäjille, mutta sis. vain korkean riskin tapaukset.  
- **Medium** tai **Low**: herkempi reagointi, mutta enemmän häiriöitä.  
  Voit myös käyttää luotettuja verkkoja vähentämään virheitä.

###  Suositus: sallia itsenäinen riskin korjaus käyttäjille
- **User risk policy**:
  - Riskin taso: **High**
  - Toimenpide: sallitaan käyttäjän turvallinen salasanan vaihto (vaatii MFA ja password writeback)
- **Sign‑in risk policy**:
  - Riskin taso: **Medium** tai **High**
  - Toimenpide: MFA-vaatimus (yksi rekisteröity menetelmä riittää)

###  Riskipolitiikkojen määrittäminen
1. Kirjaudu **Entra admin centeriin** Conditional Access -roolilla.
2. Luo uusi policy ja määritä:
   - **Users**: Kaikki, mutta poislukien break-glass-tilit
   - **Cloud apps**: Kaikki sovellukset
   - **Condition**: User risk / Sign‑in risk -asetukset ja riskitasot
3. **Access controls**:
   - User risk: “Require password change” & MFA
   - Sign‑in: “Require MFA”
4. **Session**: Aseta “Sign‑in frequency – Every time”
5. Ota käyttöön ensin **Report-only**, arvioi vaikutus ja vasta sitten siirry “On”

###  Viimeiset vihjeet
- Poista perinteiset riskipolitiikat, kun Conditional Access -malli toimii
- Älä lukitse kriittisiä tilejä – poissulje emergency-access- tai käyttöliittymätunnukset politiikoista

> **Yhteenveto**  
> - **User risk policy**: high → salasanan vaihto  
> - **Sign-in risk policy**: medium/high → MFA  
> - Testaa ensin **report-only**‑tilassa  
> - Älä sulje tilanne pois korjauskanavista  
> - Vältä tukihenkilöiden lukintahäiriöitä

### Parhaat käytännöt riskipolitiikkojen käyttöönottoon

Kun otat käyttöön riskipohjaisia käyttöehdotuksia Entra ID Protectionilla, on tärkeää varmistaa, että politiikat eivät estä pääsyä järjestelmiin tai aiheuta tarpeetonta häiriötä loppukäyttäjille.

#### Aloita “Report-only”-tilassa

- Käytä **Report-only**-asetusta kaikille uusille Conditional Access -politiikoille.
- Arvioi vaikutukset ennen kuin otat ne käyttöön oikeasti (ON).
- Voit analysoida vaikutuksia **Sign-in logs** -osiosta (filter: Report-only).
- Käytä Entra-portaalin “What If” -työkalua testataksesi vaikutuksia ennen aktivointia.

#### Määritä selkeät ja perustellut riskitasot

| Riskitaso | Milloin käyttää | Huomioitavaa |
|-----------|------------------|--------------|
| **High**  | Hyvin luottamuksellisiin resursseihin tai korkean profiilin käyttäjiin | Suositellaan “Require password change” |
| **Medium** | Useimmille tavallisille käyttäjille | Usein yhdistetty “Require MFA” -ehtoon |
| **Low**   | Harvoin käytössä suoraan, voi aiheuttaa paljon vääriä positiivisia | Käytä harkiten tai yhdistä muihin ehtoihin |

#### Poissulje “Break-glass”- ja palvelutilit

- Luo erilliset “emergency access” -tilit, joita ei koske riskipolitiikat.
- Lisää nämä tilit poikkeuksena Conditional Access -politiikoissa.
- Nämä tilit tulisi:
  - Olla suojattuja vahvalla salasanalla ja MFA:lla
  - Dokumentoida ja testata säännöllisesti
  - Olla käytössä vain hätätilanteissa

#### Varmista MFA ja Password Writeback -kyvykkyys

- Käyttäjän itsenäinen riskin korjaus (esim. salasanan vaihto) edellyttää:
  - Azure AD Password Writeback (jos käytössä hybriditunnistus)
  - Vähintään yksi rekisteröity MFA-menetelmä käyttäjällä
- Ilman näitä vaatimuksia “require password change” ei onnistu, ja käyttäjä voi jäädä lukkoon.

#### Käytä yhdistelmiä muiden ehtojen kanssa

- Rajoita politiikkojen vaikutusaluetta esim. **location**, **device platform** tai **sign-in frequency** -ehdoilla.
- Näin voit ottaa politiikan käyttöön vaiheittain ja välttää odottamattomia lukituksia.

#### Testaa käyttäjäprofiileilla

- Luo testikäyttäjäryhmiä eri rooleille: tavallinen työntekijä, ylläpitäjä, ulkoinen käyttäjä.
- Aja politiikat alkuun näille ryhmille ja tarkastele vaikutuksia.
- Käytä sign-in logeja ja policy impact -analytiikkaa (Entra portal / Sign-in logs).

> **Yhteenveto**
> - Käynnistä “Report-only” -tilassa ja analysoi vaikutus ennen aktivointia
> - Käytä “High” risk -tasoa varoen, mutta määrätietoisesti
> - Varmista emergency access -tilien toiminta ja suojaus
> - Älä oleta että MFA on automaattisesti rekisteröity – tarkista!
> - Hyödynnä sign-in-ehdot (location, platform, device) politiikkojen rajaukseen

## Passwordless ja hybridiratkaisut – mitä tulee huomioida riskipolitiikkojen kanssa

Kun käytössä on modernit tunnistusmenetelmät tai hybridimalli (esim. AD synkronointi), on tärkeää ymmärtää, miten nämä vaikuttavat riskien havaitsemiseen ja politiikkojen käyttäytymiseen.

### Passwordless ei ole "immuuni" riskeille

Vaikka käyttäjä kirjautuu ilman salasanaa (esim. Windows Hello, FIDO2, Authenticator-sovelluksella), kirjautumista voidaan silti pitää riskipitoisena:

- Microsoft Entra ID Protection arvioi **kontekstuaalisen riskin**, ei vain kirjautumismenetelmää.
- Esimerkiksi:
  - **Impossible travel**
  - **Anonymous IP**
  - **Sign-in from new country**
  - voivat silti laukaista “Medium” tai “High” riskin kirjautumiselle, vaikka kirjautuminen olisi "vahvasti todennettu".

**Suositus:** älä ohita riskipolitiikkoja vain siksi, että käytössä on passwordless – käytä samaa politiikkamallia kuin muillekin.

#### Hybridikäytössä varmista writeback ja riskidatan näkyvyys

Hybridikäyttö tarkoittaa, että käyttäjätiedot synkronoidaan paikallisesta Active Directorystä Entra ID:hen. Tämä tuo mukanaan erityispiirteitä:

- **Salasanan vaihdon writeback** vaatii seuraavat edellytykset:
  - Azure AD Connect asennettuna ja writeback asetettu päälle
  - Oikeudet päivittää salasanat paikalliseen AD:hen
- **User risk remediation** (salasanan vaihto) ei onnistu ilman writebackia – käyttäjä jää jumiin.

**Vinkki:** Tarkista `Password reset > On-premises integration` asetukset Azure-portaalissa

#### MFA-tilanne hybridissä ja passwordless-ympäristössä

- Riskipolitiikat edellyttävät, että käyttäjällä on rekisteröity vähintään yksi **moderni MFA-menetelmä**.
- Vanhat “perinteiset MFA”-rekisteröinnit eivät aina täytä vaatimuksia.
- FIDO2-avaimet ja Authenticator-sovellus toimivat hyvin, mutta varmista että ne ovat **Entra ID:n tunnistamia**.

**Tarkistuspaikka:**  
Entra admin center → Users → MFA → Authentication methods

#### Riskien analysointi eri kirjautumistavoilla

Riskien lähteet pysyvät samoina, mutta käyttäytyminen voi vaihdella:

| Kirjautumistapa     | Riskien arviointi       | Huomioitavaa |
|----------------------|--------------------------|----------------|
| Salasana + MFA       | Normaali riskitason laskenta | Riski lasketaan koko sessiosta |
| Passwordless (WHfB, FIDO2) | Riskit arvioidaan IP-, sijainti-, matka- ja anomaliatiedon perusteella | Ei "ohita" politiikkoja |
| SSO tai federointi (esim. ADFS) | Riskin laskenta rajoittunut | Riskitieto voi jäädä vajaaksi ilman täyttä Entra-integraatiota |

#### Yhteenveto

- Passwordless on turvallinen, mutta ei riskitön kirjautumismuoto
- Hybridikäytössä on erityisen tärkeää varmistaa password writeback ja MFA-toteutus
- Älä oleta, että vahva kirjautuminen korvaa riskipohjaisen päätöksenteon
- Testaa politiikat kaikilla käytetyillä kirjautumistavoilla, erityisesti hybridiympäristöissä

---

## Siirtyminen vanhoista riskipolitiikoista uusiin (Conditional Access -pohjaisiin)

Microsoft on ilmoittanut, että perinteiset Identity Protection -riskipolitiikat (User risk policy, Sign-in risk policy, MFA registration policy) poistuvat käytöstä **1.10.2026** alkaen. Ne korvataan täysin **Conditional Access** -pohjaisilla politiikoilla, jotka tarjoavat joustavamman ja keskitetymmän hallinnan.

### Mistä tunnistaa vanhat politiikat?

- Entra-portaalissa kohdassa **Identity Protection > Policies**
- Jos politiikat on konfiguroitu suoraan Identity Protectionin kautta (eikä Conditional Accessin kautta), ne ovat *vanhoja*.
- Nämä politiikat näkyvät yleensä erillisenä "MFA registration policy", "User risk policy" tai "Sign-in risk policy"

### Mikä muuttuu?

| Vanha politiikka         | Korvaava ratkaisu |
|--------------------------|-------------------|
| User risk policy         | Conditional Access -policy, jossa `User risk level` = medium/high + toimenpide |
| Sign-in risk policy      | Conditional Access -policy, jossa `Sign-in risk` = medium/high + toimenpide |
| MFA registration policy  | Conditional Access -policy, jossa `Grant controls` sisältää `Require registration of security info` |

---

### Suositeltu siirtymäsuunnitelma

1. **Tunnista käytössä olevat vanhat politiikat**
   - Kirjaa niiden konfiguraatio ja kohderyhmät ylös
   - Tunnista missä tilanteissa ne aktivoituvat ja mitä ne tekevät

2. **Suunnittele vastaavat Conditional Access -politiikat**
   - Luo CA-politiikat, jotka vastaavat vanhojen logiikkaa
   - Lisää tarvittaessa rajauksia (esim. location, device platform)
   - Aseta politiikat aluksi tilaan **Report-only**

3. **Testaa vaikutukset**
   - Käytä Entra-portaalin **Sign-in logs**- ja **What If** -työkalua
   - Varmista, että politiikka käyttäytyy odotetusti

4. **Ota käyttöön vaiheittain**
   - Aktivoi uudet CA-politiikat ryhmä kerrallaan
   - Pidä vanhat politiikat vielä rinnalla siirtymävaiheessa

5. **Poista vanhat politiikat käytöstä**
   - Kun olet varmistanut, että kaikki uudet CA-politiikat toimivat suunnitellusti
   - Varmista, että käyttäjillä on rekisteröity MFA ja salasanan vaihto toimii

---

### Yhteenveto

- Microsoftin vanhat riskipolitiikat poistuvat lokakuussa 2026 – siirtymä on **pakollinen**
- Luo korvaavat Conditional Access -politiikat, jotka hyödyntävät `Sign-in risk` ja `User risk` -ehtoja
- Varmista MFA- ja password writeback -toiminnot ennen "require password change" -politiikkojen käyttöä
- Käytä **Report-only**-tilaa ja testaa ennen aktivointia

> Siirtyminen on mahdollisuus päivittää suojaus nykyaikaiselle tasolle – tee se hallitusti ja dokumentoidusti.

---



