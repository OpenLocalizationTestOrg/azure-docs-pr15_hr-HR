<properties 
    pageTitle="Što je Azure RemoteApp? | Microsoft Azure" 
    description="Saznajte kako omogućiti zajedničko korištenje aplikacija i resursa za bilo kojeg uređaja pomoću servisa Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="what-is-azure-remoteapp"></a>Što je Azure RemoteApp?

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp poziva funkcija programa Microsoft RemoteApp lokalnog sigurnosno putem udaljene radne površine, za Azure. Azure RemoteApp pomaže vam omogućuju pristup sigurne, udaljene aplikacije s brojnim uređajima drugog korisnika. Azure RemoteApp zapravo hostira koje nisu stalni Terminal Server sesije u oblaku, a možete pristupiti pomoću njih i njihovo zajedničko korištenje s korisnicima.

Azure RemoteApp biste omogućili zajedničko korištenje aplikacija i resursa s korisnicima na gotovo bilo kojeg uređaja. Ne možemo hostira aplikacije u oblaku, što znači da ne možemo voditi brigu o hardver i mjerilo da bi odgovarao zahtjeva za korisnika. Sve što morate učiniti jest prijenos aplikacije u kojima želite zajednički koristiti i zatražite da korisnici upotrebljavaju tim aplikacijama. [Korisnici dobiti želite zadržati svoje uređaje](remoteapp-clients.md), dok sve putem portala za Azure upravljanje. Čak i imate mogućnost korištenja vjerodajnica službene vam osigurava aplikacije i podataka.

Dodatne informacije o Azure RemoteApp čitati na ili, ako ne možemo ste već convinced vas, [Isprobajte ga odmah](https://azure.microsoft.com/services/remoteapp/).

Imate pitanja o Azure RemoteApp? Pogledajte naš [Najčešća Pitanja](remoteapp-faq.md).

Azure RemoteApp je dio sustava [Microsoft virtualne radne površine infrastrukture](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Novi!** Želite li saznati više o Azure RemoteApp? Ili nije spreman za provjeru valjanosti Azure RemoteApp na razini? Uključivanje naš tjedno [stručnjacima u web-seminar](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure zbirke RemoteApp
Postoje dvije vrste zbirki [Azure RemoteApp](remoteapp-collections.md):


- **Oblak zbirke** nalazi se u i sprema podatke za programe u oblaku. Korisnici mogu pristupiti aplikacijama po prijave pomoću njihovih Microsoftova računa ili tvrtke vjerodajnice sinkronizirati ili povezani Azure Active Directory.

    Oblak zbirke kada odaberite aplikaciju koju želite zajednički koristiti ne zahtijeva vezu s bilo kojeg resursa privatne mreže vaše tvrtke (na primjer, pomoću uređaja VPN-a). Ako program koristi resursa na Internetu, OneDrive ili Azure, oblaka zbirke funkcionirat će umjesto vas. Ujedno najbrže da biste stvorili.

- **Hibridno zbirke** nalazi se u i sprema podatke u Azure oblaka, ali i omogućuje korisnicima pristup podacima i resurse pohranjene u lokalnoj mreži. Korisnici mogu pristupiti aplikacijama po prijave pomoću vjerodajnica službene sinkronizirati ili povezani Azure Active Directory.

    Odaberite zbirku hibridnog ako odredite obavezno veze na resurse na privatne mreže vaše tvrtke. Na primjer, ako aplikacija treba pristup nešto od sljedećeg:

    - Poslužitelji datoteke koja se nalazi na intranetu
    - Ubrzat ćete
    - Baza podataka iza vatrozida

    To je obično korisnijim za velike tvrtke s mnogo resursa na svojim privatnim mrežama koje nije moguće premjestiti u oblak.

Različite skupove imaju različite mogućnosti, uključujući mrežama, tako da vam najbolje odgovara slici odgovor [koji zbirke](remoteapp-collections.md) . 


### <a name="updating-your-collection"></a>Ažuriranje vaše zbirke
Jedna od ključne razlike između zbirki hibridnog i oblaka je kako se upravlja softverska ažuriranja. Sa zbirkom oblaka koja koristi predinstaliranim slika sustava Office 365 ProPlus ili Office 2013, ne morate brinuti o sva ažuriranja. Servis održava sam i kumulira out ažuriranja pridonositi, aplikacije i operacijski sustav.

Za hibridno zbirke, kao i oblaka zbirke sliku za prilagođeni predložak, ste zaduženi za održavanje slika te aplikacije. Za domenu pridruženo slike možete kontrolirati ažuriranja pomoću alata kao što je Windows Update, pravilnik grupe ili centar sustava.

Nakon što ažurirate sliku prilagođeni predložak, Prenesite novu sliku u oblak Azure i ažuriranje da biste koristili novu sliku. (To možete učiniti na stranici Azure RemoteApp **Brzi Start** ili na nadzornoj ploči.)

Dodatne informacije potražite [Ažuriranje vaše zbirke](remoteapp-update.md) .

## <a name="supported-remoteapp-clients"></a>Podržani klijenti RemoteApp
Azure RemoteApp podržana za RemoteApp klijentskim aplikacijama za Windows i Windows RT, kao i aplikacije Microsoft udaljene radne površine za Mac, iOS i Android. Vaši korisnici možete koristiti te aplikacije na svoje mobilne ili izračunati uređaje pristup novim programima Azure RemoteApp.

U odjeljku [pristup aplikacijama RemoteApp Azure](remoteapp-clients.md) dodatne informacije o klijente.

## <a name="next-steps"></a>Daljnji koraci
Otvorite! Isprobajte! Ovih članaka pomoći za početak rada u Azure RemoteApp:

- [Kakvu vrstu zbirke potrebne za Azure RemoteApp?](remoteapp-collections.md)
- [Stvaranje slike Azure RemoteApp](remoteapp-imageoptions.md)
- [Kako stvoriti oblaka skup Azure RemoteApp](remoteapp-create-cloud-deployment.md)
- [Kako stvoriti hibridnog skup Azure RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Licenciranje rad s RemoteApp Azure?](remoteapp-licensing.md)
- [Praktični savjeti za korištenje Azure RemoteApp](remoteapp-bestpractices.md)
- [Najčešća pitanja vezana uz Azure RemoteApp](remoteapp-faq.md)
 

### <a name="help-us-help-you"></a>Pomozite nam da vam pomoći 
Jeste li znali da osim ocjena u ovom se članku i upućivanje komentare dolje ispod, koje možete unijeti promjene sam članak? Nešto nedostaje? Nešto nije u redu? Nije li moguće napisati nešto što je samo pregledniji? Pomicanje prema gore, a zatim kliknite **Uređivanje na GitHub** ili **Uređivanje** da biste promijenili – one će dođite do nas za pregled, a zatim jednom smo odjaviti na njima, vidjet ćete promjene i poboljšanja ovdje.