---
title: Azure opiskelun aloittaminen ja sertifiointien merkitys työmarkkinoilla
description: >-
 Tämän on tarkoitus olla blogi-sarjan ei-tekninen aloitus ja jatkan sarjaa blogi postauksina harjoituksista Azure opiskelujen edetessä. Sarjan focus on tarkoitus olla pääasiassa Infrastructure As Codessa.
authors: Petteri
date: 2025-02-22 21:50:00 +0200
categories: [Sertifiointi, Opiskelu]
tags: [Azure, Microsoft]
pin: false
media_subpath: '/assets/media/2025-02-27-azure_opiskelu'
---

# Johdanto

Pilvipalveluiden merkitys IT-alalla on kasvanut valtavasti viime vuosina, ja yhä useampi organisaatio siirtyy joko kokonaan tai osittain pilviympäristöihin. Microsoft Azure on yksi johtavista pilvipalvelualustoista, ja sen hallintaosaaminen on nykyään tärkeä taito IT-ammattilaisille.

Lähestyn aihetta omasta kulmasta katsottuna. Olen ollut IT-alan infratehtävissä yli 20 vuotta, joista suurimman osan ajasta olen ollut kahden Suomen markkinassa ison palvelutarjoajan asiakasprojekteissa infra-asiantuntijana. Ala on tekniikan osalta jatkuvasti kehittyvä, mutta pilveistymisen myötä asiantuntijoilta vaadittava osaamisen määrä meni harppauksin eteenpäin ja vaikka on-premises (legacy) taitoja tarvitaan vielä pitkän aikaa, niin työmarkkinassa on-premises osaaminen on kokenut romahduksen.

Maailmantalous kyntää ja se näkyy yrityksien jatkuvina YT-neuvottelu kierroksina. Jos et vielä ole työtön työnhakija, niin pian saatat olla, jos et pidä huolta siitä, että pysyt kyvykkäänä ja hyödyllisenä yritykselle. Tästä kohtaa alkaa blogikirjoitukseni tarina. Parin YT-kierroksen jälkeen löysin itseni työttömyys uhan alta. Ansaitsemani palkka ei enää korreloinut siihen oikeuttavan osaamisprofiilin kanssa ja näin päädyin irtisanomisprosessiin. Onni onnettomuudessa on se, että minun kohdallani prosessi on aika pitkä ja siihen kuuluu kouluttautumisraha, joten nyt minulla on aikaa opiskella täyspäiväisesti ja voin käyttää koulutusrahaa sertifikaattitesteihin.

Pilveistymisen alkaessa en pitänyt huolta siitä, että osaamiseni kehittyy sitä mukaa kuin maailma muuttuu alalla. Työtä oli yllin kyllin on-prem projekteissa, Exchange Hybrid rakentamista ja pilvisiirtymiä yms... Kunnes tultiin siihen vaiheeseen, että nämä hommat vähentyivät merkittävästi ja yhtäkkiä uutta opittavaa asiaa oli valtavat määrät.

Niinpä tässä kirjoituksessa keskityn ruotimaan alalla pidempään työskennelleen näkökulmasta tilannetta missä aloittelee vasta nyt Azuren opiskelua, miten tarttua härkää sarvista ja miksi? Ja mitä Azure osaaminen ylipäätään on? Se kun ei oikeasti kerro mitään jos sanoo osaavansa Azurea. 

Ajatus tämän tekstin kirjoitukseen tuli omasta tilanteestani. Ajattelin, että mahdollisesti työelämässä on asiantuntijoita, joilla on samanlaisia asenteita Azuren opiskeluun kuin minulla oli aikaisemmin ja tässä koitan antaa ajatuksia näiden asenteiden muuttamiseksi.

Tämä kirjoitus on tarkoitus olla blogi-sarjan ei-tekninen aloitus ja jatkan sarjaa blogi-postauksina hands-on harjoituksista Azure opiskelujen edetessä. Sarjan focus on tarkoitus olla pääasiassa Infrastructure As Codessa.

