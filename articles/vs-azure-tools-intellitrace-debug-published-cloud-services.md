<properties 
   pageTitle="Ispravljanje pogrešaka na objavljeni u oblaku s IntelliTrace i Visual Studio | Microsoft Azure"
   description="Ispravljanje pogrešaka na objavljeni u oblaku s IntelliTrace i Visual Studio"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-a-published-cloud-service-with-intellitrace-and-visual-studio"></a>Ispravljanje pogrešaka na objavljeni u oblaku s IntelliTrace i Visual Studio

##<a name="overview"></a>Pregled

Kada se pokrene u Azure, s IntelliTrace, prijavite opsežne informacije za ispravljanje pogrešaka za ulogu instancu. Ako vam je potrebna da biste pronašli uzroka problema, zapisnike IntelliTrace možete koristiti da biste se pomicali kroz koda iz Visual Studio kao da je pokrećete u Azure. Na snazi IntelliTrace zapisa ključnog izvršavanje koda i okruženje podataka kada se izvodi kao servis u oblaku u Azure Azure aplikacije, a omogućuje ponavljanje snimljena podatke iz Visual Studio. Umjesto toga, možete koristiti daljinsko uklanjanje programskih pogrešaka da biste priložili izravno na servis u oblaku koja se izvodi u Azure. U odjeljku [ispravljanje pogrešaka servise u Oblaku](http://go.microsoft.com/fwlink/p/?LinkId=623041).

>[AZURE.IMPORTANT] IntelliTrace namijenjen samo scenariji za ispravljanje pogrešaka, a ne treba koristiti za implementaciju proizvodnje.

>[AZURE.NOTE] Možete koristiti IntelliTrace ako imate Visual Studio Enterprise instalirana i Azure aplikacije ciljeve .NET Framework 4 ili noviji. IntelliTrace prikuplja informacije o vašim Azure ulogama. Virtualnim strojevima te uloge uvijek pokrenuti 64-bitne operacijske sustave.

## <a name="to-configure-an-azure-application-for-intellitrace"></a>Da biste konfigurirali Azure aplikacije za IntelliTrace

Da biste omogućili IntelliTrace za Azure aplikaciju, morate stvoriti i objavljivanje aplikacije iz projekta za Visual Studio Azure. Morate konfigurirati IntelliTrace Azure aplikacije za objavljivanje u Azure. Ako objavite svoju aplikaciju bez konfiguriranje IntelliTrace, ali odlučite dobro da to učinite, morat ćete objaviti ponovno Visual Studio aplikacije. Dodatne informacije potražite u članku [Objavljivanje neki servis u Oblaku pomoću alata za Azure](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Kada ste spremni za implementaciju aplikacija za Azure, provjerite je li vaš projekt Sastavi ciljnih web-mjesta postavljene za **ispravljanje pogrešaka**.

1. Otvaranje izbornika prečaca za Azure projekta u pregledniku rješenja, a zatim odaberite **Objavi**.
 
    Pojavit će se čarobnjak za objavljivanje Azure aplikacije.

1. Da biste prikupili IntelliTrace zapisnika aplikacije prilikom objavljivanja u oblaku, odaberite potvrdni okvir **Omogući IntelliTrace** .

    >[AZURE.NOTE] Možete omogućiti IntelliTrace ili Profiliranje Kad objavite Azure aplikacije. Ne možete omogućiti i jedno i drugo.

1. Da biste prilagodili Osnovna konfiguracija IntelliTrace, odaberite **Postavke** hiperveze.

    Dijaloški okvir s postavkama IntelliTrace prikazuje se kao što je prikazano na sljedećoj slici. Možete navesti događaje zapisnika, želite li prikupiti podatke poziv, koji su moduli i postupke za prikupljanje zapisnika za i koliko je prostora za dodjelu snimku. Dodatne informacije o IntelliTrace potražite u članku [značajka ispravljanja pogrešaka s IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

Zapisnik IntelliTrace je kružne datoteke zapisnika maksimalne veličine navedena u postavkama IntelliTrace (Zadana veličina je 250 MB). Zapisnici IntelliTrace prikuplja s datotekom u datotečnom sustavu virtualnog računala. Kada se zatraži zapisnike, snimka je u tom trenutku u vrijeme i preuzeti s vašim lokalnim računalom.

Nakon Azure aplikacije objavljen Azure, možete odrediti ako je omogućeno IntelliTrace čvorovi Azure za izračun u programu Explorer poslužitelja, kao što je prikazano na sljedećoj slici:

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="downloading-intellitrace-logs-for-a-role-instance"></a>Preuzimanje IntelliTrace zapisnici uloga instance

IntelliTrace zapisnici uloga instance možete preuzeti iz čvor **Servise u Oblaku** u **Programu Explorer poslužitelja**. Proširite čvor **Servise u Oblaku** dok ne pronađete instancu koja vas zanima, otvorite izbornik prečaca za tu instancu i odaberite **Prikaz zapisnika IntelliTrace**. U zapisnicima IntelliTrace se preuzimaju s datotekom u direktorij na lokalnom računalu. Svaki put kada pokrenete u IntelliTrace zapisnike, stvara novu brzu snimku.

Kada se preuzimaju zapisnike, Visual Studio prikazuje tijek postupka u prozoru Azure zapisnik aktivnosti. Kao što je prikazano na sljedećoj slici, možete proširiti stavku retka za postupak da biste vidjeli dodatne detalje.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Možete nastaviti raditi u Visual Studio dok se preuzimaju zapisnike IntelliTrace. Kada u zapisniku je završio s preuzimanjem, ona se automatski otvarati u Visual Studio.

>[AZURE.NOTE] Zapisnici IntelliTrace može sadržavati framework stvara i naknadno rukuje iznimke. Interna framework kod generira te iznimke u sklopu normalni pokreće uloge, tako da ih sigurno možda zanemariti.

## <a name="see-also"></a>Vidi također

[Ispravljanje pogrešaka servise u Oblaku](https://msdn.microsoft.com/library/ee405479.aspx)

