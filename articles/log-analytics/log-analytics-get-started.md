<properties
    pageTitle="Početak rada s prijava analitiku | Microsoft Azure"
    description="S radom možete postići pomoću zapisnika analize u u programu Microsoft operacije upravljanja paket (OMS) u minutama."
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
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="get-started-with-log-analytics"></a>Početak rada s zapisnika Analytics

S radom možete postići pomoću zapisnika analize u u programu Microsoft operacije upravljanja paket (OMS) u minutama. Prilikom odabira kako stvoriti OMS radni prostor, koja je slična računa imate dvije mogućnosti:

- Paket programa Microsoft operacije upravljanja web-mjesta
- Pretplate na Microsoft Azure

Možete stvoriti besplatne OMS prostor pomoću OMS web-mjesta. Ili, Microsoft Azure pretplate možete koristiti radi stvaranja radnog prostora OMS. Obje radne prostore su functionally ekvivalentan, osim što slobodnog prostora OMS možete samo poslati 500 MB podataka svakodnevno OMS usluga. Ako koristite Azure pretplatu, možete koristiti i tu pretplatu da biste pristupili ostalim Azure servisima. Bez obzira na način koji koristite za stvaranje radnog prostora sustava ćete stvoriti radni prostor s Microsoftov račun ili račun tvrtke ili ustanove.

Evo susret postupak:

