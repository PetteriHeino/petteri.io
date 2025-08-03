---
title: VS Code ja GitHub käyttöönotto ja työskentely repositoryissä
description: >-
 Infrastucture As Code for dummies sarjan ensimmäinen kirjoitus. Käydään vähän komentoja läpi, että päästään alkuun VS Coden ja GitHubin kanssa.
authors: Petteri
date: 2025-03-01 12:00:00 +0200
categories: [Työvälineet]
tags: [VS Code, GitHub]
pin: false
media_subpath: '/assets/media/2025-03-01-vs_code_ja_github_integraatio'
---

# Johdanto

Opiskelen MS Azuren hallinnointia sertifiointitavoitteisesti. Aloittaessani opiskelemaan Azure Administrator Associate testiin (AZ-104) tuumasin, että koska Windows pöytäkoneeni hajosi, niin asennan Macbook läppäriin VS Code editorin ajatuksena, että koitan alusta lähtien opetella ylläpitämään Azurea mahdollisimman vähän graafisessa käyttöliittymässä. Sen takia tässä kirjoituksessa on asiaa VS Coden ja GitHubin käyttöönotosta MacOsille. Samat asiat koskevat toki Windowsia, mutta ne voivat olla vähän erilaisia. En tässä kirjoituksessa ota kantaa Windows asennukseen. Kirjoituksen GitHub komennot ovat kuitenkin samat, ajaa niitä Macissä tai Windowsissa.

Kun pääsin AZ-104 opiskelussa ARM templateihin, niin ymmärsin, että vaistoni oli oikeassa siinä, että VS Code kannattaa asentaa omalla läppärille. Jos olet koskaan asentanut virtuaalikoneita Azuren hallintaportaalissa, niin tiedät kuinka hidasta se voi olla. ARM templateilla voidaan automatisoida niin virtuaalikoneiden kuin muidenkin Azure resurssien asennuksia. Mittavampienkin asennusten tekeminen tapahtuu nopeasti kun Azure Resource Manager asentaa templateissa määritettyjä asennuksia ja konfiguraatioita saman aikaisesti, riippuvaisuus tilanteita lukuunottamatta.

Opiskelussakin testiympäristöjen automatisoidulla asennuksella on merkitystä. Koska haluat opiskella Azurea mahdollisimman vähillä kuluilla, niin et halua, että opiskelusession jälkeen Azureen jää mitään resursseja, mitkä tuottaa sinulle kuluja. Sen takia on hyvä automatisoida demoympäristöjen asennukset, jotta pääset aloittamaan hands on testaamisen mahdollisemman nopeasti testiympäristön asentuessa muutamassa hetkessä. Mikä parasta, kun tähtäät siihen, että voit asentaa demoympäristöt automatisoidusti, niin olet samalla jo mahdollisesti matkalla Infrastructure As Code (IaC) osaajaksi.

Internetistä löytyy tämän kirjoituksen asiat todennäköisesti paremmin dokumentoituna. En ole tätä kirjoittaessa vielä mikään VS Code ja GitHun taituri. Se onkin tämän kirjoituksen pointti. Sen sijaan, että dokumentoin itselleni asioita OneNoteen, niin dokumentoin niitä web sivuksi internetiin. Dokumentaatiossa on lähinnä sitä asiaa jäsenneltynä, mitä olen joutunut tähän mennessä VS Coden ja GitHubin kanssa tekemään. Eli peruskomentoja ylläpitoon.

Lähdin VS Coden ja GitHubin käytössä liikkeelle lukematta ensinkään dokumentaatioita ja kyselin jatkuvasti tekoälyltä että miten tehdään sitä ja tätä? 

Päädyin tekemään web sivuja DJ persoonalleni, koska halusin jonkun hyvän harjoitustyön millä saan tuntumaa VS Coden ja GitHubin käyttöön. Se oli melkoisen uuvuttava prosessi kun lähti liikkeelle siitä, että minulla ei ollut kummankaan käyttökokemusta ja osaamista entuudestaan. En varmasti olisi saanut tehtyä mitään ilman tekoälyn avustusta. Tällä haluan kannustaa asiantuntijoita koodieditorin pariin, koska VS Coden mukana tulee copilot edit ja copilot chat integroituna ja sen käyttö on ilmaista. Joskin maksullinen lisenssi löytyy myös mikä antaa Copilotin käyttöön rajattomasti. Ilmaislisenssissä on rajoitus kuinka paljon Copilotia voi komentaa kuukautta kohden.

