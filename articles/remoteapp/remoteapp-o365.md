
<properties
    pageTitle="Korištenje sustava Office s Azure RemoteApp | Microsoft Azure" 
    description="Saznajte kako Office i Azure RemoteApp funkcioniraju zajedno"
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

# <a name="using-office-with-azure-remoteapp"></a>Korištenje sustava Office s Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Imate dvije mogućnosti za smještanje aplikacije sustava Office u Azure RemoteApp: Office 365 ProPlus ili Office 2013 Professional Plus probnu verziju.

**Hej, jeste li znali imamo novi, bolje članak koji će uskoro zamijeniti ovo? Pogledajte [kako koristiti pretplatu na Office 365 s Azure RemoteApp](remoteapp-officesubscription.md). Pokriva sve informacije potrebne za korištenje sustava Office 365 + Azure RemoteApp.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
Stvorite zbirku RemoteApp pomoću sustava Office 365 ProPlus slika predložak. Ta mogućnost omogućuje da biste proširili na servisu Office 365 da biste RemoteApp. Mora imati postojećeg plana pretplate i vaši korisnici morate licenciran za Office 365 ProPlus uslugu, ili samostalne ili putem sustava Office 365 servisa tarife.

RemoteApp podržava zajedničko korištenje aktivaciji računalo programa sustava Office 365. Kada omogućite zajedničko korištenje aktivaciju računalo, a pomoću [alata za implementaciju sustava Office](http://www.microsoft.com/download/details.aspx?id=36778) za instalaciju sustava Office 365 ProPlus instalira bez koji se aktivira. Kada korisnik znak u zbirci web koji sadrži Office 365, Office provjerava ima li korisnik dodijeljeni resursi za Office 365 ProPlus. Ako je tako, Office privremeno aktivira Office 365 ProPlus - ova aktivaciju nastavi do tog korisnika znakove iz servisa.

Da biste koristili zajedničke aktivaciji računalo programa sustava Office 365, morate stvoriti [prilagođeni predložak](remoteapp-create-custom-image.md) i instalacija sustava Office 365 ProPlus postoji, slijedite [ove upute](https://technet.microsoft.com/library/dn782858.aspx).

Možete upravljati korisničkim licence za Office 365 na [Portalu za administratore sustava Office 365](https://portal.office365.com/). Pročitajte dodatne informacije o [tarifama servisa Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx).  


## <a name="office-2013-professional-plus-trial"></a>Office 2013 Professional Plus probne verzije
Tijekom 30-dnevnu probnu verziju sustava RemoteApp, možete koristiti slike sustava Office 2013 Professional Plus (probna verzija) predloška za stvaranje zbirke RemoteApp. Korisnicima možete dodijeliti zbirku probne pomoću svoje račune rad Azure Active Directory ili Microsoftova računa. Potreban je bez dodatnih pretplate.

Ovo je izvrstan za pokretanje sustava gume i dobro osjećaj za Office RemoteApp. Međutim, ta je mogućnost svrhu procjene i testiranja samo. RemoteApp zbirke stvoren pomoću sustava Office 2013 Professional Plus (probna verzija) predložak slike ne prebačena u način rada radnog i će biti onemogućena na kraju probno razdoblje.

## <a name="switching-from-trial-to-production"></a>Prijelaz s probnu verziju na proizvodnje
Kada počnete besplatnu probnu verziju 30 dana, bilješke u odjeljku RemoteApp portala će vas obavještava koliko ste otišli u probnog razdoblja prije prijelaza s računom plaćenu. Možete aktivirati svoj korisnički račun i prijeći u način rada radnog pomoću veze u ovoj bilješci.

Kada aktivirate svoj račun, to će utjecati na sve zbirke RemoteApp na vašem računu.

- Zbirke sa sustavom Windows Server 2012 R2 ili Office 365 ProPlus slika predložaka će Gladak prijelaz u proizvodnje. Sve korisničke podatke i postavke, uključujući tijeku sesije ostaju ostaje netaknuta.
- Ako ste prenijeli slike prilagođeni predložak, zbirke pomoću tih slika će i Gladak prijelaz.
- Slika sustava Office 2013 Professional Plus (probna verzija) predložak namijenjen samo procjenu. Zbirke raditi s na ovoj se slici predložak ne može biti prebačena na radnog. Će se staviti u stanju "onemogućeno".


Ako se ne prijelaz u načinu radnog po isteka probne verzije sustava, zbirke RemoteApp će biti onemogućena. Ne brinite – vaše postavke i podaci za korisnika se spremaju za drugo 90 dana tako da se i dalje možete aktivirati uslugu i prijeći u način rada radnog bez gubitka podataka.
