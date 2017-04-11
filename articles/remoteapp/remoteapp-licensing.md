<properties
    pageTitle="Azure licenciranje RemoteApp | Microsoft Azure"
    description="Saznajte kako licenciranje Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Licenciranje rad s RemoteApp Azure?

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Da bi ste postavili na servisu Azure RemoteApp svoje predloške koje su stvorili i spremni ste za objavljivanje aplikacija korisnicima. No i dalje postoji jedan preostaje utvrđivanje: licenciranje. Samo kako licenciranja rad za RemoteApp i aplikacije zajedničkog korištenja putem RemoteApp?

RemoteApp ne zahtijeva Windows licenci ni CAL udaljene radne površine. Vaša pretplata brine strane RemoteApp sam. (Pogledajte detalje o [cijenama tarife](https://azure.microsoft.com/pricing/details/remoteapp).)

Ako koristite neku od slika uvrštene u svoju pretplatu, možete bilo koju od aplikacija instalirana na toj slici bez potrebe za posebna licenca zajedničko korištenje. Na primjer, ako pomoću predloška sliku Windows Server 2012 R2 stvaranja zbirke, možete zajednički koristiti Zaštita na krajnjoj točki centar sustava s korisnicima. Samo iznimke to pravilo su Office 365 ProPlus, koji zahtijeva posebnu pretplatu, i Office 2013, koji nije moguće zajednički koristiti u zbirci proizvodnje.

Ako želite da biste koristili sliku predložak za Office 365 koji se isporučuju s RemoteApp, morate imati *postojeću* tarifu za Office 365 ProPlus. Isto vrijedi i za bilo koju aplikaciju sustava Office 365 koju ste objavili pomoću prilagođeni predložak. Potrebno je aktivirati aplikacije s pretplatom na vlastitu. To vrijedi za probno razdoblje i Plaćene pretplate. Ako želite koristiti predložak slike za Office 365 tijekom probnog razdoblja, *a još nemate pretplatu*, otvorite stranicu u Office 365 za [registraciju](https://go.microsoft.com/fwlink/p/?LinkID=403802) za probnu pretplatu. U odjeljku [kako RemoteApp i Office funkcioniraju zajedno?](remoteapp-o365.md) dodatne informacije.

Ako tijekom probnog razdoblja, ne želite da se probnu pretplatu na Office 365, koristite Office 2013 Professional Plus predložak sliku koja se isporučuje s RemoteApp. Na ovoj se slici predloška može se koristiti samo 30 dana i nije moguće pretvoriti u plaćenu zbirke.

Za ostale aplikacije morate Provjerite imate li licencu za zajedničko korištenje aplikacije.

Koji vam odgovara, desno? Možete objaviti bilo koju aplikaciju koju ste legalno pravo na zajedničko korištenje. I trebali biste provjerite je li vam uistinu imaju pravo na zajedničko korištenje programa.

Napominjemo da ne možete koristiti Pokrećite ili količinskog licenciranja ugovor u oblak zbirke. *Možete* koristiti količinskog licenciranja ugovor za aktivaciju aplikacije u sklopu zbirke hibridnog (osim u aplikacijama sustava Office). Samo morate ih instalirati na predlošku sliku s medija količinskog licenciranja. Praćenje informacija iz dobavljaču programa da biste instalirali licenci u okruženju udaljene radne površine.