![za uhodavanje dijagrama](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Prijava analitiku preduvjeti i pitanja vezana uz implementaciju

- Potreban vam je plaćenu pretplatu na Microsoft Azure potpuno korištenje zapisnika analize. Ako nemate pretplatu na Azure, stvorite [pomoću računa](https://azure.microsoft.com/free/) koji omogućuje pristup bilo koji servis za Azure. Ili možete stvoriti pomoću računa OMS na web-mjestu [Operacije paket za upravljanje](http://microsoft.com/oms) i kliknite **isprobajte besplatno**.
- Radni prostor programa OMS
- Svakom računalu sa sustavom Windows koje želite prikupiti podatke iz morate pokrenuti Windows Server 2008 SP1 ili noviji
- Pristup [vatrozid](log-analytics-proxy-firewall.md) na OMS web-adrese servisa
- Poslužitelj za [OMS prijava analitiku Forwarder](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (pristupnik) da biste proslijedili promet s poslužitelja OMS, ako pristup Internetu nije dostupan na računalima
- Ako koristite prijava analitiku podržava operacije Upravitelj 2012 SP1 UR6 Operations Manager i iznad i operacije Upravitelj 2012 R2 UR2 i veće. Podrška za proxy je dodana u operacije Upravitelj 2012 SP1 UR7 i operacije Upravitelj 2012 R2 UR3. Odredite kako ga će integriran s OMS.
- Određuje ima li vaše računalo izravan pristup Internetu. Ako ne trebaju pristupnika server da biste pristupili OMS web-mjesta servisa. Sve pristup je putem HTTP.
- Odredite koji su tehnologije i poslužitelji će poslati podatke OMS. Na primjer, kontrolera domena sustava SQL Server, itd.
- Dodijelite dozvolu korisnici OMS i Azure.
- Ako ste zabrinuti podatkovni promet, pojedinačno implementacija svakog rješenja i testiranje učinak na performanse prije dodavanja dodatnih rješenja.
- Pregledajte podatkovni promet i performanse prilikom dodavanja rješenja i značajki analize zapisnika. To obuhvaća događaj zbirke prikupljanje zapisnika, prikupljanje podataka performanse, itd. Je bolje započeti s minimalnim zbirke do podatkovni promet ili otkrio utjecaj na performanse.
- Provjerite je li da Windows agenata ne i upravlja se pomoću Operations Manager, u suprotnom će rezultat duplicirane podatke. Vrijedi i za Azure temelji-agenata koji imaju Azure Dijagnostika omogućena.
- Nakon što instalirate agenata, provjerite funkcionira li agenta ispravno. Ako nije, provjerite da koji šifriranja API-JA: sljedeći generacije (CNG) ključ odvajanja je onemogućen putem pravila grupe.
- Neka rješenja prijava analitiku su dodatni preduvjeti



## <a name="sign-up-in-3-steps-using-the-operations-management-suite"></a>Prijavite se u 3 korake pomoću upravljanja paket operacije

1. [Operacije paket za upravljanje](http://microsoft.com/oms) web-mjesto, a zatim kliknite **isprobajte besplatno**. Prijavite se pomoću Microsoftova računa kao što je Outlook.com ili s računa tvrtke ili ustanove nudi vaša tvrtka ili obrazovna ustanova koristi Office 365 ili druge Microsoftove servise.
2. Navedite naziv jedinstveni radnog prostora. Radni prostor je logički spremnik pohrane podataka upravljanja. Ga omogućuje vam particija podataka između različitih timova u tvrtki ili ustanovi kao podaci za njegov radnog prostora. Navedite adresu e-pošte i regija na mjesto na koje želite imati podatke pohranjene.  
    ![Stvaranje radnog prostora i povezivanje pretplate](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Nakon toga možete stvoriti novu pretplatu za Azure ili povezivanje s postojećom Azure. Ako želite nastaviti pomoću besplatnu probnu verziju, kliknite **Ne sada**.  
  ![Stvaranje radnog prostora i povezivanje pretplate](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

Spremni ste za početak rada s portala paket za upravljanje operacije.

Možete saznati više o postavljanju radnog prostora i povezivanje postojeće Azure računa u radne prostore stvorene pomoću paket za upravljanje operacije na [Upravljanje pristupom zapisnika analize](log-analytics-manage-access.md).

## <a name="sign-up-quickly-using-microsoft-azure"></a>Prijavite se brzo pomoću Microsoft Azure

1. Idite na [portal za Azure](https://portal.azure.com) i prijavite se u, pregledajte popis servisa i odaberite **Prijava analitiku (OMS)**.  
    ![Portal za Azure](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Kliknite **Dodaj**, a zatim odaberite mogućnosti za sljedeće stavke:
    - Naziv **OMS radnog prostora**
    - **Pretplata** – ako imate više pretplata, odaberite onaj koji želite pridružiti novi radni prostor.
    - **Grupa resursa**
    - **Mjesto**
    - **Cijene sloju**  
        ![brzo stvaranje](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Kliknite **Stvori** i prikazat će vam Detalji radnog prostora na portalu za Azure.       
    ![Detalji o radnog prostora](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Kliknite vezu **OMS Portal** da biste otvorili operacije paket za upravljanje web-mjesta s novi radni prostor.

Spremni ste za početak rada s portala za operacije paket za upravljanje.

Možete saznati više o postavljanju radnog prostora i povezivanje postojećih radnih prostora koji ste stvorili s paket za upravljanje operacije za Azure pretplate na [Upravljanje pristupom zapisnika analize](log-analytics-manage-access.md).

## <a name="get-started-with-the-operations-management-suite-portal"></a>Početak rada s portala paket za upravljanje operacije
Odaberite rješenja i povezati se s poslužiteljima koji želite upravljati, kliknite pločicu **Postavke** , a zatim slijedite korake u ovom odjeljku.  

![Početak rada](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Dodavanje rješenja** - prikaz svojih instaliranih rješenja.  
    ![rješenja](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Kliknite da biste dodali više rješenja **posjetiti galeriju** .  
    ![rješenja](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Odaberite rješenje, a zatim kliknite **Dodaj**.
2. **Povezivanje izvor** – odaberite kako želite povezati s poslužiteljskom okruženju za prikupljanje podataka:
    - Povezivanje Windows Server ni klijent instalacijom agent.
    - Povežite Linux poslužitelje s OMS Agent za Linux.
    - Koristite račun Azure prostor za pohranu koji je konfiguriran s Windows ili Linux Azure dijagnostičkih VM nastavkom.
    - Pomoću sustava centar za komponentu Operations Manager priložite upravljanje grupe ili cijele uvođenja Operations Manager.
    - Omogućivanje Telemetrijskih Windows da biste koristili nadograditi analize.
        ![povezani izvora](./media/log-analytics-get-started/oms-onboard-data-sources.png)    

3. **Prikupljanje podataka** Konfiguriranje barem jedan izvor podataka za popunjavanje podataka u radni prostor. Kada to učinite, kliknite **Spremi**.    

    ![Prikupljanje podataka](./media/log-analytics-get-started/oms-onboard-logs.png)    


## <a name="optionally-connect-servers-directly-to-the-operations-management-suite-by-installing-an-agent"></a>Po želji se povezati s poslužiteljima izravno u paketu za upravljanje operacije instalacije agent

Sljedeći primjer pokazuje kako se instalira agent za Windows.

1. Kliknite pločicu **Postavke** , kliknite karticu **Povezani izvora** , kliknite karticu za vrstu izvora koji želite dodati, a ili preuzimanje agent ili dodatne informacije o omogućivanju agent. Na primjer, kliknite **Preuzimanje Agent za Windows (64-bitni)**. Za Windows, možete instalirati agent na Windows Server 2008 SP1 ili noviji ili sustavu Windows 7 SP1 ili novijem.
2. Instalirajte agenta na jednom ili više poslužitelja. Možete instalirati agenata jednu po jednu ili više automatiziranog metode pomoću [prilagođene skripte](log-analytics-windows-agents.md)ili možete koristiti postojeće rješenje softver raspodjele koje biste mogli imati.
3. Kada se slažete licencni ugovor, a zatim odaberite instalacijsku mapu, odaberite **Poveži agent za Azure zapisnika analize (OMS)**.   
    ![Instalacijski program agent](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Na sljedećoj stranici vas se za radni prostor ID i ključ radnog prostora. Radni prostor ID i tipku prikazuju se na zaslonu koju ste preuzeli datoteka agent.  
    ![Agent za tipke](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  

    ![Prilaganje poslužitelja](./media/log-analytics-get-started/oms-onboard-key.png)
5. Tijekom instalacije, kliknite **Dodatno** da biste po izboru možete postaviti na proxy poslužitelju i unijeti podatke za provjeru autentičnosti. Kliknite gumb **Dalje** da biste se vratili na zaslon podatke radnog prostora.
6. Kliknite **Dalje** da biste provjerili valjanost radni prostor ID i ključa. Ako se pronađe pogreške, možete kliknuti **ponovno** ispravci. Kada se potvrđuju radni prostor ID i ključ, kliknite da biste dovršili instalaciju agent **instalirati** .
7. Na upravljačkoj ploči kliknite Microsoft Agent nadzor > kartica Azure prijava analitiku (OMS). Zelena kvačica ikona prikazat će se na agente komunicirati s uslugom paket za upravljanje operacije. Na početku, traje otprilike 5 10 minuta.

>[AZURE.NOTE] Kapacitet upravljanja i konfiguraciji Procjena rješenja trenutno nisu podržani po poslužiteljima povezani izravno u paketu za upravljanje operacije.


Možete povezati i agent sustava centra operacije Upravitelj 2012 SP1 ili noviji. Da biste to učinili, odaberite **Poveži agent za sustav centar za komponentu Operations Manager**. Kada odaberete koje mogućnosti šaljete podataka sa servisom bez dodatni ili učitavanje na upravljanje grupe.

Dodatne informacije o povezivanju agente u paketu za upravljanje operacije na [računalima povezivanje Windows zapisnika analize](log-analytics-windows-agents.md).

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>Po želji povezivanje poslužitelja koja koristi sustav centar za komponentu Operations Manager

1. Na konzoli za komponentu Operations Manager odaberite **Administracija**.
2. Proširite čvor **Radu uvide** , a zatim odaberite **Radu uvida vezu**.

  >[AZURE.NOTE] Ovisno o tome što ažuriranje skupne vrijednosti od SCOM koristite, možda ćete vidjeti čvor za *Savjetnik za centar sustava*, *Radu uvida*ili *Operacije upravljanja paket*.

3. Kliknite vezu **registrirati radu uvid u** pri u gornjem desnom kutu, a zatim slijedite upute.
4. Nakon dovršetka postupka čarobnjaka za registraciju, kliknite vezu **Dodaj računala/grupu** .
5. U dijaloškom okviru za **Pretraživanje na računalu** možete potražiti računala ili grupe nadzire Operations Manager. Odabir računala ili grupe da biste onboard ih analize zapisnika, kliknite **Dodaj**, a zatim **u redu**. Možete provjeriti da OMS usluga prima podatke tako da odete na pločicu za **Korištenje** na portalu paket za upravljanje operacije. Podaci se trebale prikazati u otprilike 5 10 minuta.

Dodatne informacije o povezivanju Operations Manager u paketu za upravljanje operacije na [Povezivanje prijava analitiku u komponente Operations Manager](log-analytics-om-agents.md).

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>Po želji analize podataka servisa u oblaku u Microsoft Azure

Upravljanje paket operacije omogućuje brzo traženje događaja i IIS zapisnika za servise u oblaku i virtualnim strojevima omogućivanjem Dijagnostika za servise u Oblaku Azure. Možete primati i dodatne uvida za Azure virtualnim strojevima instalacijom Microsoft Agent za nadzor. Dodatne informacije o konfiguriranju okruženja Azure da biste koristili paket za upravljanje operacije na [Povezivanje Azure spremište zapisnika analize](log-analytics-azure-storage.md).


## <a name="next-steps"></a>Daljnji koraci

- [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md) za dodavanje funkcionalnosti i njihovo prikupljanje podataka.
- Upoznavanje s [zapisnika pretraživanja](log-analytics-log-searches.md) da biste pogledali detaljne podatke prikupljene putem rješenja.
- Da biste spremili i prikazivanje prilagođene pretraživanja pomoću [nadzornih ploča](log-analytics-dashboards.md) .