# Millaista osaamista IT-alalla arvostetaan tänään?

Tänä päivänä IT alan työpaikkailmoituksia tutkiessa huomaa kuinka osaamisvaatimukset ovat asiantuntijarooleissa muuttuneet rajusti viime vuosien aikana. Pilvipalveluosaaminen nousee esiin yhtenä keskeisimmistä vaatimuksista. 

Alalla pitkään työskennelleiden on hyvä välillä katsella työpaikka ilmoituksia, vaikka ei olisi uutta työpaikkaa etsimässäkään. IT-ala muuttuu nopeasti, nyt varsinkin kun olemme eläneet viime vuodet "pilveistymisen" aikakautta. Työpaikkailmoituksista saa käsityksen siitä, mitä työnantajat tällä hetkellä arvostavat ja sitä voi peilata omaan osaamisprofiiliin varmistakseen työpaikkansa ja miksei mahdollisesti myös paremmasta palkasta haaveillessa. 

Toki työnantajien odottama osaaminen työpaikkailmoituksissa usein on vähän epärealistinen. Jos henkilö suvereenisti hallitsisi kaiken mitä monissa työpaikka ilmoituksissa odotetaan, niin kyseessä olisi todellinen kansainvälisen tason superosaaja, jotka ovat harvassa. Työpaikkailmoituksista silti saa suuntaa antavan käsityksen siitä minkälaisia osaajaprofiileja yritykset arvostavat tällä hetkellä.

Harvemmin työpaikka ilmoituksissa enää puhutaan on-premises osaamisesta, vaikka legacy palveluista ei vielä pitkään aikaan päästä irti. Varsinkaan isommissa ja kompleksisimmissa ympäristöissä. Sinänsä omituinen ilmiö, koska uudet alalle tulijat aloittavat opinnot suoraan pilvipalveluista ja on-prem palveluiden osaaminen on vähäistä. Jossain vaiheessa legacy osaaminen voi olla vielä hetken aikaa merkityksellistä.

Tänä päivänä Infra-asiantuntijoiden perusvaatimus on jo osata  Azurea ja lisäksi katsotaan hyvällä jos osaamista on myös AWS- tai Google Cloudista (vähän riippuu mitä näistä painotetaan riippuen asiantuntijaprofiilista). Lisäksi tietysti odotetaan, että M365 palveluiden hoitaminen kokonaisuudessaan sujuu myös.

Kun tällä hetkellä luen työpaikkailmoituksia, niin Azure osaamisessa korostetaan rajusti AI osaamista. Se on ymmärrettävää, koska alan kuuma uudehko aihe tällä hetkellä on AI ratkaisut. Aihe on aika uusi esim. MS osaamiskartassa, joten kysyntää on paljon ja tekijöiden määrä ei vielä kohtaa millään tavalla kysyntää. 

Lisäksi paljon näkee DevOpsiin, dataan ja securityyn liittyviä hakemuksia. 

Voisi siis olettaa, että johonkin näistä alueista kannattaa alkaa erikoistumaan, kun Azuren perustaidot alkaa olemaan hyppysissä.

# Miten Microsoftin sertifioinnit tukevat työmarkkinoilla menestymistä?

Hyvä työntekijä on tietysti paljon muutakin kuin sertifioinnit ja tekninen osaaminen. Asiantuntija roolissakaan tekninen osaaminen ei ole kaikki kaikessa, mutta toki se on keskeinen osa tätä roolia. 

Työnhakijana sertifioinnit auttavat näkymään työnhakijamassasta ja työnantajalle sertifioiduista osaajista voi olla sekä välitöntä että välillistä hyötyä. Välitön hyöty esimerkiksi siinä, että sertifioinnit voivat olla kytkyssä yrityksen kumppanuuteen kuten Microsoftin kanssa ja kumppanuudesta saatavin etuuksiin. Välillistä hyötyä esimerkiksi myynnissä saatava vaikutelma osaamisesta jne… Näin ollen sertifiointien merkitys ei pelkästään rajoitu oman osaamisen todistamiseen.

