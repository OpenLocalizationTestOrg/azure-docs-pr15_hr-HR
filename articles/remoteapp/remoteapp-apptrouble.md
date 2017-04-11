<properties 
    pageTitle="Azure RemoteApp otklanjanje poteškoća – pokretanje i veze pogrešaka aplikacije | Microsoft Azure" 
    description="Saznajte kako otkloniti poteškoće s početnim i povezivanje s aplikacijama u Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



#<a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Otklanjanje poteškoća s Azure RemoteApp – pokretanje i veze pogrešaka aplikacije 

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Aplikacije koje se nalaze u Azure RemoteApp uspijeva da biste pokrenuli nekoliko razloga. U ovom se članku opisuju razloge i poruke o pogreškama koje korisnici mogu pojaviti kada pokušate pokrenuti aplikacije. Također se govori o neuspjeha veze. (Ali u ovom se članku opisuju problemi prilikom prijave klijentskog programa Azure RemoteApp.)  

Nastavite čitati informacije o česte poruke o pogreškama zbog pogreške aplikacije pokretanje i veze.

##<a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Ne možemo primate prilikom postavljanja... Pokušajte ponovno 10 minuta.

Ova pogreška znači Azure RemoteApp Skaliranje da bi odgovarao kapacitetom korisnika. U pozadini više instanci Azure RemoteApp VM stvaraju se rukovati kapaciteta potrebama korisnika. Obično to traje oko pet minuta, ali može potrajati do 10 minuta. Ponekad to se ne događa dovoljno brzo te resurse potrebno odmah. Na primjer 9 AM scenarij u kojem mnogi korisnici trebate koristiti aplikacije Azure RemoteAppn u isto vrijeme. Ako se to dogodi vam možemo možete omogućiti **način kapaciteta** na pozadinska. Da biste to učinili u zahtjev za podršku za Azure možete otvoriti i ili e-pošte nam se na [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com). Biti sigurni da biste obuhvatili identifikacijskog Broja za pretplatu na zahtjev.  

![Ne možemo dobivate prilikom postavljanja](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Neuspjeh automatski-ponovnog spajanja s aplikacijama, ponovno pokrenite aplikaciju  

Ova poruka o pogrešci je često vidjeti ako koristili Azure RemoteApp i staviti računalo u stanje mirovanja dulje od četiri sata, a zatim probudio PC i klijentskog programa Azure RemoteApp pokušaj da se automatski ponovno povezali i vremensko ograničenje je premašena.  Uputite korisnike da otvore u aplikaciji i pokušate otvoriti iz klijentskog programa Azure RemoteApp.

![Neuspjeh automatski-ponovnog spajanja svojim aplikacijama](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>Problemi s temp profila 

Ta se pogreška pojavljuje kada svoj korisnički profil (korisničkog profila Disk) nije uspjela dostupnosti i korisnik prima privremene profila.  Administratori moraju dođite do zbirke na portalu za Azure i zatim idite na karticu **sesije** i pokušajte **Odjava** korisnika. To će prisilno cijelog zapisnika s sesiju korisnika – a zatim je korisnik pokušati ponovno pokrenite aplikaciju. Ako to ne uspije obratite se podršci za Azure i ili e-pošte nam se na [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp ne funkcionira

Poruka o pogrešci znači klijentskog programa Azure RemoteApp uzrokuju problem, a potrebno je ponovno pokrenuti. Uputite korisnika da biste zatvorili: odaberite **zatvorite program** , a zatim ponovno pokrenite klijent Azure RemoteApp.  Ako problem i dalje Otvori i zahtjev za podršku za Azure možete i ili e-pošte nam se na [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

![Azure RemoteApp ne funkcionira](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Remote Desktop Connection je pristup ovaj resurs došlo je do pogreške. Ponovno vezu ili se obratite administratoru sustava

Ovo je u generičkoj poruci o pogrešci – obratite se podršci za Azure i ili e-pošte nam se na [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com) tako da se može se istražiti. 

![Generički Azure RemoteApp](./media/remoteapp-apptrouble/ra-apptrouble4.png) 