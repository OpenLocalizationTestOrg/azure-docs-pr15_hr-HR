<properties
    pageTitle="Povezivanje računalima i uređajima OMS pomoću pristupnika OMS | Microsoft Azure"
    description="OMS upravlja uređaja i računala Operations Manager nadzirati povezivanje s pristupnika OMS nemaju pristup Internetu OMS usluga slanja podataka."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>
<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="banders"/>

# <a name="connect-computers-and-devices-to-oms-using-the-oms-gateway"></a>Povezivanje računalima i uređajima OMS pomoću OMS pristupnika

Ovaj dokument u članku se opisuje kako vaše OMS upravlja uređaja i računala na sustav centar operacije Upravitelj SCOM nadzirati možete poslati podatke OMS usluga za čije su pristup Internetu. OMS pristupnika možete prikupiti podatke i poslali ga u OMS usluga u njihovo ime.

Pristupnik je HTTP prosljeđivanje proxy koji podržava HTTP tuneliranje pomoću naredbe za POVEZIVANJE HTTP-a. Pristupnika možete rukovati do 2000 OMS istovremeno povezani uređaja kada se izvoditi na 4 core procesora 16 GB poslužitelj sa sustavom Windows.

Kao primjer enterprise ili velike tvrtke ili ustanove možda poslužitelji veza s mrežom, no možda neće imati mogućnost povezivanja s Internetom. U drugi primjer možda imate mnogo točku Prodaja (Položaj) uređaja s bez srednje vrijednosti ih izravno nadzor. I u drugi primjer Operations Manager možete koristiti pristupnika OMS kao proxy poslužitelj. U ovim se primjerima OMS pristupnika možete prenijeti podatke s agenata koji su instalirani na ove poslužitelje ili Položaj uređajima OMS.

Umjesto svaki agent za pojedinačne slanje podatke izravno u OMS i potrebno izravnu internetsku vezu, svi podaci agent umjesto šalje se putem s jednog računala s internetskom vezom. Računalu je kojoj instalirati i pomoću pristupnika. U ovom scenariju agenata možete instalirati na računalima s bilo koje mjesto na koje želite da biste prikupili podatke. Pristupnik zatim prenosi podatke iz na agenata OMS izravno – pristupnik analizirati bilo koji od podataka koji se prenose.

Možete nadzirati OMS pristupnika i analizirati performanse i podaci o događaju na poslužitelju na kojem je instaliran, morate instalirati OMS agent na računalu na kojem je pristupnika i instaliran.

Pristupnik mora imati pristup internetu da biste prenijeli podatke na OMS. Agent za svaki mora imati mrežne veze radi njegova pristupnika tako da se agenata automatski možete prenijeti podatke iz pristupnika. Da biste postigli najbolje rezultate, ne može instalirati pristupnika na računalima na kojima je kontroler domene.

Ovo je dijagram koji prikazuje toka podataka iz Izravni agenata OMS.

![Dijagram izravno agent](./media/log-analytics-oms-gateway/direct-agent-diagram.png)

Ovo je dijagram koji prikazuje toka podataka iz komponente Operations Manager OMS.

![Operacije Upravitelj dijagrama](./media/log-analytics-oms-gateway/scom-mgt-server.png)

## <a name="install-the-oms-gateway"></a>Instalacija OMS pristupnika

Instaliranje ovaj pristupnika zamjenjuje prethodne verzije pristupnika Provjerite jeste li instalirali (Prijava analitiku Forwarder).

Preduvjeti: .net Framework 4,5, Windows Server 2012 R2 SP1 i noviji

