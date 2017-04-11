<properties
    pageTitle="Stvorite račun za Azure obradu | Microsoft Azure"
    description="Saznajte kako stvoriti račun za Azure grupe na portalu za Azure da biste pokrenuli veliki paralelno radnih opterećenja u oblak"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Stvorite račun grupe za Azure pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](batch-account-create-portal.md)
- [Upravljanje grupe za .NET](batch-management-dotnet.md)

Saznajte kako stvoriti račun za Azure obradu [Azure portal][azure_portal], te gdje možete pronaći važne kao što su svojstva računa pristupne ključeve i račun URL-ova. Ne možemo navode i grupe za cijene i povezivanje računa za pohranu Azure s računa za obradu da bi mogli koristiti [pakete aplikacija](batch-application-packages.md) i [zadržava izlaz za posao i zadatak](batch-task-output.md).

## <a name="create-a-batch-account"></a>Stvaranje grupe za račun

1. Prijava na [portal za Azure][azure_portal].

2. Kliknite **Novi** > **Izračun** > **grupe za servis**.

    ![Grupe na tržištu][marketplace_portal]

3. Prikazat će se **Novi račun za obradu** plohu. U odjeljku stavki *u* do *e* ispod za opise svaki element plohu.

    ![Stvaranje grupe za račun][account_portal]

    na. **Naziv računa**: jedinstveni naziv za račun grupe. Taj naziv mora biti jedinstvena u području Azure račun se stvara (Vidi *mjesto* dolje). Može sadržavati samo malim slovima, brojeva i mora biti 3 24 znakova.

    b. **Pretplate**: pretplate u kojoj želite stvoriti račun grupe. Ako imate samo jedan pretplatu, odabire se po zadanom.

    c. **Grupa resursa**: postojeći resurs grupiranje za novi račun grupe ili ako želite stvoriti novu.

    d. **Lokacija**: programa Azure regija u kojoj želite stvoriti račun grupe. Samo područja podržava svoju pretplatu i grupa resursa prikazuju se kao jedna od mogućnosti.

    e. **Račun za pohranu** (neobavezno): račun za pohranu **opće namjene** pridružiti (veza) na novi račun grupe. Dodatne informacije potražite u članku [povezani Azure prostora za pohranu račun](#linked-azure-storage-account) ispod.

4. Kliknite **Stvori** da biste stvorili račun.

  Portalu znači da je **Deploying** račun, a nakon dovršetka **implementacijama uspjela** obavijest pojavljuje se u *obavijesti*.

## <a name="view-batch-account-properties"></a>Prikaz svojstava grupe za račun

Nakon što račun, možete otvoriti **plohu računa grupe** da biste pristupili njegove postavke i svojstva. Sve postavke računa i svojstva možete pristupiti pomoću lijevom izborniku plohu račun grupe.

![Grupe za račun plohu Azure portalu][account_blade]

* **URL za obradu računa**: programima koje stvarate [obradu razvoj API-ji](batch-technical-overview.md#batch-development-apis) sustava potreban račun URL-a za upravljanje resursima i pokretanje zadataka u račun. URL za račun grupe s oblikovanjem sljedeće:

    `https://<account_name>.<region>.batch.azure.com`

![URL računa grupe na portalu][account_url]

* **Tipke za pristup**: aplikacija trebati tipkovnog prilikom rada s resursima na vašem računu grupe. Da biste pogledali ili Obnovi tipke za pristup računu grupe, unesite `keys` u okvir za **pretraživanje** lijevom izborniku na račun plohu grupe, zatim odaberite **tipke**.

    ![Grupe za račun tipke Azure portalu][account_keys]

## <a name="pricing"></a>Cijene

Računi za obradu nudi se samo u "Besplatne sloj," što znači da ne naplaćuje za obradu račun sam. Vam se naplatiti za podlozi Azure računalnim resursa koje vašeg rješenja za obradu i resursima troše druge servise kada vaš pokreću. Ako, na primjer, vam se naplatiti za čvorove računalnim u svoje grupe i za podatke koje ste pohranili na pohranu Azure kao ulazni i izlazni za svoje zadatke. Isto tako, ako koristite značajku [pakete](batch-application-packages.md) serije, vam se naplatiti za pohranu Azure resursa koji se koriste za pohranu paketi za aplikacije. U odjeljku [grupe za cijene] [ batch_pricing] dodatne informacije.

## <a name="linked-azure-storage-account"></a>Povezani poslovni subjekt Azure prostora za pohranu

Kao što je rečeno ranije, (nije obavezno) možete povezati s računom za pohranu **opće namjene** na račun za obradu. Značajku [pakete](batch-application-packages.md) serije koristi spremište blobova platforme u povezane opće namjene račun za pohranu, kao i [Obradu datoteka konvencije .NET](batch-task-output.md) biblioteke. Ove značajke će vam pomoći u implementacija aplikacije pokretanje grupe za zadatke i persisting se daju podatke.

Obrada trenutno podržava *samo* vrstu računa za pohranu za **opće namjene** kao što je opisano u koraku 5, [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account), u [račune za pohranu o Azure](../storage/storage-create-storage-account.md). Kada povežete račun za Azure prostor za pohranu na račun za obradu, je li vezu *samo* račun za pohranu **opće namjene** .

![Stvaranje računa za pohranu "Opće namjene"][storage_account]

Preporučujemo vam da stvorite račun za pohranu za isključivo korištenje vaš račun grupe.

>[AZURE.WARNING] Pobrinuti se kada ponovno stvaranje pristupnih tipki povezani poslovni subjekt za pohranu. Obnovi samo jedan račun ključa za pohranu, a zatim kliknite **Sinkroniziraj tipke** na povezani račun plohu za pohranu. Da biste omogućili tipke koje je potrebno prenijeti na računalni čvorove u grupe, a zatim Obnovi i sinkronizirati s logotipom, ako je potrebno, pričekajte pet minuta. Ako oba tipke Obnovi u isto vrijeme, vaše čvorove računalnim nećete moći sinkronizirati ili ključ te će izgubiti pristup računu za pohranu.

  ![Ponovno stvaranje ključeva za pohranu računa][4]

## <a name="batch-service-quotas-and-limits"></a>Grupe servisa kvotama i ograničenja

Imajte na umu da kao i kod pretplate Azure i drugih servisa za Azure, određenih [kvotama i ograničenja](batch-quota-limit.md) primjenjuju s računima za obradu. Trenutni kvote za obradu račun pojavi na portalu na računu **Svojstva**.

![Grupe za račun kvote Azure portalu][quotas]

Ove kvote Imajte na umu kao što su dizajniranje i skaliranje gore radnih opterećenja sustava grupe. Na primjer, ako vaše skup nije dostigao cilj broj računalnim čvorove koje ste naveli, možda Dostigli ste core kvotu za vaš račun grupe.

Imajte na umu da se ne ograničili jednog računa za obradu Azure pretplatu. Pokretanje većeg broja radnih opterećenja grupe u jedan račun grupe ili distribucija vaše radnih opterećenja među računima za grupe u okviru iste pretplate, ali u različitim područjima Azure.

Mnoge od tih kvote možete povećati jednostavno sa zahtjevom za podršku besplatnih poslan na portalu za Azure. Potražite u članku [kvotama i ograničenja servisa za obradu Azure](batch-quota-limit.md) pojedinosti o traži povećava kvote.

## <a name="other-batch-account-management-options"></a>Druge mogućnosti upravljanja za obradu računa

Osim pomoću portala za Azure, možete stvoriti i upravljati grupe za račune za sljedeće:

* [PowerShell cmdleti za grupe](batch-powershell-cmdlets-get-started.md)
* [Azure EŽA](../xplat-cli-install.md)
* [Upravljanje grupe za .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Daljnji koraci

* Potražite u članku [Pregled obrade Azure značajku](batch-api-basics.md) da biste saznali više o grupe servisa koncepata i značajke. U članku govori o primarni obradu resurse kao što su grupe, računalnim čvorove, zadatke i zadatke, a sadrži pregled značajki usluge koje omogućuju veliki računalnim radno opterećenje izvršavanja.

* Informirajte se o osnovama razvoj aplikacije za obradu omogućena pomoću [grupe za .NET klijentska biblioteka](batch-dotnet-get-started.md). [Uvodni članak](batch-dotnet-get-started.md) vodi vas kroz rad aplikacije koji koristi servis za obradu izvršiti radno opterećenje na više računalnim čvorove i uključuje Azure prostora za pohranu za pripremna datoteka radno opterećenje i dohvaćanja.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Ponovno stvaranje ključeva za pohranu računa"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
