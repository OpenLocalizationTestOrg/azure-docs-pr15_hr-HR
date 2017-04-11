<properties
    pageTitle="Korištenje preusmjeravanje u Azure RemoteApp | Microsoft Azure"
    description="Upute za konfiguriranje i korištenje preusmjeravanje RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-redirection-in-azure-remoteapp"></a>Korištenje preusmjeravanje u Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Preusmjeravanje omogućuje interakciju s udaljenog aplikacije pomoću uređaja povezanih s lokalnog računala, telefona i tableta korisnika. Ako, na primjer, ako je navedena Skype pomoću servisa Azure RemoteApp korisnički mora kamere instalirali na svoje računalo za rad sa servisa Skype. To vrijedi i za pisač, zvučnika, monitora i perifernih uređaja raspon USB povezani.

RemoteApp upravlja Remote Desktop Protocol (RDP) i RemoteFX za preusmjeravanje.

## <a name="what-redirection-is-enabled-by-default"></a>Po zadanom je omogućena koje preusmjeravanje?
Kada koristite RemoteApp, sljedeće preusmjeravanja omogućena je prema zadanim postavkama. Informacije u zagradama pokazuju postavke RDP.

- Reproduciraj zvukove na lokalnom računalu (**za reprodukciju na ovom računalu**). (audiomode:i:0)
- Snimiti zvuk s lokalnog računala te poslati na udaljenom računalu (**zapis s ovog računala**). (audiocapturemode:i:1)
- Ispis lokalne pisače (redirectprinters:i:1)
- COM priključci (redirectcomports:i:1)
- Pametna kartica uređaj (redirectsmartcards:i:1)
- Međuspremnik (mogućnost Kopiranje i lijepljenje) (redirectclipboard:i:1)
- Poništite vrstu fonta izglađivanje (Dopusti Izglađivanje fonta: i:1)
- Preusmjeravanje sve podržane vrste Uključi i radi uređaja. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Dostupna je što preusmjeravanje?
Po zadanom su onemogućeni dvije mogućnosti za preusmjeravanje:

- Preusmjeravanje pogon (pogon mapiranje): lokalno računalo pogona postaju mapirani pogoni u udaljenu sesiju. Na taj način spremanja i otvaranje datoteka iz lokalne pogone dok radite u udaljenu sesiju.
- USB preusmjeravanje: koristite USB uređaja povezanih s vašim lokalnim računalom unutar udaljenu sesiju.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Promjena postavki preusmjeravanja RemoteApp
Možete promijeniti postavke za preusmjeravanje uređaja za zbirku pomoću programa Microsoft Azure PowerShell SDK. Nakon što instalirate novi PowerShell i SDK, najprije konfigurirajte ga tako da biste upravljali pretplatom, kao što je opisano [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

Da biste postavili Prilagođena svojstva RDP koristiti naredbu otprilike ovako:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Imajte na umu te *n* se koristi kao razdjelnika između pojedinačnih svojstva.)

Da biste dobili popis konfigurirani tako što prilagođena svojstva RDP, pokrenite sljedeći cmdlet. Imajte na umu da samo prilagođena svojstva prikazana su kao i izlazni rezultata nije zadana svojstva:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Kada postavite prilagođena svojstva navedite sva prilagođena svojstva svaki put; postavka u suprotnom se vraća je onemogućen.   

### <a name="common-examples"></a>Primjeri uobičajenih
Da biste omogućili preusmjeravanje pogon, koristite sljedeći cmdlet:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

Pomoću tog cmdleta da biste omogućili USB i pogon preusmjeravanje:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Koristite ovaj cmdlet za omogućivanje zajedničkog korištenja u međuspremnik:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] Svakako posve odjavite svi korisnici u zbirci (i ne samo prekinuti) prije testirati promjene. Da bi korisnici potpuno odjavljeni, idite na karticu **sesije** u zbirci na portalu za Azure i odjava sve korisnike koji su povezani ili prijavljeni. Ponekad može potrajati nekoliko sekundi lokalnog pogona koje se prikazuju u programu Explorer sesiju.

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Promjena postavki za preusmjeravanje USB na vaš klijent za Windows

Ako želite koristiti USB preusmjeravanje na računalima na kojima se povezuje s RemoteApp postoje 2 akcije koje je potrebno da se dogodi. 1 – administrator vam mora odobriti USB preusmjeravanje na razini zbirke pomoću Azure PowerShell. 2 – na svaki mjesto na koje želite koristiti preusmjeravanje USB uređaj, morate omogućiti pravilnika grupe koji omogućuje ga. Ovaj korak ćete morati poduzeti za svakog korisnika koji želi koristiti USB preusmjeravanje.

> [AZURE.NOTE] Preusmjeravanje USB s Azure RemoteApp podržano je samo za računala sa sustavom Windows.

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>Omogućivanje USB preusmjeravanje za zbirku RemoteApp
Da biste omogućili USB preusmjeravanje na razini zbirke, koristite sljedeći cmdlet:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>Omogućivanje USB preusmjeravanje za klijentsko računalo

Konfiguriranje postavki za preusmjeravanje USB na vašem računalu:

1. Otvorite uređivač pravila lokalne grupe (GPEDIT. MSC). (Pokrenuti gpedit.msc iz naredbenog retka.)
2. Otvorite **računala Configuration\Policies\Administrative Templates\Windows Components\Remote radne površine Services\Remote radnom površinom Client\RemoteFX USB uređaj preusmjeravanje**.
3. Dvokliknite **Dopusti RDP preusmjeravanje druge podržanih RemoteFX USB uređaja s ovog računala**.
4. Odaberite **omogućeno**, a zatim odaberite **administratorima i korisnicima u prava pristupa za preusmjeravanje RemoteFX USB**.
5. Otvorite naredbeni redak s administratorskim ovlastima i pokrenite sljedeću naredbu:

        gpupdate /force
6. Ponovno pokrenite računalo.

Alat za upravljanje pravilnikom grupe možete koristiti za stvaranje i Primjena pravila preusmjeravanja USB na sva računala u vašoj domeni:

1. Prijavite se u kontrolerom domene kao administrator domene.
2. Otvorite konzoli za upravljanje pravilnikom grupe. (Kliknite **Start > Administrativni alati > Upravljanje pravilima grupe**.)
3. Dođite do domene ili organizacijsku jedinicu kojoj želite stvoriti pravila.
4. Desnom tipkom miša kliknite **Zadana pravila domene**, a zatim kliknite **Uredi**.
5. Otvorite **računala Configuration\Policies\Administrative Templates\Windows Components\Remote radne površine Services\Remote Desktop Connection Client\RemoteFX USB uređaj preusmjeravanje**.
6. Dvokliknite **Dopusti RDP preusmjeravanje druge podržanih RemoteFX USB uređaja s ovog računala**.
7. Odaberite **omogućeno**, a zatim odaberite **administratorima i korisnicima u prava pristupa za preusmjeravanje RemoteFX USB**.
8. Kliknite **u redu**.  