Aikaisemmin Microsoftin kumppaniohjelmassa oli käytössä Gold ja Silver -tasot, joissa yritykset ansaitsivat tunnustettuja kompetensseja suorittamalla tiettyjä sertifiointeja ja täyttämällä osaamisvaatimuksia. Tämä on nyt historiaa, ja tilalle on tullut Cloud Partner Program, jossa yritykset voivat saavuttaa Solutions Partner -statuksen tietyillä osa-alueilla.

Uudessa mallissa yrityksille kertyy osaamispisteitä kolmesta eri kategoriasta:

1. Performance (Suorituskyky) – Mittaa uusien asiakkuuksien ja käytössä olevien palveluiden määrää.
2. Skilling (Osaaminen) – Yrityksessä työskentelevien sertifioitujen asiantuntijoiden määrä ja heidän suorittamansa sertifikaatit.
3. Customer Success (Asiakashyöty) – Mittaa asiakasympäristöjen käyttöasteen ja edistyksen.

Yritykset tarvitsevat Solutions Partner Designation -tunnustuksia, jotka saavutetaan keräämällä riittävästi osaamispisteitä eri alueilla, kuten Infrastructure, Data & AI, Security, Business Applications, Digital & App Innovation ja Modern Work. Tämä tarkoittaa sitä, että työnantajat arvostavat asiantuntijoita, jotka suorittavat sertifiointeja, sillä se auttaa heidän yritystään saavuttamaan arvokkaamman kumppanistatuksen Microsoftin ekosysteemissä.

