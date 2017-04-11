<properties 
   pageTitle="Upravljati računom za pohranu StorSimple | Microsoft Azure"
   description="U članku se objašnjava kako možete koristiti stranici Konfiguracija StorSimple Upravitelj za dodavanje, uređivanje, brisanje ili zakretanje sigurnosnih ključeva za pohranu račun povezan s polja virtualne StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/29/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storage-accounts-for-storsimple-virtual-array"></a>Upravljanje računima za pohranu za StorSimple virtualne polja pomoću servisa StorSimple Manager

## <a name="overview"></a>Pregled

Stranici **Konfiguracija** predstavlja globalni servisa parametre koje je moguće stvoriti u servisu StorSimple Manager. Parametara se mogu primijeniti na svim uređajima koji su povezani sa servisom, a obuhvaćaju:

- Računi za pohranu 
- Pristup zapisima kontrola 

Pomoću ovog praktičnog vodiča objašnjava kako možete koristiti stranici **Konfiguracija** za dodavanje, uređivanje ili brisanje račune za pohranu za vaše StorSimple virtualne polja. Informacije u ovom ćete praktičnom vodiču odnosi samo na polja virtualne StorSimple pokrenut softver izdanje ožujak 2016 GA.

 ![Konfiguriranje stranice](./media/storsimple-ova-manage-storage-accounts/configure_service_page.png)  

Računi za pohranu sadrže vjerodajnica koje uređaj koji se koristi za pristup računu za pohranu kod davatelja usluge oblaka. Za račune za pohranu za Microsoft Azure, ovo su vjerodajnice kao što su naziv računa i pristup primarni ključ. 

Na stranici **Konfiguracija** sve račune za pohranu koji su stvoreni za naplatu pretplate se prikazuju u obliku tablice koje sadrže sljedeće informacije:

- **Naziv** – jedinstveni naziv dodijeljen račun prilikom stvaranja.
- **SSL omogućen** – bez obzira na SSL omogućen, a uređaj u oblak komunikacije je putem sigurnog kanala.

Uobičajene zadatke povezane s računima za pohranu koji se mogu obaviti na stranici **Konfiguracija** su:

- Dodavanje računa za pohranu 
- Uređivanje prostora za pohranu računa 
- Brisanje računa za pohranu 


## <a name="types-of-storage-accounts"></a>Vrsta računa za pohranu

Postoje tri vrste računa za pohranu koje je moguće koristiti s uređajem StorSimple.

