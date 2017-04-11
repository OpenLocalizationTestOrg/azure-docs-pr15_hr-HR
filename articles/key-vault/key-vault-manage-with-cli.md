<properties
    pageTitle="Upravljanje ključ sigurnog korištenja EŽA | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča automatizirati uobičajene zadatke u ključ sigurnog pomoću na EŽA"
    services="key-vault"
    documentationCenter=""
    authors="BrucePerlerMS"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="bruceper"/>

# <a name="manage-key-vault-using-cli"></a>Upravljanje ključ sigurnog korištenja EŽA #
Azure ključ sigurnog dostupan je u većini područja. Dodatne informacije potražite u članku [ključ sigurnog cijene stranice](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Uvod  
Pomoću ovog praktičnog vodiča pomoći da započnete s Azure sigurnog ključ za stvaranje kontejnera hardened (sigurnog) u Azure za pohranu i upravljanje šifriranja tipke i tajne u Azure. Ga vodit će vas kroz postupak korištenja sučelja naredbenog retka za različite platforme Azure da biste stvorili sigurnog koja sadrži ključ ili lozinke koje možete koristiti s Azure aplikacije. Zatim pokazuje kako aplikaciju možete koristiti taj ključ ili lozinku.

**Procijenjena vrijeme da biste dovršili:** 20 minuta

>[AZURE.NOTE]  Pomoću ovog praktičnog vodiča ne uključuje upute za pisanje Azure aplikacije da jedan od koraka obuhvaća, koji pokazuje kako da biste autorizirali aplikaciju pomoću tipke ili tajna u ključa zbirke ključeva.
>
>Trenutno ne možete konfigurirati Azure ključ sigurnog na portalu za Azure. Umjesto toga koristite ove upute za različite platforme sučelje naredbenog retka. Ili Azure PowerShell upute potražite u članku [ovom ćete praktičnom vodiču ekvivalentan](key-vault-get-started.md).

Pregled informacija o sigurnog ključ Azure, potražite u članku [što je sigurnog ključ Azure?](key-vault-whatis.md)

## <a name="prerequisites"></a>Preduvjeti
Da biste dovršili ovaj Praktični vodič, morate imati sljedeće:

- Pretplate na Microsoft Azure. Ako ne postoji, možete se prijaviti za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial).
- Sučelje naredbenog retka verziju 0.9.1 ili noviji. Da biste instalirali najnoviju verziju i povezati Azure pretplatu, potražite u članku [Instaliranje i konfiguriranje sučelje naredbenog retka za različite platforme Azure](../xplat-cli-install.md).
- Aplikacija koja će biti konfigurirana tako da pomoću tipke ili lozinke koje ste stvorili pomoću ovog praktičnog vodiča. Primjer aplikacije dostupna iz [Microsoftova centra za preuzimanje](http://www.microsoft.com/download/details.aspx?id=45343). Upute potražite u članku popratnu datoteke Readme.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Pristup pomoći s sučelje naredbenog retka različite platforme Azure

Pomoću ovog praktičnog vodiča pretpostavlja da ste upoznati sa sučeljem naredbenog retka (tulumu, Terminal, naredbeni redak)

Na – pomoć ili -h parametar može se koristiti za prikaz pomoći za određenu naredbe. Umjesto toga azure pomoć [naredba] [mogućnosti] oblik može se koristiti za vraćanje iste podatke. Na primjer, sljedeće naredbe sve vraćaju iste podatke:

    azure account set --help

    azure account set -h

    azure help account set

Kada se nalazite u jasno o parametrima potrebne za naredbu odnose radi korištenja – pomoć, -h ili azure pomoć [naredba].

Možete pročitati i sljedeći vodiči za upoznati s Azure Voditelj resursa u sučelje naredbenog retka različite platforme Azure:

- [Kako instalirati i konfigurirati Azure različite platforme naredbeni redak sučelja](../xplat-cli-install.md)
- [Korištenje Azure sučelje naredbenog retka za različite platforme s Azure Voditelj resursa](../xplat-cli-azure-resource-manager.md)


## <a name="connect-to-your-subscriptions"></a>Povezivanje s pretplate

Da biste se prijavili pomoću računa tvrtke ili ustanove, koristite sljedeću naredbu:

    azure login -u username -p password

ili ako želite da se prijavite tako da upišete interaktivno

    azure login

>[AZURE.NOTE]  Prijava metoda funkcionira samo s računa tvrtke ili ustanove. Račun tvrtke ili ustanove je korisnik koja se upravlja administrator tvrtke ili ustanove i definirano u vašoj tvrtki ili ustanovi Azure Active Directory klijentu.


Ako trenutno nemate račun tvrtke ili ustanove, a se pomoću Microsoftova računa za prijavu u pretplatu za Azure, pomoću sljedećih koraka možete jednostavno stvoriti.

1.  Prijavite se na stranici Prijava na [Portal za upravljanje Azure](https://manage.windowsazure.com/)i klikom na servisu Active Directory.
2.  Ako nema direktorijem, odaberite Stvori direktorija i pružili potrebne informacije.
3.  Odaberite direktorija i dodavanje novog korisnika. Novi korisnik je račun tvrtke ili ustanove. Tijekom stvaranja korisnika će biti navedena s obje adresu e-pošte za korisnika i privremenu lozinku. Spremite te informacije kako se koristi u drugom koraku.
4.  S portala sustava odaberite postavke, a zatim uključite administratori. Odaberite Dodaj pa Dodaj novog korisnika kao suradnika administrator. Time se omogućuje račun tvrtke ili ustanove da biste upravljali pretplatom Azure.
5.  Na kraju, odjavite se iz aplikacije portala za Azure, a zatim se prijavite u pomoću novog računa tvrtke ili ustanove. Ako je ovo prvi put prijave pomoću tog računa, morat ćete promijeniti lozinku.

Dodatne informacije o korištenju račun tvrtke ili ustanove s Microsoft Azure potražite u članku [registrirati za Microsoft Azure kao tvrtkom ili ustanovom](../active-directory/sign-up-organization.md).

Ako imate više pretplata i želite da navedete jednu određenu za Azure ključ sigurnog, upišite sljedeće da biste vidjeli popis pretplata za vaš račun:

    azure account list

Zatim, da biste odredili pretplate da biste koristili, upišite:

    azure account set <subscription name>

Dodatne informacije o konfiguriranju sučelje naredbenog retka za različite platforme Azure potražite [u](../xplat-cli-install.md)članku instalacija i konfiguriranje Azure različite platforme sučelje za naredbenog retka.


## <a name="switch-to-using-azure-resource-manager"></a>Prijelaz na korištenje Voditelj resursa za Azure

Ključ sigurnog zahtijeva Azure Voditelj resursa, pa upišite sljedeću naredbu da biste prešli u način Azure Voditelj resursa:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Stvorite novu grupu resursa

Kada koristite upravitelj resursa Azure, sve povezane resurse stvaraju se u grupu resursa. Ne možemo će stvoriti novu grupu resursa 'ContosoResourceGroup' za ovog praktičnog vodiča.

    azure group create 'ContosoResourceGroup' 'East Asia'

Prvi parametar je naziv grupe resursa, a drugi parametar "mjesto". Mjesto, koristite naredbu `azure location list` da biste odredili kako odrediti zamjenski mjesto onome u ovom primjeru. Ako trebate dodatne informacije, upišite:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Registrirajte se davatelja resursa sigurnog ključ
Provjerite je li taj davatelj resursa ključ sigurnog registriran u svoju pretplatu:

`azure provider register Microsoft.KeyVault`

To samo je potrebno učiniti jednom po pretplati.


## <a name="create-a-key-vault"></a>Stvaranje ključa zbirke ključeva

Korištenje na `azure keyvault create` naredbu da biste stvorili ključa zbirke ključeva. Ovo pismo ima tri obavezno parametara: naziv grupe resursa, naziv ključa sigurnog i zemljopisnu lokaciju.

Na primjer, ako koristite naziv sigurnog ContosoKeyVault, naziv grupe resursa za ContosoResourceGroup i mjesto istočnoazijski, upišite:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

Izlaz iz ta naredba prikazuje svojstva zbirke ključeva ključa koji ste upravo stvorili. Dva najvažnija svojstva su:

- **Naziv**: U ovom primjeru to je ContosoKeyVault. Koristit ćete naziv cmdleti za drugi ključ sigurnog.
- **vaultUri**: U ovom primjeru to je https://contosokeyvault.vault.azure.net. Aplikacije koje koriste vaš sigurnog do njegova REST API-JA morate koristiti ovaj URI.

Račun za Azure sada ovlašteni za izvođenje operacije na ovom ključa sigurnog. Što još nitko drugi je.


## <a name="add-a-key-or-secret-to-the-key-vault"></a>Dodajte ključ ili tajna ključa zbirke ključeva

Ako želite Azure sigurnog ključ za stvaranje ključa zaštićenima softver za vas, koristite na `azure key create` naredbu i upišite sljedeće:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Međutim, ako imate postojeći ključ u datoteci .pem spremljen kao lokalne datoteke u datoteku pod nazivom softkey.pem koje želite prenijeti sigurnog ključ Azure, upišite sljedeću naredbu da biste uvezli ključ iz sustava. Datoteka PEM koji štiti tipku softverom servisa sigurnog ključ:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Sada možete referencirati ključ koji ste stvorili ili prenijeli sigurnog ključ Azure, pomoću njegov URI. Pomoću **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** uvijek preuzeli trenutnu verziju, a **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** da biste pristupili slijedite ovaj određenu verziju.

Da biste dodali na tajna sigurnog, koji se naziva SQLPassword lozinku, a koji ima vrijednost od stranicu$ $w0rd za Azure ključ sigurnog, upišite sljedeće:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Sada možete referencirati ovu lozinku koju ste dodali sigurnog ključ Azure, pomoću njegov URI. Pomoću **https://ContosoVault.vault.azure.net/secrets/SQLPassword** uvijek preuzeli trenutnu verziju, a **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** da biste pristupili slijedite ovaj određenu verziju.

Pogledajmo prikaz ključ ili tajna koju ste upravo stvorili:

- Da biste vidjeli svoj ključ, upišite:`azure keyvault key list --vault-name 'ContosoKeyVault'`
- Da biste pogledali svoje tajna, upišite:`azure keyvault secret list --vault-name 'ContosoKeyVault'`


## <a name="register-an-application-with-azure-active-directory"></a>Registracija aplikacije pomoću servisa Azure Active Directory

Ovaj korak obično želite napraviti razvojni inženjer na odvojenom računalu. Nije specifična za Azure ključ sigurnog, ali se, za dovršenosti.


>[AZURE.IMPORTANT] Da biste dovršili vodič, vaš račun, na sigurnog i aplikaciju koja će registrirati u ovom ćete koraku svi moraju biti u istoj Azure direktoriju.

Aplikacije koje koriste ključne sigurnog morate provjeriti autentičnost pomoću tokena iz servisa Azure Active Directory. Da biste to učinili, vlasnik aplikacije morate registrirati aplikacije u svoje Azure Active Directory. Na kraju Registracija aplikacije vlasnik dobiva sljedeće vrijednosti:


- **ID aplikacije** (poznat i kao ID klijenta) i **provjeru autentičnosti** (poznat i kao dijeljena tajna). Aplikacija morate prezentirati od te vrijednosti Azure Active Directory, da biste dobili token. Kako će se aplikacija konfiguriran za to ovisi o aplikaciji. Vlasnik aplikacije za uzorak aplikaciju sigurnog ključ postavlja te vrijednosti u datoteci app.config.



Da biste registrirali aplikaciju servisa Azure Active Directory:

1. Prijavite se na portal za Azure.
2. Na lijevoj strani kliknite **Servisa Active Directory**, a zatim direktoriju u kojem će registrirati svoje aplikacije. <br> <br> Napomena: Morate odabrati isti direktorij koji sadrži Azure pretplate koji ste stvorili vaše ključa sigurnog. Ako ne znate koje direktorija to je, kliknite **Postavke**, prepoznavanje pretplate koji ste stvorili vaše ključa sigurnog, a zatim zapišite naziv direktorija koji se prikazuje u zadnjem stupcu.

3. Kliknite **APLIKACIJE**. Ako nema aplikacije dodani direktorija, ova stranica će se prikazati samo veza **Dodaj aplikaciju** . Kliknite vezu ili ili, možete kliknuti **DODAJ** na naredbenoj traci.
4.  U čarobnjaku za **Dodavanje APLIKACIJE** na na **što želite učiniti?** stranice, kliknite **Dodaj aplikaciju je moje tvrtke ili ustanove razvoju**.
5.  Na stranici **Recite nam o aplikaciji** Navedite naziv aplikacije, a zatim odaberite **API WEB AND/OR APLIKACIJU za WEB** (zadano). Kliknite ikonu dalje.
6.  Na stranici **Svojstva aplikacije** navedite **URL za prijavu na** i **ID URI Aplikaciju** za web-aplikaciju. Ako aplikacija nije te vrijednosti, možete ih učiniti za ovaj korak (možete, primjerice, odrediti http://test1.contoso.com za oba okvira). Nije važno ako postoji tih web-mjesta; što je važno je aplikaciju ID URI za svaku aplikaciju razlikuje za svaku aplikaciju u direktoriju. Imenik koristi taj niz za označavanje aplikacije.
7.  Kliknite ikonu dovršen da biste spremili promjene u čarobnjaku.
8.  Na stranici za brzo pokretanje kliknite **KONFIGURIRAJ**.
9.  Pomaknite se do odjeljka **tipke** , odaberite trajanje i zatim kliknite **SPREMI**. Stranica osvježava i prikazuje vrijednost ključa. Pomoću ovog ključa i vrijednost **ID KLIJENTA** , morate konfigurirati aplikacije. (Upute za tu konfiguraciju će biti specifičnim aplikacijama.)
10. Kopirajte vrijednost ID klijenta s ove stranice koje ćete koristiti u sljedećem koraku za postavljanje dozvola za vaše zbirke ključeva.




## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autorizirajte aplikaciju koju želite koristiti ključ ili tajna

Da biste autorizirali aplikacije da biste pristupili ključ ili tajna u na sigurnog, poslužite se `azure keyvault set-policy` naredba.

Na primjer, ako se zovete sigurnog ContosoKeyVault i željenu aplikaciju da biste autorizirali sadrži ID klijenta sustava 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, a želite da biste autorizirali aplikacije za šifriranje i prijavite se pomoću tipki u vašem sigurnog, zatim pokrenite sljedeću naredbu:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '["decrypt","sign"]'

>[AZURE.NOTE] Ako su pokrenuti na Windows naredbeni redak, trebali biste jednostrukim navodnicima zamijenite dvostruke navodnike, a i escape Interna dvostrukih navodnika. Na primjer: "[\"dešifriranje\",\"znak\"]".

Ako želite da biste autorizirali tu istu aplikaciju da biste pročitali tajne u vaše zbirke ključeva pokrenite sljedeću naredbu:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '["get"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Ako želite koristiti modul sigurnost hardver (HSM) ##

Za dodatnu jamstva, moguće je uvesti i generiranje ključeva u hardver sigurnost Module (HSMs) koji nikad ostavite HSM granicu. Na HSMs su FIPS 140-2 razine 2 provjeriti. Ako taj zahtjev ne odnose na vas, preskočite ovaj odjeljak i otvorite da biste [izbrisali ključa sigurnog i pridruženi ključeva i tajne](#delete-the-key-vault-and-associated-keys-and-secrets).

Da biste stvorili ključeve HSM zaštićen, morate imati pretplatu sigurnog koji podržava zaštićenima HSM tipke.

Kada stvorite na keyvault, dodajte parametar "sku":

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

U ovom sigurnog možete dodati zaštićenima softver (kao što je prikazano ranije) i HSM zaštićene tipke. Da biste stvorili ključa HSM zaštićen, postavite na odredište parametar "HSM":

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

Koristite sljedeću naredbu da biste uvezli ključ iz datoteke .pem na vašem računalu. Ta se naredba uvozi tipku u HSMs u servisu sigurnog ključ:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

Uvozi sljedeće naredbe u "Premjesti vlastiti ključ" paketa (BYOK). Omogućuje vam stvaranje ključa u vašem lokalnom HSM i prenijeti ga HSMs u servisu sigurnog ključ bez napuštanja granicu HSM ključa:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Detaljnije upute o tome da biste generirali paket BYOK potražite u članku [upute za korištenje tipki HSM-Protected sigurnog ključ Azure](key-vault-hsm-protected-keys.md).


## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Izbrišite ključ sigurnog i pridruženi tipke i tajne

Ako više ne trebate ključa sigurnog i ključ ili tajna koje sadrži, možete izbrisati ključa sigurnog pomoću naredbe Izbriši azure keyvault:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Ili možete izbrisati grupu cijela Azure resursa koja sadrži ključne sigurnog i ostale resurse uvrštene u toj grupi:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Ostale naredbe Azure sučelje naredbenog retka za različite platforme

Ostale naredbe možda korisne su za upravljanje sigurnog ključ Azure.

Ta se naredba navedeni tablični prikaz svih tipke i odabrana svojstva:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Ta naredba prikazuje cijeli popis svojstava za navedeni ključ:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Ta se naredba navedeni tablični prikaz svih tajnu imena i odabrana svojstva:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Evo primjera kako ukloniti određenih tipka:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Evo primjera kako ukloniti određene tajna:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Daljnji koraci

Programiranje reference, potražite u članku [Vodič za Azure ključ sigurnog programiranje](key-vault-developers-guide.md).