Tässä vielä mainostuksena Copilotin tuoman lisän tekemiseen:  
Copilotilta voit
- kysyä tekoälyltä apua kun et ymmärrä miten jokin asia tehdään VScodessa, koodissa, terminal komennoissa jne..
- pyytää kertomaan mitä jokin valmis koodi tekee
- pyytää luomaan koodia antamillasi spekseillä
- pyytää debugausta virheellisesti toimivaan koodiin

Nolla taidoilla pystyin luomaan web sivuston ja tekemään siihen liittyvää ongelmanselvitystä. Jos copilotia ei olisi ollut, niin web sivusto ei olisi noussut henkiin. Toki siinäkin meni aikaa. Hajoitin web sivut vain pahemmin ennenkuin ne lopulta oli täysin toimivat, koska jouduin uskomaan kaiken mitä tekoäly ehdottaa. Tekoäly on mahtava opettaja ja apuri, mutta sen käyttö edellyttää kriittysyyttä ja omien aivojen käyttöä. 

Tästä kokemuksesta viisastuneena voin kuitenkin sanoa, että pääasiassa tekoäly toimii tänä päivänä käsittämättömän hyvin, mutta jotta saat toimivia ratkaisuja, niin sinun pitää pystyä antamaan mahdollisimman hyvät taustatiedot tekoälylle. Asiantuntijat aina voivottelevat puutteellisista lähtötiedoista kun jotakin muutosprojektia käynnistetään, tekoälyn kanssa voi testata saman ilmiön kun itse annat puutteelliset lähtötiedot, niin kuinka hyvää jälkeä sieltä tekoälyltä sitten saat.

Web sivuja luodessa alkoi olemaan selvä, että minun pitää jossain kohtaa dokumentoida ihan näitä perus terminal komentoja, jotta ymmärrän niihin liittyviä vivahteita eikä kaikkea tarvi kysyä tekoälyltä tai jos kysyy niin pystyy sanomaan vastaan jos jokin ei vaikuta oikealta ratkaisulta. 

Aloitin dokumentoinnin omaan OneNoteen, mutta pian siinä tuli ajatus, että tämä dokumentaatio voisi olla ensimmäinen blogikirjoitus Infrastructure As Code for dummies sarjalleni. Tässä lähinnä dokumentoitu hieman VS Code ja GitHub käyttöönottoa ja perus zsh komentoja VS Coden terminalissa. Saatan päivittää tätä dokumentin sisältöä jatkossa kun työskentelen VS Coden ja GitHubin parissa ja kokemusta tulee lisää.

Paremmat dokumentaatiot VS Codesta ja Githubista löytyvät täältä:

- <a href="https://code.visualstudio.com/docs" target="_blank" rel="noopener noreferrer">VS Code</a>
- <a href="https://docs.gitlab.com/user/get_started/" target="_blank" rel="noopener noreferrer">GitHub</a>

Mikäli olet kiinnostunut web sivujen luomisesta, joihin nämä sivuni pohjautuvat:
- <a href="https://chirpy.cotes.page/posts/getting-started/" target="_blank" rel="noopener noreferrer">Chirpy</a>

## VS Code GitHub -integraation asennus ja käyttöönotto

### 1. Luo GitHub tunnus

Käy luomassa GitHub tili osoitteessa https://github.com/

### 2. Asenna VS Code

lataa asennuspaketti ja asenna editori https://code.visualstudio.com/Download

### 3. Varmista, että Git on asennettuna

Avaa VS Code

Todennäköisesti VS Code ei ole asennettu, jos vasta asensit editorin, mutta jos on jokin muu tilanna niin voit tarkastaa onko GitHun jo asenettu komennolla:

```zsh 
git --version
```
Jos tämä ei palauta versiota, asenna Git Macille:

```zsh
brew install git
```

Jatkossa pidä GitHub päivitettynä komennolla

```zsh
brew update && brew upgrade git
```

Seuraavaksi asennetaan GitHub Pull Requests and Issues laajennus. Tämä löytyy Extensions: Marketplacesta (klikkaa vasemmassa laidassa olevaa ikonio missä on neljä neliötä joista yksi on erillään). Kirjoita hakukenttään GitHub, paikallista GitHub Pull Requests ja klikkaa install.