- **Automatski generirani prostora za pohranu računa** – kao što je naziv predlaže, ove vrste računa za pohranu automatski se stvara prilikom stvaranja servis. Dodatne informacije o načina stvaranja ovog računa za pohranu potražite u članku [Stvaranje novog servisa](storsimple-ova-manage-service.md#create-a-service). 
- **Računi za pohranu u pretplatu servisa** – to su računi Azure prostora za pohranu koje su vezane uz iste pretplate kao servisa. Da biste saznali više o kako ti pohranu se stvaraju račune, potražite u članku [O računi servisa Azure prostora za pohranu](../storage/storage-create-storage-account.md). 
- **Računi za pohranu izvan s pretplatom** – to su računi Azure prostora za pohranu koji su povezani sa servisima i vjerojatno postojao prije stvaranja servis.

Svaki StorSimple virtualne polja stvara spremnik (s prefiksom hcs) u račun povezan prostora za pohranu. Ovaj spremnik sadrži sve podatke oblaka za svoj uređaj. Izbrišite ovaj spremnik tako da pristupa putem servisa za pohranu Azure kao što je ta akcija će dovesti do gubitka podataka.

## <a name="add-a-storage-account"></a>Dodavanje računa za pohranu

Možete dodati račun za pohranu StorSimple Upravitelj konfiguracije servisa unosom jedinstveni neslužbeni naziv i vjerodajnice za pristup koje ste povezali s računom za pohranu. Imate mogućnost koje donosi njihovo Omogućivanje načina rada sigurne sockets layer (SSL) za stvaranje sigurnog kanala za mrežnu komunikaciju između uređaja i s oblakom.

Možete stvoriti više računa za davatelja usluga za dani oblaka. Prilikom spremanja račun za pohranu, servis pokušava komunicirati s vaš davatelj usluga oblaka. Vjerodajnice i materijala za pristup navedeni će se autentičnost trenutno. Račun za pohranu stvara samo ako se potvrdi provjeru autentičnosti. Ako provjera autentičnosti ne uspije, pa poruka o pogrešci odgovarajuće prikazat će se.

Voditelj resursa za pohranu račune stvorene u Azure portal podržani su i s StorSimple. Voditelj resursa za pohranu račune neće se prikazati u padajućem popisu za odabir, samo prostora za pohranu račune stvorene na portalu za Azure klasični će se prikazati. Voditelj resursa za pohranu računi ćete morati dodati postupkom da biste dodali račun za pohranu prema uputama u nastavku.

Ispod je detaljan postupak dodavanja računa Azure klasični prostora za pohranu.

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-ova-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Uređivanje prostora za pohranu računa

Možete urediti račun za pohranu koji se koristi vaš uređaj. Ako uređujete na račun za pohranu koji se trenutno koristi, polja dostupna za izmjenu su tipka za pristup i načinu SSL za račun za pohranu. Možete navesti novi prostor za pohranu tipkovni prečac ili izmjena odabir **načinu Omogući SSL** i spremiti ažurirane postavke.

#### <a name="to-edit-a-storage-account"></a>Da biste uredili s računom za pohranu

1. Na stranici odredišna servisa odaberite uslugu, dvokliknite naziv usluge i kliknite **Konfiguriraj**.

2. Kliknite **Dodavanje i uređivanje račune za pohranu**.

3. U dijaloškom okviru **Dodavanje/uređivanje prostora za pohranu računa** :

  1. U padajućem popisu računa **Za pohranu**, odaberite postojeći račun koji želite izmijeniti. 
  2. Ako je potrebno, možete izmijeniti odabira **Omogući način rada SSL** .
  3. Možete odabrati Obnovi ključeva za pristup računu prostora za pohranu. Dodatne informacije potražite u članku [Obnovi račun tipki za pohranu](storage-create-storage-account.md#manage-your-storage-access-keys). Navedite novi ključ računa za pohranu. Račun za Azure pohranu je to pristupni primarni ključ. 
  4. Kliknite ikonu provjeri ![provjerite ikona](./media/storsimple-ova-manage-storage-accounts/checkicon.png) da biste spremili postavke. Postavke će se ažurirati na stranici **Konfiguracija** . 
  5. Pri dnu stranice, kliknite **Spremi** da biste spremili nedavno ažurirane postavke. 

     ![Uređivanje prostora za pohranu računa](./media/storsimple-ova-manage-storage-accounts/modifyexistingstorageaccount.png)
  
## <a name="delete-a-storage-account"></a>Brisanje računa za pohranu

> [AZURE.IMPORTANT] Prostor za pohranu računa možete izbrisati samo ako se ne koristi. Ako je račun za pohranu koristi, bit ćete obaviješteni.

#### <a name="to-delete-a-storage-account"></a>Da biste izbrisali račun za pohranu

1. Na stranici odredišna servisa StorSimple Upravitelj odaberite uslugu, dvokliknite naziv usluge i kliknite **Konfiguriraj**.

2. Tablični popis račune za pohranu, postavite pokazivač iznad račun koji želite izbrisati.

3. Ikona Izbriši (**x**) prikazat će se u ekstremne desnom stupcu za taj račun za pohranu. Kliknite ikonu **x** da biste izbrisali vjerodajnice.

4. Kada se zatraži potvrda, kliknite **da** da biste nastavili s brisanjem. Tablični popis će se ažurirati u skladu s promjenama.

5. Pri dnu stranice, kliknite **Spremi** da biste spremili nedavno ažurirane postavke.


## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [upravljati vaše StorSimple virtualne polja](storsimple-ova-web-ui-admin.md).
