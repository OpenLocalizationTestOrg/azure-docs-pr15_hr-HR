<properties 
   pageTitle="Upravljati računom za pohranu StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako možete koristiti stranici Konfiguracija StorSimple Manager možete dodavati, uređivati, brisanje ili zakretanje sigurnosnih ključeva za pohranu račun."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/29/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>Pomoću upravitelja StorSimple servisa upravljati računom za pohranu

## <a name="overview"></a>Pregled

Stranici **Konfiguracija** predstavlja parametara globalni usluge koje je moguće stvoriti u servisu StorSimple Manager. Parametara se mogu primijeniti na svim uređajima koji su povezani sa servisom, a obuhvaćaju:

- Računi za pohranu 
- Predlošci propusnosti 
- Pristup zapisima kontrola 

Pomoću ovog praktičnog vodiča objašnjava kako možete koristiti stranici **Konfiguracija** za dodavanje, uređivanje, ili izbrisati račune za pohranu ili zakretanje sigurnosnih ključeva za pohranu račun.

 ![Konfiguriranje stranice](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Računi za pohranu sadrže vjerodajnica koje uređaj koji se koristi za pristup računu za pohranu kod davatelja usluge oblaka. Za Microsoft Azure račune za pohranu, ovo su vjerodajnice kao što su naziv računa i pristup primarni ključ. 

Na stranici **Konfiguracija** sve račune za pohranu koji su stvoreni za naplatu pretplate se prikazuju u obliku tablice koje sadrže sljedeće informacije:

- **Naziv** – jedinstveni naziv dodijeljen račun prilikom stvaranja.
- **SSL omogućen** – li u SSL omogućen, a uređaj oblak komunikacije je putem sigurnog kanala.
- **Korišteni po** – broj jedinicama pomoću računa za pohranu.

Uobičajene zadatke povezane s računima za pohranu koji se mogu obaviti na stranici **Konfiguracija** su:

- Dodavanje računa za pohranu 
- Uređivanje prostora za pohranu računa 
- Brisanje računa za pohranu 
- Ključni zakretanje račune za pohranu 

## <a name="types-of-storage-accounts"></a>Vrsta računa za pohranu

Postoje tri vrste računa za pohranu koje je moguće koristiti s uređajem StorSimple.

- **Automatski generirani prostora za pohranu računa** – kao što je naziv predlaže, ove vrste računa za pohranu automatski se stvara prilikom stvaranja servis. Da biste saznali više o načina stvaranja ovog računa za pohranu, pročitajte članak [Korak 1: Stvaranje novog servisa](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) u [uvođenja lokalnog StorSimple uređaj](storsimple-deployment-walkthrough.md). 
- **Računi za pohranu u pretplatu servisa** – to su računi Azure prostora za pohranu koje su vezane uz iste pretplate kao servisa. Da biste saznali više o kako ti pohranu se stvaraju račune, potražite u članku [O računi servisa Azure prostora za pohranu](../storage/storage-create-storage-account.md). 
- **Računi za pohranu izvan s pretplatom** – to su računi Azure prostora za pohranu koji su povezani sa servisima i vjerojatno postojao prije stvaranja servis.

## <a name="add-a-storage-account"></a>Dodavanje računa za pohranu

Možete dodati račun za pohranu unosom jedinstveni neslužbeni naziv i vjerodajnice za pristup koje ste povezali s računom za pohranu (kod davatelja usluga navedeni oblaka). Imate mogućnost koje donosi njihovo Omogućivanje načina rada sigurne sockets layer (SSL) za stvaranje sigurnog kanala za mrežnu komunikaciju između uređaja i s oblakom.

Možete stvoriti više računa za davatelja usluga za dani oblaka. Imajte na umu, no da nakon stvaranja računa za pohranu, ne možete promijeniti davatelja usluge oblaka.

Prilikom spremanja račun za pohranu, servis pokušava komunicirati s vaš davatelj usluga oblaka. Vjerodajnice i materijala za pristup navedeni će se autentičnost trenutno. Račun za pohranu stvara samo ako se potvrdi provjeru autentičnosti. Ako provjera autentičnosti ne uspije, pa poruka o pogrešci odgovarajuće prikazat će se.

Voditelj resursa za pohranu račune stvorene u Azure portal podržani su i s StorSimple. Voditelj resursa za pohranu račune neće se prikazivati na padajućem popisu za odabir prilikom pokušaja stvaranje spremnika glasnoću samo za pohranu korisničke račune stvorene na portalu za Azure klasični će se prikazati. Voditelj resursa za pohranu računi ćete morati dodati postupkom da biste dodali račun za pohranu opisanim.

> [AZURE.NOTE] Postupak dodavanja računa za pohranu razlikuje se ovisno o verziji programa StorSimple koristite. Obavezno slijedite odgovarajuće postupak za vašu verziju StorSimple.


[AZURE.INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Uređivanje prostora za pohranu računa

Račun za pohranu koji koriste spremniku glasnoću možete uređivati. Ako uredite na račun za pohranu koji se trenutno koristi samo polja dostupnom za izmjenu je tipka za pristup za račun za pohranu. Možete navesti novi tipkovni prečac za pohranu i spremiti ažurirane postavke.

#### <a name="to-edit-a-storage-account"></a>Da biste uredili s računom za pohranu

1. Na stranici odredišna servisa odaberite uslugu, dvokliknite naziv usluge i kliknite **Konfiguriraj**.

2. Kliknite **Dodavanje i uređivanje račune za pohranu**.

3. U dijaloškom okviru **Dodavanje/uređivanje prostora za pohranu računa** :

  1. U padajućem popisu računa **Za pohranu**, odaberite postojeći račun koji želite izmijeniti. To može obuhvaćati i račune za pohranu koji pojavile su se automatski kada je stvoreno servis.
  2. Ako je potrebno, možete izmijeniti odabira **Omogući način rada SSL** .
  3. Možete odabrati da biste zakrenuli ključeva za pristup računu prostora za pohranu. Dodatne informacije o kako obavljati ključne zakretanje potražite u članku [Rotiranje ključ računa za pohranu](#key-rotation-of-storage-accounts) .
  4. Kliknite ikonu provjeri ![provjerite ikona](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) da biste spremili postavke. Postavke će se ažurirati na stranici **Konfiguracija** . Kliknite **Spremi** da biste spremili nedavno ažurirane postavke.

     ![Uređivanje prostora za pohranu računa](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)
  
## <a name="delete-a-storage-account"></a>Brisanje računa za pohranu

> [AZURE.IMPORTANT] Prostor za pohranu računa možete izbrisati samo ako je ne koristi spremniku glasnoću. Ako spremniku glasnoću koristi račun za pohranu, najprije izbrišite glasnoće, a zatim izbrišite račun povezan prostora za pohranu.

#### <a name="to-delete-a-storage-account"></a>Da biste izbrisali račun za pohranu

1. Na stranici odredišna servisa StorSimple Upravitelj odaberite uslugu, dvokliknite naziv usluge i kliknite **Konfiguriraj**.

2. Tablični popis račune za pohranu, postavite pokazivač iznad račun koji želite izbrisati.

3. Ikona Izbriši (**x**) prikazat će se u ekstremne desnom stupcu za taj račun za pohranu. Kliknite ikonu **x** da biste izbrisali vjerodajnice.

4. Kada se zatraži potvrda, kliknite **da** da biste nastavili s brisanjem. Tablični popis će se ažurirati u skladu s promjenama.

## <a name="key-rotation-of-storage-accounts"></a>Ključni zakretanje račune za pohranu

Sigurnosnih vam razloga ključa rotacije često preduvjet je u centri za podatke. 

> [AZURE.NOTE] Sljedeće informacije ključa zakretanje i zakretanje postupak primjenjuju se samo račune za pohranu za Microsoft Azure. Ako koristite drugog davatelja usluge u oblaku, možete upravljati tipke račun za pohranu putem drugog davatelja nadzorne ploče.
 
Microsoft Azure pretplate može imati jedan ili više računa pridruženog prostora za pohranu. Pristup poslovne subjekte upravlja tipki pretplate i pristup za svaki račun za pohranu. 

Kada stvorite račun za pohranu, Microsoft Azure stvara dva tipki pristupa 512-bitni prostora za pohranu koji se koriste za provjeru autentičnosti prilikom pristupa računu za pohranu. Imate dvije tipke za pristup za pohranu omogućuje Obnovi tipki bez prekida u funkcioniranju servisa za pohranu ili pristup tom servisu. Ključ koji se trenutno koristi je *primarni* ključ i sigurnosne kopije ključ naziva *sekundarne* ključ. Jedno od te dvije tipke mora biti navedena kad uređaju Microsoft Azure StorSimple pristupa vaš davatelj usluga oblaka za pohranu.

## <a name="what-is-key-rotation"></a>Što je ključ zakretanje?

Obično aplikacije koristiti samo jedan od ključeva pristup vašim podacima. Nakon određenog vremena, imate premjestite na drugi ključ za korištenje aplikacije. Nakon što ste prešli aplikacije sekundarne ključ, možete povući prvog ključa i stvoriti novi ključ. Pomoću dvije tipke na taj način omogućuje pristup aplikacijama podatke bez povećavanja sve isključiti.

Tipke račun za pohranu uvijek se sprema u usluzi u obrascu šifrirane. Međutim, to može se ponovno postaviti putem StorSimple Upravitelj servisa. Servis možete dobiti primarni ključ i sekundarne ključ za sve račune za pohranu u okviru iste pretplate, uključujući račune stvorene u servis za pohranu, kao i zadani prostor za pohranu stvoreni računi koji su generirani prilikom StorSimple Upravitelj servis za servis je prvi. Servis za servis upravitelja StorSimple će se uvijek se ove tipke na portalu Azure klasični i sprema ih u šifrirane način.

## <a name="rotation-workflow"></a>Zakretanje tijeka rada

Administrator sustava Microsoft Azure možete Obnovi ili promijeniti primarni ili sekundarni ključ izravno pristupiti računu za pohranu (putem servisa Microsoft Azure Storage). Servis StorSimple Upravitelj potražite tu promjenu automatski.

Obavještavate servis upravitelja StorSimple promjene ćete potrebna za pristup servisu Upravitelj StorSimple pristup računu za pohranu i sinkronizirajte primarni ili sekundarni ključ (ovisno o tome koji je promijenjena). Servis zatim dobiva najnoviji ključ, šifrira tipki i šalje šifrirani ključ na uređaju.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Da biste sinkronizirali tipke za račune za pohranu u okviru iste pretplate kao servis (samo za Azure)

1. Na stranici **servisa** kliknite karticu **Konfiguracija** .

2. Kliknite **Dodavanje i uređivanje račune za pohranu**.

3. U dijaloškom okviru učinite sljedeće:

  1. Odaberite račun za pohranu pomoću ključa koji želite sinkronizirati. Tipke za pohranu računa šifrirane kada su prikazani.
  2. U servisu StorSimple Upravitelj trebate ažurirati ključ koji je prethodno promijenjen u servisu spremište na platformi Microsoft Azure. Ako access primarni ključ je promijenjeno (regenerated), kliknite **Sinkroniziraj primarni ključ**. Ako je promijenjeno sekundarni ključ, kliknite **Sinkroniziraj sekundarni ključ**.

    ![Sinkronizacija tipke](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Da biste sinkronizirali tipke za račune za pohranu izvan pretplate servisa

1. Na stranici **servisa** kliknite karticu **Konfiguracija** .

2. Kliknite **Dodavanje i uređivanje račune za pohranu**.

3. U dijaloškom okviru učinite sljedeće:

  1. Odaberite račun za pohranu s tipkovni prečac koji želite ažurirati.
  2. Morat ćete ažuriranje tipkovni prečac za pohranu na servisu StorSimple Upravitelj. U ovom slučaju, vidjet ćete tipkovni prečac za pohranu. U okvir y **Tipka za pristup računu za pohranu**unesite novi ključ. 
  3. Spremite promjene. Ključ za pohranu računa programa access sada treba ažurirati.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [sigurnosti StorSimple](storsimple-security.md).
- Dodatne informacije o [korištenju StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