### 4. Kirjaudu GitHubiin VS Codessa ja autentikointi

Editorin vasemmassa alakulmassa olevasta identiteetti kuvakkeesta kirjaudutaan GitHub Pull Requests... ja GitHub Copilotiin.

#### SSH-autentikointi avaimen luominen ####

Aja seuraava komento shellissä:

```zsh
ssh-keygen -t ed25519 -C "sähköpostisi@domain.com"
```
Korvaa sähköpostiosoitte GitHub tunnukseesi määritellyllä sähköpostiosoitteella.  
Kun komentoa suoritetaan, niin voit painaa olla vastaamatta ja painaa enteriä kummassakin kysymyksessä jos olet tyytyväinen oletuspolkuun koneellasi ja et määritä salasanaa avaimelle.

Aktivoidaan SSH-agentti:

```zsh
eval "$(ssh-agent -s)"
```

Lisää uusi SSH-avain agenttiin:
```zsh
ssh-add ~/.ssh/id_ed25519
```

Kopioi komennon tuottama koko hässäkkä GitHubiin oman tunnuksen asetuksiin SSH and GPG keys välilehdelle SSH keysiin.

Testaa yhteys GitHubiin
```zsh
ssh -T git@github.com
```
Mikäli saat tämän henkisen vastauksen, niin yhteyden pitäisi olla kunnossa "You've successfully authenticated, but GitHub does not provide shell access." 


### 5. Luodaan GitHub repository ja paikallinen kansio, jotka liitetään toisiinsa

Luo kansio omalle tietokoneelle, mitä haluat käyttää paikallisena kopiona. Avaa kansio VS Codessa File valikosta ja Open Folder...

Mene selaimella GitHubiin. Sivun ylälaidassa on valikkorivi ja oikeassa laidassa näkyy plust ja sen vieressä kolmio/nuoli alaspäin. Klikkaa nuolta ja valitse New repository. Anna repositorylle nimi ja valitse onko repository public vai private... esim. tämä blogi on public repositoryssa. Klikkaa Create repository.

Liitetään paikallinen kansio GitHub repositoryyn kommennolla:

```sh
git remote add origin <repo-osoite>
```
Voit tarkistaa, että liitos onnistui:

```sh
git remote -v
```

## Työskentely repositoryn kanssa

Tässä dokumentaation kappaleessa lähinnä keskeistä ovat komennot brancheille, joita oman tämän hetkisen ymmärryksen mukaan voisi periaatteessa sanoa myös juurihakemistoiksi. Lisäksi versiohallinnan komennot git add, git commit ja datan siirto push komennolla. 

Nämä ovat keskeisimmät asiat paikallisen ja github repositoryn ylläpidon kannalta. Github dokumentaatiosta voi lukea syvemmin näistä.

### 1. Branchit (haarat)

Repositoryssa voi olla monta eri haaraa mihin dataa tallennetaan.

Haaroja on sekä paikallisessa repossa, että GitHub repossa. Tässä komentoja joilla voi tutkia ja operoida paikallisia -ja etähaaroja:


| Komento      | Kuvaus    |
| ---  |  ----------- |
| git branch| Listaa paikalliset haarat |
| git branch -r | Listaa etähaarat (GitHubissa) |
| git branch -a | Listaa kaikki haarat |
| git status    | Näyttää nykyisen haaran ja muutokset  |
| git rev-parse --abbrev-ref HEAD |
| git branch -v | Näyttää haarojen viimeisimmät commitit |
| git checkout -b uusibranch | luo uusibranch nimisen lokaalin branchin ja vaihtaa uusibranch branchin aktiiviseksi |
| git push -u origin uusibranch | kopioi uusibranch branchin GitHubiin |
| git checkout develop | vaihtaa develop nimiseen brachiin |
| git fetch | tuo etäbranchit siirtymättä kopioutuun branchiin |

Kaksi branchia voidaan yhdistää esim "uusibranch" -> "main" komennoilla
```zsh
git checkout main
git merge uusibranch
git push origin main
```

Opiskelun alussa harvemmin tarvitsee branchien kanssa operoida, mutta jotta dataa ei tule siirtäneeksi minne sattuu niin on hyvä tuntea komennot, joilla pystyy tarkistamaan lokaalit ja etäbranchit tarvittaessa. Esimerkiksi git branch -a komento tuottaa tällä hetkellä omassa VS Codessa seuraavat tiedot
"* gh-pages
  master
  remotes/origin/gh-pages"

