<properties
   pageTitle="Pokretanje aplikacija u sustavu Windows na bilo kojem uređaju s Azure RemoteApp | Microsoft Azure"
   description="Saznajte kako zajednički koristiti bilo koju aplikaciju sustava Windows s korisnicima Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Pokretanje aplikacija u sustavu Windows na bilo kojem uređaju s Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Možete pokrenuti aplikaciju Windows bilo gdje na bilo kojem uređaju odmah, utjecati – samo pomoću Azure RemoteApp. Hoće li se prilagođenu aplikaciju napisali prije 10 godina ili aplikacije sustava Office, korisnici više ne trebate uz je određeni operacijski sustav (kao što je Windows XP) za te nekoliko aplikacije.

S Azure RemoteApp vaši korisnici možete koristiti svoje uređaje sa sustavom Android ili Apple i dobit isti sučelje oni dobivenog u sustavu Windows (ili na uređaje Windows phone). To se postiže tako da hosting aplikaciju sustava Windows u skup virtualnim računalima sustava Windows na Azure - ih korisnici mogu pristupati s bilo kojeg mjesta čije su povezani s Internetom. 

Nastavite čitati primjer točno onako kako ćete to učiniti.

U ovom se članku smo ćete omogućiti zajedničko korištenje programa Access sa svim naših korisnika. Međutim, možete koristiti bilo koju aplikaciju. Pod uvjetom da biste mogli instalirati aplikaciju na računalu sa sustavom Windows Server 2012 R2, koji možete je zajednički koristiti pomoću sljedećih koraka. [Preduvjeti za aplikaciju](remoteapp-appreqs.md) da biste bili sigurni da će funkcionirati aplikacije možete pregledati.

Napominjemo da jer Access je baza podataka, a želimo tu bazu podataka biti koristan, ne možemo će se način nekoliko dodatnih koraka da biste omogućili pristup korisnicima zajedničko korištenje podataka programa Access. Ako aplikacija nije baze podataka ili nije potrebno korisnici moći pristupiti zajedničke datoteke, možete preskočiti te korake ovog praktičnog vodiča

> [AZURE.NOTE] <a name="note"></a>Potreban vam je račun za Azure da biste dovršili ovaj Praktični vodič:
> - Možete je [besplatno otvorite račun za Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): dohvaćanje kredita možete koristiti da biste isprobali plaćenu servisa Azure i čak i kada se koriste najviše možete zadržati račun i korištenje slobodno Azure servisa, kao što su web-mjesta. Vaša kreditna kartica nikad naplatiti, osim ako izričito Promjena postavki i od nje zatražite da se naplatiti.
> - Možete [aktivirati pogodnosti pretplatnika MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): MSDN vaša pretplata vam kredita svakog mjeseca, koje možete koristiti za plaćenu Azure servise.


## <a name="create-a-collection-in-remoteapp"></a>Stvaranje zbirke RemoteApp

Započnite tako da stvorite zbirku. Zbirka služi kao spremnik za aplikacije i korisnicima. Svaku zbirku temelji se na sliku – možete stvoriti vlastitu ili neki koji ste dobili uz pretplatu. Za ovaj vodič smo koristite sliku probno razdoblje sustava Office 2013 – sadrži aplikaciju koju želite zajednički koristiti.

1. Na portalu Azure pomicanje prema dolje u navigacijskom oknu stabla lijevoj strani dok ne ugledate RemoteApp. Otvaranje stranice.
2. Kliknite **Stvori RemoteApp zbirke**.
3. Kliknite **brzo stvaranje** , a zatim unesite naziv za vašu zbirku.
4. Odaberite područje koje želite koristiti za stvaranje zbirke web. Za najbolje mogućnosti rada, odaberite područje koji je najsličniji geografski na mjesto gdje hoće li korisnici pristupati aplikaciju. Ako, na primjer, u ovom ćete praktičnom vodiču, korisnici će se nalaziti u Redmond, Washington. Područje najbliže Azure je **Zapad SAD -a**.
5. Odaberite naplate plan koji želite koristiti. Osnovni plan za naplatu stavlja 16 korisnika na velike VM Azure, dok je standardni tarifu za naplatu ima 10 korisnika na velike VM Azure. Kao primjer s Općenito osnovni plan funkcionira odlično za tijek rada Vrsta unosa podataka. Za produktivnost aplikaciju, kao što je Office trebali standardne plan.
6. Na kraju, odaberite sliku Office 2013 Professional. Na ovoj se slici sadrži aplikacije sustava Office 2013. Samo podsjetnik - Ova slika je samo dobro za probno razdoblje zbirke i POCs. Koje ' ne možete koristiti na ovoj se slici u zbirku proizvodnje.
7. Sada kliknite **Stvaranje RemoteApp zbirke**.