Näihin alueisiin tällä hetkellä laskettavat sertifioinnit voi tarkistaa Microsoftin Certification Posterista helpoiten (https://arch-center.azureedge.net/Credentials/Certification-Poster_en-us.pdF). Julisteen sertifikaatti laatikot toimivat myös linkkeinä sertifioinnin omalle nettisivulle, mistä pääsee 
    
- lueskelemaan mitä taitoja kyseisessä sertifioinnissa testataan
- Varaamaan testiaikoja
- Opiskelemaan sertifioinnin osa-alueita ilmaiseen Learn Training palveluun

Tässä kuvakaappaus PDFstä (klikkaa kuvaa, niin saat sen suuremmaksi):

![CertificationPoster](/certificationposter.png)

Kuvaa katsellessa voi vaan hymyillä ajatusta siitä, että mitä henkilö tarkoittaa sillä jos sanoo osaavansa Azurea?


# Asennoituminen opiskeluun


Miksi opiskelen Azurea vasta nyt, kun on kova paine saada taottua osaamista kalloon mahdollisemman nopeaan?

Siihen minulla oli oikeastaan kaksi keskeistä syytä joista ainakin pari syytä on tuttuja suurimmalle osalle alalla työskenteleville:

* Ajankäyttö
* Jaksaminen
* Ennakkoluulot

Ajankäyttö ja jaksaminen oikeastaan liittyvät hyvin läheisesti toisiinsa. IT asiantuntija viettää ensin työpäivän näyttöpäätteen ääressä ja sitten pitäisi jaksaa tuijottaa näyttöä lisää opiskelujen merkeissä. Lisäksi kun asiantuntija on kiinni työläässä projektissa tai aktiivisia projekteja on käynnissä samaan aikaan useampi, niin työpäivät helposti venähtävät paljon kahdeksan tuntia pidemmiksi. Siihen päälle vielä yksityiselämän ajankäyttö, niin saa olla mestari suunnittelemaan ajankäyttöä niin, että sinne mahtuisi mukaan myös opiskelua. Aika usein asiantuntijan pitää hyväksyä se fakta, että työ opettaa sen minkä opettaa.

Minun kohdalla ennakkoluulot tarkoittivat kahta seikkaa:

- Kuvittelin, että Azurea on vaikea opiskella, koska opiskelu tapahtuu maksullisella alustalla. Olen ollut liian pitkää siinä kankeassa "mindsetissä" että opiskelu ei saa maksaa itselle juuri mitään.
- Kuvittelin myös aina, että pilvipalveluiden päälle rakentaminen on loppujen lopuksi paljon helpompaa kuin on-prem tekeminen ja asiat on pitkälti samoja erilailla toteutettuna, niin eipä se mikään homma ottaa haltuun kunhan saa vaan aikaiseksi aloittaa opinnot.

Nyt kun olen opiskelut aloittanut ja pari sertifiointia suorittanut tuosta Microsoftin sertifiointikartasta, niin voin vain todeta että kummatkin näistä ennakkoluuloista oli vääriä. 

Azure opiskeluissa alkuun pääsee käyttämättä rahaa juuri ensinkään. Monet Azure Administrator Associate sertifioinnin osa-alueista on täysin ilmaisia Azuressa testailla. Maksullisiakin asioita testaillessa pääsee pitkän aikaa hyvin minimaalisilla kuluilla, koska testattavat asiat eivät vie montaa minuuttia ja kun on valmista niin poistaa heti kaikki resurssit mitä Azure subscriptionin alle teki. Näin kuukautta kohden jonkun verran maksua tulla, mutta alkuun opiskelu on pitkän aikaa sitä, että jos pitää vaan huolta, niin on ihme jos kuukauden opiskelu maksu ylittää edes yhden euron. Opiskelua ei siis kannata karttaa sen takia, että palvelu on maksullinen.

Mitä tulee toiseen ennakkoluuloon mikä minulla oli, niin kyllä pilvialustalle tekeminen tietyllä tapaa on helpompaa, mutta asiaa on vain niin valtava määrä että se ottaa hetken kun sulattelee lukemaansa ja testailee käytännössä Azuren eri palveluita.

Omalla kohdalla Azure opintojen alkuun pääseminen oli siis aika pitkälle asennemuutos. Ja nyt on aikakin opiskelulle järjestetty, joskin aika valitettavalla tavalla. Eipä kauheasti toivo, että kukaan joutuu päivittämään osaamistaan kilpaa ajan kanssa näissä olosuhteissa. Kannattaa siis fiksata ajankäyttö opiskelulle silloinkin kun työelämä vie ison osan omasta ajankäytöstä, niin ei välttämättä tarvitse nähdä irtisanomisprosessia.

Seuraavana, kun ajankäyttö ja asenne on kunnossa, niin tulee päätös siitä että miten opiskelua lähtee etenemään?


# Sertifiointiputki
 

Ilokseni voin sanoa, että Microsoftilla on suuret määrät opiskelua tukevaa ilmaista oppimateriaalia. Sertifiointitavoitteinen opiskelu helpottaa myös sitä, että opiskelu ei rönsyile sinne tänne, vaan keskittyy opiskeluissa vain kulloinkin työn alla olevien sertifiointien aihealueisiin. Kun opittavaa asiaa on paljon, niin sertifiointeihin opiskelu tuo opintoihin järjestelmällisyyttä. Kun sertifoinnit ohjaavat opiskelua, niin sitten vain pitää määritellä sertifiointi polku mitä lähtee opiskelemaan.

Infra-asiantuntijalle suosittelen aloittamista Azure Administrator (AZ-104) testistä. Taitaa oikeastaan tuossa julisteen kartassa sertit mennäkin suositellussa järjestyksessä ylhäältä alaspäin. Ainakin infrastructure sarakkeessa. 

Tein itse nimittäin virheen suorittamalla ensin Windows Server Hybrid Administrator  Associate sertifioinnin (AZ-800 ja AZ-801 testit). 

Ajattelin, että tuo on vanha Windows Server testi mihin on tuotu hybrid server asiaa mukaan. Ja sitähän tuo testi kyllä osittain onkin, mutta tuntui pitkästi siltä, että sertifikaatissa painotettiin Hyper-V klusteria ja moniko infra-asiantuntija tekee päivittäin työssään Hyper-V klusteireita? Lisäksi tässä sertifioinnissa Azure teknologiat tuntuvat hyvin abstrakteilta komponenteilta kun alla ei ole sertifiointeja joissa näihin palveluiden mennään syvemmin. Muutenkin varsinkin kun tässä aikaa vastaan koittaa saada suoritettua sertifiointeja, niin se, että tämä sertifiointi vaatii kaksi sertifiointitestiä tuntui aika rajulta, muiden sertifiointien ollessa sertifiointi per hyväksytty testi. Meni siis hetki ennen kuin sai tuntea onnistumisen tunnetta, mikä voi olla alkuun pääsemisessä tärkeää vaikka ei edes opiskelisi aikaa vastaan.

Seuraavaksi olisi tarkoitus aloittaa Azure Network Engineer opinnot. Voi olla, että sen jälkeen hylkään hetkeksi Infrastructure sarakkeen ja siirryn joko securityyn tai DevOpsiin. Tietoturva sinänsä olisi aika selkeä ja ehkä vähemmän vaativa alue sisäistää, kun ei ole varsinaista koodari osaamista eikä työelämän osaamista konteista, mutta en nyt ole vielä tätä kirjoittaessa tutkinut, että tarvitseeko DevOps suoritukset välttämättä niinkään koodari osaamista. Azure Administrator sertifiointia opiskellessa viimeistään alkaa ymmärtää Azuren Infrastructure As Coden tehokkuuden ja oletan, että IaC osaaminen syventyy DevOpsissa, mutta saatan olla väärässäkin.

Nyt kun kompetenssi ajattelua ei enää ole MS kumppanuus tasoissa ja jos haluaa ajatella että suoritetuilla sertifioinneilla haluaa tehdä itsensä kiinnostavaksi kohteeksi rekryissä tai tuottaa arvoa yritykselle muutenkin kuin pelkällä työn teolla, niin kannattaisi varmaan ajatella, että sertifiointeja olisi usealta certification posteriin määritellyillä osa-aluella. Vähintään kahdesta eri laatikosta.

Esimerkiksi infraosaajalle suositeltavia sertifiointikombinaatioita voisi olla:

- Azure Administrator (AZ-104) → Azure Solutions Architect (AZ-305) → Azure Security Engineer (AZ-500)
- Azure Administrator (AZ-104) → Azure Network Engineer (AZ-700) → Azure Security Engineer (AZ-500)

Jos haluaa erottua työmarkkinoilla entistä enemmän, voi harkita myös DevOps-, Data- tai Security-haaran sertifiointeja, kuten Azure DevOps Engineer (AZ-400) tai Security-haaran sertifikaatit (SC-200, SC-300, SC-100).

Itsellä ei data alueeseen ole tällä hetkellä oikein intoa tarttua, koska siellä tulee SQL asiat vastaan, mitä en ole koskaan opiskellut kuin vähän ohi mennen. Kiinnostaa kyllä, mutta siihen ei välttämättä ihan lyhyellä opiskelulla pääse vielä kunnolla sisään. Ja niin houkuttelevaa kuin AI onkin työpaikkailmoitusten perusteella, niin se taitaa olla kuitenkin enemmän koodareiden valtakuntaa (en ole perehtynyt).

Miten aloittaa Azure-opiskelu tehokkaasti?

1. Järjestä ajankäyttö
    - Ajankäytön järjestäminen työelämän ohessa on priorisointipäätös. Kuinka paljon on valmis uhraamaan omaa vapaa-aikaa oman työelämän osaamisen ylläpitoon?
    - Miten järjestää aika opiskeluun, niin että jaksaa opiskella? Ei sen loppujen lopuksi tarvitse olla paljon aikaa per viikko, jos vain opiskelee säännöllisesti. Arkena ei välttämättä kannata asialle uhrata paljon aikaa, mutta jos vaikka ottaisi tavaksi iltasella lukea lyhyen aikaa MS Learniä? Ei niin paljon, että ei kerkeä vähän katsomaan telkkariakin yms.. Viikonloppuaamuisin ennen kuin rientää viikonlopun muihin askareisiin, niin opiskelisi pari tuntia lauantaina ja sunnuntaina? Tekisi siinä vähän hands on opiskelua Azuressa etc…?
    - Kun nämä asiat kondiksessa, niin sitten yrittää vaan rohkeasti työelämässä päästä tekemään asioita joita kotona parhaillaan opiskelee.
          
2. Asenne kohdilleen
    - Vaikka osa opiskelusta voi olla maksullista, niin hinnat on varsin maltillisia ja mitättömiä kun pitää huolen, että Azure resurssit on poistettu välittömästi kun lopettaa hands on opiskelun.
        
3. Tekniset valmistelut ennen opiskelun aloittamista
    - Luo oma Azure-tili, mikä on täysin maksuton kun palvelussa ei ole asennettuna resursseja joista tulee kuluja.
    - Ota käyttöön GitHub ja asenna VS Code kehitysympäristöksi. Jos olet on-prem asiantuntija, niin luultavasti et ole käyttänyt muuta editoria kuin korkeintaan Powershell ISEä. VS Code on täysin suvereeni editorina verrattuna ISEen ja sen käytön opiskelu kannattaa aloittaa heti alusta lähtien kun alkaa opiskelemaan Azurea. VS Coden lisäksi haluat oppia käyttämään Githubia ja opetella heti alkuun miten datan commitointi ja push omalta koneelta Githubin repositoryyn tapahtuu.
    - Hyödynnä GitHub Copilotin ilmaista versiota opiskelun tukena. VS Codeen integroitu AI on opiskelun ja editorissa työskentelyn kannalta niin ehdoton apu, että sitä välillä miettii, että miten kukaan on oppinut mitään ennen AI työkaluja.
        
4. Hyödynnä tarjolla olevia opiskelumateriaaleja
    - Microsoft Learn ( https://learn.microsoft.com/en-us/training/) tarjoaa palveludokumentaatioiden lisäksi ilmaisia kursseja ja opintopolkuja omatoimiseen opiskeluun sertifiointitestiin valmistelevassa opiskelussa.
    - Azure-dokumentaatio ( https://learn.microsoft.com/en-us/azure/) syventää ymmärrystäsi ja auttaa testeihin valmistautumisessa.
    - Microsoft Partner University on Microsoftin koulutusalusta, joka on suunnattu Microsoftin kumppaneille. Se tarjoaa kattavasti oppimateriaalia, joka liittyy Microsoftin teknologioihin, myyntiin, asiakasmenestykseen ja kumppanuusohjelmiin. Pitäisi löytyä myös teknistä asiantuntijakoulutusta. En tiedä tästä oikeastaan paljoakaan, huomasin tätä blogikirjoitusta kirjoittaessa että tällainenkin on näköjään olemassa.
    - Harjoituskokeet (esim. MeasureUp) auttavat hahmottamaan, milloin olet valmis varsinaiseen testiin. Kun hankit vouchereita sertifiointi testiin, niin osta MeasureUp harjoitustesti kylkeen, mikä ei lisää hintaa montaa kymppiä ja voit harjoitustestillä kartoittaa valmiuttasi testiin ja huomaat mikäli jokin osa-alue on vielä vähän heikosti hallinnassa.

Miten ikinä lopulta päätyykään kehittämään osaamistaan, niin tärkeintä varmasti on se, että sitä malttaa tehdä opiskelua jatkuvasti, eikä tuudittaudu pelkästään siihen, että työ opettaa tekijäänsä.
