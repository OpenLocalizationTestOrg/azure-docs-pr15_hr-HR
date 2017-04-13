<properties
   pageTitle="Kako upravljati konfiguracija servisa i profile | Microsoft Azure"
   description="Saznajte kako raditi s konfiguraciju i profili konfiguracije datoteke servisa | koji se spremanje postavki za implementaciju okruženja i objavljivanje postavke za servise u oblaku."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-manage-service-configurations-and-profiles"></a>Kako upravljati konfiguracija servisa i profile

## <a name="overview"></a>Pregled

Kad objavite servis u oblaku, Visual Studio pohranjuje informacije o konfiguraciji u dvije vrste datoteka konfiguracije: servis konfiguraciju i profile. Konfiguracija servisa (.cscfg datoteke) spremanje postavki za implementaciju okruženja za Azure oblaku. Azure te datoteke konfiguracije koristi kada je upravlja servise u oblaku. S druge strane, profili (.azurePubxml datoteke) iz trgovine postavke objavljivanja za servise u oblaku. Ove su postavke zapis o što odlučite kada koristiti Čarobnjak za objavljivanje i Visual Studio koriste lokalno. U ovoj se temi objašnjava kako raditi s obje vrste datoteka konfiguracije.

## <a name="service-configurations"></a>Konfiguracija servisa

Možete stvoriti više konfiguracija servisa za svaki od vašeg okruženja za implementaciju. Na primjer, možete stvoriti konfiguracija servisa za lokalni okruženje koje koristite za pokretanje i testirati Azure aplikacije i drugi konfiguracija servisa za okruženje sustava radnog.

Možete dodati, brisanje, preimenovanje i izmjena ove konfiguracije servisa svojim potrebama. Ove konfiguracije servisa možete upravljati iz Visual Studio, kao što je prikazano na sljedećoj slici.