![Stvaranje zbirke oblaka RemoteApp](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Time ćete stvaranja mjesta, ali može potrajati prema gore za jedan sat.

Sada ste spremni dodati korisnike.

## <a name="share-the-app-with-users"></a>Zajedničko korištenje aplikacije s korisnicima

Kada zbirka uspješno je stvorena, vrijeme je da biste objavili pristup korisnicima i dodali korisnike koji imaju pristup ga.

Ako vam se krećete izvan čvor Azure RemoteApp tijekom stvaranja zbirke, počnite tako da odaberete način vratiti na početnoj stranici Azure.

2. Kliknite zbirke koju ste stvorili ranije da biste pristupili dodatnim mogućnostima i konfiguriranje zbirke.
![Novu zbirku oblaka RemoteApp](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. Na kartici **Objavljivanje** kliknite **Objavi** na dnu zaslona, a zatim **objavite pokrenuti programi na izborniku**.
![Objavljivanje programa RemoteApp](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. Odaberite aplikacije koje želite objaviti na popisu. Za naše svrhu smo odabrali programa Access. Kliknite **dovrši**. Pričekajte aplikacije da biste dovršili objavljivanje.
![Objavljivanje pristup RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. Dovršetku aplikaciju objavljivanje, lakši putem **Korisničkog pristupa** karticu da biste dodali korisnike koji je potreban pristup aplikacijama. Unesite imena korisnika (adresu e-pošte) za korisnike, a zatim kliknite **Spremi**.

![Dodavanje korisnika u RemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. Sada je vrijeme za obavještavanje korisnika o tih novih aplikacija i kako im pristupiti. Da biste to učinili, vaše korisnicima poslati poruku e-pošte koja pokazuje na URL za preuzimanje klijent udaljene radne površine.
![Klijenta preuzme URL-a za RemoteApp](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Konfiguriranje programa access u programu Access

Neke se aplikacije potrebna dodatna konfiguracija nakon implementacije kroz RemoteApp. Posebno pristup, ne možemo ćete stvoriti zajedničko korištenje datoteka na Azure koji svi korisnici mogu pristupiti. (Ako ne želite da to učinite, možete stvoriti [zbirke hibridnog](remoteapp-create-hybrid-deployment.md) [umjesto naš zbirke oblaka] koja omogućuje korisnicima pristupiti datotekama i informacije o lokalnoj mreži.) Zatim ćemo morat ćete obavještavanje naših korisnika za mapiranje na lokalni pogon na svom računalu Azure datotečnom sustavu.

Kao administrator obavljate prvi dio. Nakon toga imamo neke korake za korisnike.

1. Pokrenite objavljivanjem naredbenim sučeljem retka (cmd.exe). Na kartici **Objavljivanje** odaberite **cmd**, a zatim kliknite **Objavljivanje > Objavi program pomoću puta**.
2. Unesite naziv aplikacije i put. Za naše svrhu upotrijebili "Eksplorer za datoteke" kao naziv i "% SYSTEMDRIVE%\windows\explorer.exe" put.
![Objavite cmd.exe datoteku.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Sada ćete morati stvoriti Azure [račun za pohranu](../storage/storage-create-storage-account.md). Pod nazivom naše "accessstorage", pa odaberite naziv koji je smislen. (Da biste misquote Highlander može biti samo jedna "accessstorage.") ![Računa za pohranu za naše Azure](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Sada vratite se na nadzornu ploču da biste mogli primati put prostora za pohranu (krajnjoj točki mjesto). Ćete tom se mogućnošću poslužite u malo, pa provjerite negdje kopiranja.
![Put račun za pohranu](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Nakon toga kad stvoren je račun za pohranu, morate pristupni primarni ključ. Kliknite **Upravljanje pristupnih tipki**, a zatim kopirajte pristupni primarni ključ.
6. Sada, postavite kontekstu računa za pohranu i stvaranje nove zajedničke datoteke za Access. U prozoru povećane komponente Windows PowerShell pokrenite sljedeće Cmdlete:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    Da bi se za naše se zajednički koristi, ovo su cmdleta možemo pokrenuti:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


Sada je uključivanje korisnika. Najprije su korisnici instalirajte [RemoteApp klijenta](remoteapp-clients.md). Nakon toga korisnici moraju mapirati pogon svoj račun te Azure za zajedničko korištenje datoteka koju ste stvorili, a zatim dodajte svoje datoteke programa Access. Evo kako ih učiniti:

1. U klijentu RemoteApp pristupiti objavljene aplikacije. Pokrenite cmd.exe program.
2. Pokrenite sljedeću naredbu da biste mapirali pogon s računala i zajedničko korištenje datoteka:

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    Ako postavite parametar **/ trajnog** da mapiranih pogon će i dalje pojavljuje preko sesije.
1. Sada, pokrenite aplikaciju Eksplorer za datoteke s RemoteApp. Kopirajte sve datoteke Access koju želite koristiti u zajedničke aplikacije za zajedničko korištenje datoteka.
![Stavljanje datoteke programa Access u Azure zajedničko korištenje](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. Na kraju, otvorite Access i otvorite bazu podataka koju ste upravo zajednički se koristi. Trebali biste vidjeti podatke u programu Access izvodi iz oblaka.
![Realni baze podataka programa Access izvodi iz oblaka](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Sada možete postaviti pomoću programa Access na bilo kojeg uređaja – samo provjerite je li instalirati RemoteApp klijenta.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

Sad kad ste usvojili stvaranja zbirke, pokušajte stvoriti [zbirke koji koristi Office 365](remoteapp-tutorial-o365anywhere.md). Ili možete stvoriti [zbirke hibridnog ](remoteapp-create-hybrid-deployment.md)koji mogu pristupiti lokalnoj mreži.

<!--Image references-->
 