1. Preuzmite najnoviju verziju pristupnika OMS iz [Microsoftova centra za preuzimanje](http://download.microsoft.com/download/2/5/C/25CF992A-0347-4765-BD7D-D45D5B27F92C/OMS%20Gateway.msi).
2. Da biste pokrenuli instalaciju, dvokliknite **OMS Gateway.msi**.
3. Na stranici dobrodošlice, **Dalje**.  
    ![Čarobnjak za postavljanje pristupnika](./media/log-analytics-oms-gateway/gateway-wizard01.png)
4. Na stranici licencni ugovor, odaberite **prihvaćam uvjete licencnog ugovora** biste na licencni ugovor, a zatim **Dalje**.
5. Na stranici adresa za priključak i proxy poslužitelja:
    1. Unesite broj priključka TCP će se koristiti za pristupnik. Postavljanje otvorit će se taj broj priključka iz Vatrozid za Windows. Zadana vrijednost je 8080.
    Valjani raspon broj priključka je 1 65535. Ako unos podijeljene ovaj raspon, prikazuje se poruka o pogrešci.
    2. Po želji poslužitelja na kojem je instaliran pristupnik nije potrebno koristite proxy poslužitelj, upišite adresu proxy gdje treba pristupnika da biste se povezali. Na primjer, `http://myorgname.corp.contoso.com:80` ako prazno, pristupnika će pokušati izravno povezati s Internetom. U suprotnom pristupnika povezuje proxy poslužitelj. Proxy poslužitelj zahtijeva provjeru autentičnosti, upišite svoje korisničko ime i lozinku.
        ![Konfiguraciju proxy čarobnjak pristupnika](./media/log-analytics-oms-gateway/gateway-wizard02.png)  
    3. Kliknite **Dalje**
6. Ako nije omogućena Updates Microsoft, pojavit će se stranica Microsoft Update kojem možete odabrati da biste omogućili Microsoftova Updates. Odaberite neku stavku, a zatim kliknite **Dalje**. U suprotnom, prijeđite na sljedeći korak.
7. Na stranici odredišnu mapu, ostavite mapu zadane **%ProgramFiles%\OMS pristupnika** ili upišite mjesto na mjesto na koje želite instalirati pristupnika, a zatim kliknite **Dalje**.
8. Na jeste li spremni za instalaciju stranice, kliknite **Instaliraj**. Kontrola korisničkih računa može prikazati traži dozvolu za instaliranje. Ako želite, kliknite **da**.
9. Nakon dovršetka instalacije kliknite **Završi**. Možete provjeriti servis pokrenut je tako da otvorite dodatak services.msc, a provjerite prikazuje li se **OMS pristupnika** na popisu servisa.  
    ![Usluge – OMS pristupnika](./media/log-analytics-oms-gateway/gateway-service.png)

## <a name="install-an-agent-on-devices"></a>Instalacija agent na uređaje

Ako je potrebno, potražite u članku [Povezivanje Windows računalima prijava analitiku](log-analytics-windows-agents.md) informacije o instalaciji izravno povezani agenata. U članku se opisuje kako biste mogli instalirati agent pomoću čarobnjaka za postavljanje ili pomoću naredbenog retka.

## <a name="configure-oms-agents"></a>Konfiguriranje OMS agenata

Potražite u članku [Konfiguracija postavki proxy poslužitelja i vatrozida s Microsoft Agent nadzor](log-analytics-proxy-firewall.md) informacije o konfiguriranju agent za korištenje proxy poslužitelj, što je taj slučaj je pristupnika.

Operacije Upravitelj agenata slanje podataka kao što su Operations Manager upozorenja, konfiguraciji Procjena, prostora instanci i kapacitet podataka, putem poslužitelja za upravljanje. Ostali podaci visoku glasnoću, kao što su zapisnike, performanse i sigurnost IIS šalju izravno OMS pristupnika. Popis svih podataka koji se šalju putem svakog kanala potražite u članku [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md) .

>[AZURE.NOTE]
Ako namjeravate koristiti pristupnika s mrežnog opterećenja potražite u članku [možete konfigurirati za ujednačavanje opterećenja mreže](#optionally-configure-network-load-balancing).

## <a name="configure-a-scom-proxy-server"></a>Konfiguriranje SCOM proxy poslužitelja

Konfiguriranje komponente Operations Manager da biste dodali pristupnika će poslužiti kao proxy poslužitelj. Kada ažurirate konfiguracija proxy poslužitelj, konfiguraciju proxy automatski primjenjuje se na sve agenata operacije rukovoditelju.

Da biste koristili pristupnik za podršku Operations Manager, morate koristiti:

- Microsoft Agent nadzor (verzija agent – **8.0.10900.0** i novijim verzijama) instalirana na poslužitelju pristupnika i konfiguriran za radne prostore OMS s kojom želite komunicirati.
- Pristupnik moraju imati mogućnost povezivanja s Internetom ili se povezati s proxy poslužitelja koji ne.

### <a name="to-configure-scom-for-the-gateway"></a>Da biste konfigurirali SCOM pristupnika

1. Otvorite konzolu za komponentu Operations Manager i u odjeljku **Operacija upravljanja paket**, kliknite **vezu** , a zatim kliknite **Konfiguracija Proxy poslužitelja**:  
    ![Konfiguriranje komponente Operations Manager – Proxy poslužitelja](./media/log-analytics-oms-gateway/scom01.png)
2. Odaberite **koristi proxy poslužitelj za pristup paket za upravljanje operacije** , a zatim upišite IP adresu poslužitelja OMS pristupnika. Provjerite je li započnete s na `http://` prefiks:  
    ![Operations Manager – adresa proxy poslužitelja](./media/log-analytics-oms-gateway/scom02.png)
3. Kliknite **Završi**. Poslužitelj za komponentu Operations Manager povezan je s OMS radnog prostora.

## <a name="configure-network-load-balancing"></a>Konfiguriranje mrežnog opterećenja

Možete konfigurirati pristupnik za visoke dostupnosti pomoću stvaranjem klaster za ujednačavanje opterećenja za mrežu. Klaster upravlja promet iz vaše agenata po preusmjeravanja tražene veze iz Microsoftove agente za nadzor preko svoje čvorove. Ako funkcionira jedan pristupnik poslužitelj, promet dobiva preusmjereni na ostale čvorove.

1. Otvorite upravitelj za ujednačavanje učitavanja mreže i stvorite klaster.
2. Desnom tipkom miša kliknite klaster prije nego što dodate pristupnika, a zatim odaberite **Svojstva klaster.** Konfiguriranje klaster imati vlastitu IP adresa:  
    ![Mrežni učitavanja opterećenja Manager – klaster IP adresa](./media/log-analytics-oms-gateway/nlb01.png)
3. Povezati s poslužiteljem OMS pristupnika s na Microsoft nadzor instaliran Agent desnom tipkom miša kliknite na klaster IP adresa, a zatim **Dodajte glavno računalo klaster**.  
    ![Mrežni učitati ujednačavanje Upravitelj – Dodavanje glavnog računala klaster](./media/log-analytics-oms-gateway/nlb02.png)
4. Upišite IP adresu poslužitelja u pristupnika koji se želite povezati:  
    ![Mrežni upravitelja za ujednačavanje opterećenja – Dodavanje glavnog računala klaster: povezivanje](./media/log-analytics-oms-gateway/nlb03.png)
5. Na računalima s Internetom, obavezno upišite IP adresu klaster Kada konfigurirate **Svojstva Agent nadzor Microsoft**:  
    ![Microsoft Agent svojstva – postavke proxy poslužitelja za nadzor](./media/log-analytics-oms-gateway/nlb04.png)

## <a name="configure-for-automation-hybrid-workers"></a>Konfiguriranje za zaposlenici zaduženi za hibridno automatizacije

Ako zaposlenici zaduženi za automatizaciju hibridnog u svom okruženju, sljedeći koraci pružaju ručnog, privremene zaobilazna rješenja da biste konfigurirali pristupnik ih podržava.

U sljedećim koracima, morate znati Azure regije gdje se nalazi na račun za automatizaciju. Da biste pronašli mjesto:

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Odaberite servis za automatizaciju Azure.
3. Odaberite odgovarajući račun za Azure automatizaciju.
4. Prikaz njegova područja u odjeljku **lokacije**.  
    ![Portal za Azure – mjesto računa za automatizaciju](./media/log-analytics-oms-gateway/location.png)

Pomoću sljedećih tablica za prepoznavanje URL-a za svaku lokaciju:

**Zadatak izvođenja podataka servisa URL-ova**

| **mjesto** | **URL-A** |
| --- | --- |
| Sjeverna središnje SAD-a | ncus-jobruntimedata-RN-su1.azure-automation.net |
| Europa Zapad | ne možemo-jobruntimedata-RN-su1.azure-automation.net |
| Južna središnje SAD-a | scus-jobruntimedata-RN-su1.azure-automation.net |
| Istočni SAD-a | eus-jobruntimedata-RN-su1.azure-automation.net |
| Središnja Kanada | kopija-jobruntimedata-RN-su1.azure-automation.net |
| Sjeverna Europa | No-jobruntimedata-RN-su1.azure-automation.net |
| Južna istočnoazijski | Sea-jobruntimedata-RN-su1.azure-automation.net |
| Središnje Indija | CID-jobruntimedata-RN-su1.azure-automation.net |
| Japan | jpe-jobruntimedata-RN-su1.azure-automation.net |
| Australija | elika i mala slova-jobruntimedata-RN-su1.azure-automation.net |

**Agent servisa URL-ova**

| **mjesto** | **URL-A** |
| --- | --- |
| Sjeverna središnje SAD-a | ncus-agentservice-RN-1.azure-automation.net |
| Europa Zapad | ne možemo-agentservice-RN-1.azure-automation.net |
| Južna središnje SAD-a | scus-agentservice-RN-1.azure-automation.net |
| Istočni SAD-a | eus2-agentservice-RN-1.azure-automation.net |
| Središnja Kanada | kopija-agentservice-RN-1.azure-automation.net |
| Sjeverna Europa | No-agentservice-RN-1.azure-automation.net |
| Južna istočnoazijski | Sea-agentservice-RN-1.azure-automation.net |
| Središnje Indija | CID-agentservice-RN-1.azure-automation.net |
| Japan | jpe-agentservice-RN-1.azure-automation.net |
| Australija | elika i mala slova-agentservice-RN-1.azure-automation.net |

Ako računalo je registrirana kao hibridnog tempiranja automatski za zakrpa pomoću rješenja za ažuriranje upravljanja, slijedite ove korake:

1. Dodavanje URL-ove usluge posao izvođenja podataka na popisu dopušteni glavnog računala pristupnika OMS. Ako, na primjer: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Ponovno pokrenite servis OMS pristupnika pomoću cmdleta komponente PowerShell sustava sljedeće:`Restart-Service OMSGatewayService`

Ako je računalo na-boarded za automatizaciju Azure pomoću cmdleta za registraciju hibridnog tempiranja, poduzmite sljedeće korake:

1. Dodajte URL za registraciju agent servisa na popisu dopušteni glavnog računala pristupnika OMS. Ako, na primjer:`Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Dodavanje URL-ove usluge posao izvođenja podataka na popisu dopušteni glavnog računala pristupnika OMS. Ako, na primjer: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Ponovno pokrenite servis OMS pristupnika.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Korisni cmdleta ljuske PowerShell

Cmdleti za omogućuju zadatke koji su potrebni za ažuriranje postavki konfiguriranje pristupnika OMS. Prije nego što ih koristiti, ne zaboravite:

1. Instalirajte OMS pristupnika (MSI).
2. Otvorite prozor PowerShell.
3. Da biste uvezli modul, upišite sljedeću naredbu:`Import-Module OMSGateway`
4. Ako nije bilo pogrešaka u prethodnom koraku, modul je uspješno uvezena, a može se koristiti s cmdletima. Vrsta`Get-Module OMSGateway`
5. Kad unesete promjene pomoću na Cmdlete, provjerite je li ponovno pokrenite servis pristupnika.

Ako dođe do pogreške u 3, uvesti nije modul. Pogreška može pojaviti kada je nije moguće pronaći modul ljuske PowerShell. Možete pronaći u put instalacije na pristupnika: C:\Program File\Microsoft OMS Gateway\PowerShell.

| **Cmdlet** | **Parametri** | **Opis** | **Primjeri** |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` | Ključ (obavezno) <br> Vrijednost | Promjene konfiguracije servisa | `Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` | Ključ | Dohvaća konfiguraciju servisa | `Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` | Adresa <br> Korisničko ime <br> Lozinke | Postavlja adrese (i vjerodajnica) prijenos (upstream) proxy poslužitelja | 1. postavite odgovor proxy i vjerodajnicu:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. Postavi proxy odgovor koji se ne mora provjere autentičnosti:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3. poništite odgovor postavka za proxy poslužitelj, odnosno ne morate proxyja odgovor:`Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` |   | Dobiti adresu preusmjeravanja (upstream) proxy poslužitelja | `Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` | Glavno računalo (obavezno) | Dodaje glavnog računala na popis dopuštenih | `Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHos`t | Glavno računalo (obavezno) | Uklanja glavno računalo s popisa dopuštenih | `Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` |   | Dobiti trenutno dopuštene glavnog računala (samo lokalno konfigurirani dopuštena glavno računalo, nemojte uvrstiti automatski preuzete dopuštene glavnog računala) | `Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` | Predmet (obavezno) | Dodaje klijentski certifikat koji se primjenjuju na popis dopuštenih | `Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` | Predmet (obavezno) | Uklanja predmet certifikat klijenta s popisa dopuštenih | `Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificat`e |   | Dobiti trenutno dopuštene klijent predmeta certifikat (samo lokalno konfigurirani dopuštene predmeta, nemojte uvrstiti automatski preuzete dopuštene predmeta) | `Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshoot"></a>Rješavanje problema

Preporučujemo da instalirate OMS agent na računalima na kojima ste instalirali pristupnika. Zatim možete koristiti agenta za prikupljanje događaji koje pristupnik.

![Preglednik događaja – OMS pristupnika zapisnika](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS pristupnika događaj ID-ova i opisa**

Sljedeća tablica prikazuje ID-ovi događaja i opisi OMS pristupnika zapisnika događaja.

| **ID-A** | **Opis** |
| --- | --- |
| 400 | Pogreška sve aplikacije koje sadrže određene ID-a |
| 401 | Pogrešan konfiguracija. Na primjer: listenPort = "tekst" umjesto cijeli broj |
| 402 | Iznimke u Raščlanjivanje TLS rukovanja porukama |
| 403 | Pogreška s mrežom. Na primjer: ne može povezati s poslužiteljem cilj |
| 100 | Opće informacije |
| 101 | Servis je pokrenut |
| 102 | Servis je zaustavljeno |
| 103 | Naredba za POVEZIVANJE HTTP poslao klijenta |
| 104 | Ne i POVEZIVANJE HTTP naredbi |
| 105 | Odredišni poslužitelj ne nalazi na popisu dopuštenih ili s odredišnim priključkom nije sigurna priključak (443) <br> <br> Provjerite jesu li agent MMA na poslužitelj pristupnika i agenata komunikaciju s pristupnika povezani s istim radnim prostorom zapisnika analize.|
| 105 | Pogreška TcpConnection – klijenta nije valjan certifikat: CN = pristupnika <br><br> Pobrinite se da: <br> <br> & #149; Koristite pristupnik s brojem verzije 1.0.395.0 ili noviji. <br> & #149; Agent za MMA na poslužitelj pristupnika i agenata komunikaciju s pristupnika povezani isti prostor zapisnika analize. |
| 106 | Iz bilo kojeg razloga koji je sesiju TLS sumnjivih i odbačene |
| 107 | Sesije TLS potvrdio |

**Mjerača performansi za prikupljanje**

Sljedeća tablica prikazuje dostupne mjerača performansi OMS pristupnika. Možete dodati mjerača pomoću nadzora performansi.

| **Ime** | **Opis** |
| --- | --- |
| OMS pristupnika/aktivno klijent veze | Broj aktivnog klijentske mreže (TCP) veze |
| Broj OMS pristupnika/pogrešaka | Broj pogreške |
| Klijentski pristupnik/povezani OMS | Broj povezanih klijenata |
| Broj pristupnika/odbijanje OMS | Broj odbijanja zbog pogreške pri provjeri valjanosti sve TLS |

![Pristupnik OMS mjerača performansi](./media/log-analytics-oms-gateway/counters.png)


## <a name="get-assistance"></a>Pomoć

Kada se niste prijavili u portal za Azure, možete stvoriti na zahtjev za pomoć s OMS pristupnika ili bilo kojim drugim Azure service ili značajke nekog servisa.
Da biste zatražili pomoć, kliknite simbol upitnik u gornjem desnom kutu portalu, a zatim kliknite **Novi zahtjev za podršku**. Popunite novi obrazac zahtjeva za podršku.

![Novi zahtjev za podršku](./media/log-analytics-oms-gateway/support.png)

Ostavite povratne informacije o OMS ili prijava analitiku na [forumu Microsoft Azure povratne informacije](https://feedback.azure.com/forums/267889).

## <a name="next-steps"></a>Daljnji koraci

- [Dodavanje izvora podataka](log-analytics-data-sources.md) da biste prikupili podatke iz izvora povezani u radnom prostoru OMS i spremite ga u spremištu OMS.