![Konfiguracija servisa za upravljanje](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Dijaloški okvir **Upravljanje konfiguracije** možete otvoriti i iz stranice svojstava u ulogu. Da biste otvorili svojstva uloge Azure projekta, otvorite izbornik prečaca za uloge, a zatim **Svojstva**. Na kartici **Postavke** proširite popis **Konfiguracija servisa** , a zatim odaberite **Upravljanje** da biste otvorili dijaloški okvir **Upravljanje konfiguracije** .

### <a name="to-add-a-service-configuration"></a>Da biste dodali konfiguracija servisa

1. U pregledniku rješenja otvaranje izbornika prečaca za Azure projekta, a zatim odaberite **Upravljanje konfiguracije**.

    Pojavit će se dijaloški okvir **Konfiguracija servisa za upravljanje** .

1. Da biste dodali konfiguracija servisa, morate stvoriti kopiju postojeće konfiguracije. Da biste to učinili, odaberite konfiguraciju koju želite kopirati na popisu naziv, a zatim odaberite **Stvori kopiju**.

1. (Neobavezno) Konfiguracija servisa drugi naziv, odaberite novi konfiguracija servisa na popisu naziv, a zatim odaberite **Preimenuj**. U tekstni okvir **naziv** upišite naziv koji želite koristiti za tu konfiguraciju servisa, a zatim odaberite **u redu**.

    Novu servisa konfiguracije datoteku pod nazivom ServiceConfiguration. [Novi naziv] .cscfg dodaje se Azure projekta u programu Explorer rješenja.


### <a name="to-delete-a-service-configuration"></a>Da biste izbrisali konfiguracija servisa

1. U pregledniku rješenja, otvorite izbornički prečac za Azure projekt, a zatim odaberite **Upravljanje konfiguracije**.

    Pojavit će se dijaloški okvir **Konfiguracija servisa za upravljanje** .

1. Da biste izbrisali konfiguracija servisa, odaberite konfiguraciju koju želite izbrisati s popisa **naziv** , a zatim odaberite **Ukloni**. Da biste potvrdili da želite izbrisati tu konfiguraciju prikazuje se dijaloški okvir.

1. Odaberite **Izbriši**.

     Konfiguracijska datoteka servisa je uklonjena iz Azure projekta u programu Explorer rješenja.


### <a name="to-rename-a-service-configuration"></a>Da biste preimenovali konfiguracija servisa

1. U pregledniku rješenja, otvorite izbornički prečac za Azure projekt, a zatim **Upravljanje konfiguracije**.

    Pojavit će se dijaloški okvir **Konfiguracija servisa za upravljanje** .

1. Da biste preimenovali konfiguracija servisa, odaberite novi konfiguracija servisa na popisu **naziv** , a zatim **Preimenuj**. U tekstni okvir **naziv** upišite naziv koji želite koristiti za tu konfiguraciju servisa, a zatim odaberite **u redu**.

    Naziv datoteke za konfiguraciju servisa se mijenja u programu project Azure u pregledniku rješenja.

### <a name="to-change-a-service-configuration"></a>Da biste promijenili konfiguracija servisa

- Ako želite da biste promijenili konfiguracija servisa, otvorite izbornički prečac za određene ulogu želite promijeniti u Azure projektu, a zatim **Svojstva**. U odjeljku [Kako: Konfiguriranje uloge za Azure Oblaku s Visual Studio](https://msdn.microsoft.com/library/azure/hh369931.aspx) dodatne informacije.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Provjerite postavka različita kombinacije pomoću profila

Pomoću profila možete automatski unijeti **Čarobnjak za objavljivanje** s raznih kombinacija postavke za različite potrebe. Ako, na primjer, možete imati jedan profil za ispravljanje pogrešaka i drugi za izdanje sastavlja. U tom slučaju profila **za ispravljanje pogrešaka** promijenile **IntelliTrace** omogućeno i odabrali konfiguracije **ispravljanje pogrešaka** i profila **izdanje** promijenile **IntelliTrace** onemogućeno i konfiguracija **izdanje** odabran. Različite profile nije moguće koristiti i za implementaciju servis pomoću računa za različite prostora za pohranu.

Kada prvi put pokrenete čarobnjak stvara je zadani profil. Visual Studio pohranjuje profil u datoteku koja ima datotečni nastavak .azurePubXml koja se dodaje Azure projekta u mapi **profila** . Ako ručno odredite različitih mogućnosti kada pokrenete čarobnjak za kasnije, ona se automatski ažurira. Prije nego što pokrenete sljedeći postupak, potrebno je već objavljena servis u oblaku barem jednom.

### <a name="to-add-a-profile"></a>Da biste dodali profila

1. Otvaranje izbornika prečaca za Azure projekta, a zatim odaberite **Objavi**.

1. Pokraj **profila ciljnog** popisa odaberite gumb **Spremi profil** kao Sljedeća slika prikazuje. Za vas stvara profil.

    ![Stvaranje novog profila](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. Nakon stvaranja profil, odaberite **<... upravljanje >** na popisu **cilj profila** .

    Pojavit će se dijaloški okvir **Upravljanje profilima** i kao Sljedeća slika prikazuje.

    ![Upravljanje profilima dijaloški okvir](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. Na popisu **naziv** odaberite profil, a zatim odaberite **Stvori kopiju**.

1. Odaberite gumb za **Zatvaranje** .

    Pojavit će se novi profil na ciljnom popisu profila.

1. Na popisu **ciljni profil** odaberite profil koji ste upravo stvorili. Objavljivanje fotoalbuma popunjavaju pomoću mogućnosti iz profila koji ste odabrali.

1. Odaberite **prethodno** i **sljedeće** gumbe za prikaz svake stranice čarobnjaka za objavljivanje, a zatim prilagodite postavke za ovaj profil. Informacije potražite u članku [Objavljivanje čarobnjak aplikacije za Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Kada završite s prilagodbom postavke odaberite **Dalje** da biste se vratili na stranicu postavke. Profil se sprema Kad objavite servis pomoću ove postavke ili ako odaberete **Spremanje** pokraj popisa profila.

### <a name="to-rename-or-delete-a-profile"></a>Preimenovanje ili brisanje profila

1. Otvaranje izbornika prečaca za Azure projekta, a zatim odaberite **Objavi**.

1. Na popisu **ciljni profil** odaberite **Upravljanje**.

1. U dijaloškom okviru **Upravljanje profilima** odaberite profil koji želite izbrisati, a zatim **Ukloni**.

1. U dijaloškom okviru za potvrdu koji će se prikazati odaberite **u redu**.

1. Odaberite **Zatvori**.

### <a name="to-change-a-profile"></a>Da biste promijenili profil

1. Otvaranje izbornika prečaca za Azure projekta, a zatim odaberite **Objavi**.

1. Na popisu **ciljni profil** odaberite profil koji želite promijeniti.

1. Odaberite **prethodno** i **sljedeće** gumbe za prikaz svake stranice čarobnjaka za objavljivanje, a zatim promijenite željene postavke. Informacije potražite u članku [Objavljivanje čarobnjak aplikacije za Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Kada završite s promjenom postavki, odaberite **Dalje** da biste se vratili na stranicu **Postavke** .

1. (Neobavezno) odaberite **Objavi** da biste objavili pomoću nove postavke servisa u oblaku. Ako ne želite objaviti servis u oblaku trenutno i zatvorite čarobnjak za objavljivanje, Visual Studio vas pita želite li spremiti promjene na profil.

## <a name="next-steps"></a>Daljnji koraci

Informirajte se o konfiguriranju druge dijelove Azure projekte od Visual Studio, potražite u članku [Konfiguriranje projekta programa Azure](http://go.microsoft.com/fwlink/p/?LinkID=623075)