Tämä tarkoittaa sitä, että minulla on kaksi paikallista branchia gh-pages ja master, joista gh-pages on aktiivinen. Näiden lisäksi yksi etähaara remotes/origin/gh-pages. Etähaaran tunnistaa polusta remotes/origin/

### 2. Peruskomennot datan työntämiseen

Joudut melkein aina suorittamaan samat kolme komentoa, kun sinulla on versiohallinta käytössä ja siirrät dataa lokaalista repositorysta GitHub repositoryyn.

| Komento      | Kuvaus    |
| ---  |  ----------- |
| git add . | Lisää kaikki muutokset nykyisestä hakemistosta ja alihakemistoista |
| git commit -m "feat: lisätty uusi ominaisuus" | Tallentaa muutokset paikalliseen versiohistoriaan ja antaa niille kuvaavan viestin. |
| git push origin main | Lähettää muutokset main haaraan. |

Kun kaikki kolme komentoa on ajettu virheettömästi, niin data siirtyy GitHub repositoryyn kaikkien muutosten osalta.  
Kaikilla kolmella komennolla on omat kahvansa, niin että ne voi komentaa tekemään samaa asia hieman erilailla.

##### git add
Merkitys: lisää kaikki muuttuneet tiedostot staging arealle eli valmistelee ne commitointia varten
vaihtoehdot:

- ***git add.*** -> lisää kaikki muutokset nykyisestä hakemistosta ja alihakemistoista
- ***git add tiedosto*** -> lisää vain tietyn tiedoston staging arealle
- ***git add -A*** -> lisää kaikki muutokset, mukaan lukien poistetut tiedostot

#### git commit
Merkitys: Tallentaa muutokset paikalliseen versiohistoriaan.

git commit komennolla on kahvoja, joista tässä muutama yleisimmin käytetty.


**-m "message"**    
Merkitys: tehtyihin toimenpiteisiin jää viesti, että mitä muutoksessa on tehty

-m kahvan viesti osassa pitää olla jokin alla luetelluista merkinnöistä mukana tai shell antaa virheen komentoa ajettaessa

**feat:** → Uusi ominaisuus  
**fix:** → Bugikorjaus  
**chore:** → Ylläpitotoimenpide (esim. riippuvuuksien päivittäminen)  
**refactor:** → Koodin uudelleenjärjestely ilman toiminnallisia muutoksia  
**docs:** → Dokumentaation päivitys  
**style:** → Muotoilu- tai tyylimuutokset (ei koodin toimintaan vaikuttavia)  
**test:** → Testien lisäys tai päivitys  
**perf:** → Suorituskyvyn parannukset  
**ci:** → Continous Integration -konfiguraatioiden muokkaus

Esimerkki
```zsh
git commit -m "fix: korjattu virheellinen muuttujan nimi"
```
**--amend**    
Merkitys: muokkaa edellistä committia (korvaa uudella).  

Esimerkki 
```zsh
git commit --amend -m "fix: korjattu tiedostoja hakemistossa x"  
```
Esimerkin komento ajettu, koska edellisen commitin jälkeen tehty samaan muutokseen liittyviä lisämuutoksia ja haluttu muuuttaa commit viestiä  

Jos halutaan korvata edellinen commit uudella, mutta ei haluta muuttaa viestiä, niin se onnistuu tällä komennolla:  
```zsh
git commit --amend --no-edit
```

```zsh
git commit -a --amend --no-edit
```

**-a**    
Commitoi kaikki muuttuneet ja trackatut tiedostot, <mark>ei lisää uusia tiedostoja</mark>.   
```zsh
git commit -a -m "fix: korjattu bugi"  
```
Käyttötapa: commitoi ilman git add komentoa  

#### git push
Pushia tehdessä pelkkä "git push" komento riittää, mikäli lokaali branch seuraa etäbranchia.

Tarkista seuraako lokaali branch etäbranchia komennolla
```zsh
git branch -vv
```

Kun teet ensimmäisen push komennon etäbranchiin niin aja komento
```zsh
git push -u origin branchnimi
```
tämän jälkeen branchit ovat linkitetty toisiinsa.