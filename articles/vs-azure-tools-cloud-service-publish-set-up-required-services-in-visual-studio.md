<properties
   pageTitle="Priprema za objavu ili uvođenje Azure aplikacije Visual Studio | Microsoft Azure"
   description="Saznajte postupke da biste postavili oblaka i računa servisi za pohranu i konfigurirali Azure aplikaciju."
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

# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Priprema za objavu ili uvođenje Azure aplikacije Visual Studio

## <a name="overview"></a>Pregled

Prije objavljivanja projekta servis oblaka, morate postaviti sljedeći servisi:

- **Servis u oblaku** da biste pokrenuli vaša uloga u okruženje za Azure

- Na **račun za pohranu** koji omogućuje pristup servisima Blob, reda čekanja i tablice.

Koristite sljedeće postupke za postavljanje tih servisa i konfigurirali aplikaciju


## <a name="create-a-cloud-service"></a>Stvaranje servis u oblaku

Da biste objavili servis u oblaku za Azure, prvo morate stvoriti servise u oblaku koja se izvodi na ulogama u okruženje za Azure. [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885)možete stvoriti neki servis u oblaku kao što je opisano u odjeljku **Da biste stvorili neki servis u oblaku pomoću portala za Azure klasični**, u nastavku ovog članka. Možete stvoriti i servise u oblaku u Visual Studio pomoću čarobnjaka za objavljivanje.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Da biste stvorili neki servis u oblaku pomoću Visual Studio

1. Otvaranje izbornika prečaca za Azure projekta, a zatim odaberite **Objavi**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. Ako se niste prijavili, prijavite se pomoću korisničkog imena i lozinke za Microsoftov račun ili račun tvrtke ili ustanove koji je povezan s pretplatom Azure.

1. Odaberite gumb **Dalje** da biste prešli na stranicu **Postavke** .

    ![Čarobnjak za uobičajene postavke objavljivanja](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. Na popisu **Servise u Oblaku** odaberite **Stvori novo**. Pojavit će se dijaloški okvir **Stvaranje servisa Azure** .

1. Unesite naziv servis u oblaku. Naziv dio URL-a servisa za obrasce i stoga mora biti globalno jedinstveni. Naziv je velika i mala slova.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Da biste stvorili neki servis u oblaku pomoću portala za Azure klasični

1. Prijavite se na web-mjestu Microsoft [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkId=253103) .

1. (neobavezno) Da biste prikazali popis servisa u oblaku koji ste već stvorili, odaberite vezu za servise u Oblaku na lijevoj strani stranice.

1. Odaberite na **+** ikonu u donjem lijevom uglu, a zatim na izborniku koji će se prikazati odaberite **Servis u Oblaku** . Drugi zaslon s dvije mogućnosti **Brzo stvaranje** te **Stvaranje prilagođenih**, pojavit će se. Ako odaberete da **Brzo stvaranje**, možete stvoriti neki servis u oblaku samo navođenjem URL i regije koje on će se fizički nalaziti. Ako ste odabrali **Stvoriti prilagođene**, odmah možete objaviti servis u oblaku navođenjem paket (.cspkg datoteka), datoteka konfiguracije (.cscfg) i certifikat. Stvaranje prilagođene nije potreban ako namjeravate objaviti servis u oblaku pomoću naredbe **Objavi** u projektu programa Azure. Naredba **Objavi** je dostupna na izborniku prečaca za Azure projekt.

1. Odaberite **Brzo stvaranje** kasnije pomoću Visual Studio objavljivanje servis u oblaku.

1. Unesite naziv za servis u oblaku. Pojavit će se cijeli URL pokraj naziva.

1. Na popisu odaberite područje gdje se nalaze krajnjeg korisnika.

1. Pri dnu prozora odaberite vezu za **Stvaranje servis u Oblaku** .

## <a name="create-a-storage-account"></a>Stvaranje računa za pohranu

Račun za pohranu omogućuje pristup servisima Blob, reda čekanja i tablice. Možete stvoriti račun za pohranu pomoću Visual Studio ili [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Stvaranje računa za pohranu pomoću Visual Studio

1. U **Pregledniku rješenja**, otvorite izbornički prečac za **pohranu** čvor, a zatim **Stvori račun za pohranu**.

    ![Stvaranje novog računa Azure prostora za pohranu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. Odaberite ili unesite sljedeće podatke za novi račun za pohranu u dijaloškom okviru **Stvaranje računa za pohranu** .
    - Azure pretplatu u koju želite dodati račun za pohranu.
    - Naziv koji želite koristiti za novi račun za pohranu.
    - Regija ili afinitet grupe (primjerice Zapad SAD-a ili istočnoazijski).
    - Vrste replikacije koji želite koristiti za račun za pohranu, kao što su suvišnih zemlj.

1. Kada završite, odaberite **Stvori**. Pojavit će se novi račun za pohranu na popisu **prostora za pohranu** u **Programu Explorer poslužitelja**.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Stvaranje računa za pohranu pomoću portala za Azure klasični

1. Prijavite se na web-mjestu Microsoft [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkId=253103) .

1. (Neobavezno) Da biste pogledali svoje račune za pohranu, odaberite vezu **za pohranu** u oknu na lijevoj strani stranice.

1. U donjem lijevom kutu stranice odaberite na **+** ikona.

1. Na izborniku koji će se prikazati odaberite **prostora za pohranu**, a zatim **Brzo stvaranje**.

1. Prostor za pohranu računu dajte naziv koji će rezultirati jedinstveni URL-a.

1. Imenujte servis u oblaku. Pojavit će se cijeli URL pokraj naziva.

1. Na popisu područja, odaberite područje gdje se nalaze krajnjeg korisnika.

1. Odredite želite li omogućiti zemlj replikaciju. Ako omogućite zemlj replikacije podataka spremit će se u više mjestima da biste smanjili mogućnost gubitka. Ova značajka omogućuje pohranu više skupi, ali trošak možete smanjiti tako da omogućite zemlj mjesto prilikom stvaranja računa za pohranu umjesto dodavati značajku kasnije. Dodatne informacije potražite u članku [zemlj replikacije](http://go.microsoft.com/fwlink/?LinkId=253108).

1. Pri dnu prozora odaberite vezu za **Stvaranje računa za pohranu** .

Kada stvorite račun za pohranu, vidjet ćete URL-ovi koje možete koristiti da biste pristupili resursima u svakoj od servisa Azure prostora za pohranu i primarnih i sekundarnih tipkovni prečaci za vaš račun. Pomoću ove tipke za provjeru autentičnosti zahtjeva u odnosu na servise za pohranu.

>[AZURE.NOTE] Sekundarni tipkovnog nudi isti pristup računu za pohranu kao primarni pristupni ključ i generira kao sigurnosnu kopiju treba ugrožena biti primarni tipkovnog. Osim toga, preporučuje se da Obnovi pristupnih tipki redovito. Možete izmijeniti postavke za niz veze da biste koristili tipku sekundarne pri obnovi primarni ključ, a zatim ga da biste koristili regenerated primarni ključ pri obnovi sekundarne ključ možete izmijeniti.

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Konfiguriranje aplikacije za korištenje usluge na račun za pohranu

Morate konfigurirati sve uloga koja može pristupiti servise za pohranu korištenja servisa Azure prostora za pohranu koji ste stvorili. Da biste to učinili, možete koristiti više konfiguracija servisa Azure projekta. Prema zadanim postavkama dva stvoreni u projektu Azure. Pomoću više konfiguracija servisa možete koristiti isti niz za povezivanje u kodu, ali imate neku drugu vrijednost za niz za povezivanje u svakom konfiguraciju servisa. Ako, na primjer, možete koristiti jedan konfiguracija servisa za pokretanje i ispravljanje pogrešaka aplikacije lokalno pomoću emulator Azure prostora za pohranu i konfiguracija različite servisa da biste objavili aplikaciju Azure. Dodatne informacije o konfiguracijama servisa potražite u članku [Konfiguriranje vaše Azure projekt pomoću više servisa konfiguracije](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Da biste konfigurirali aplikaciju za korištenje usluga koje omogućuje pohranu računa

1. U Visual Studio otvorite Azure rješenje. U pregledniku rješenja, otvorite izbornički prečac za svaku ulogu u Azure projektu koji pristupa servise za pohranu i odaberite **Svojstva**. U uređivaču za Visual Studio prikazat će se stranica s nazivom uloge. Na stranici prikazuju se polja za karticu **Konfiguracija** .

1. Na stranicama svojstava za ulogu, odaberite **Postavke**.

1. Na popisu **Konfiguracija servisa** odaberite naziv konfiguracije servis koji želite urediti. Ako želite da biste promijenili cjelokupan konfiguracija servisa za ovu ulogu, možete odabrati **Sve konfiguracije**.  Dodatne informacije o ažuriranju konfiguracija servisa potražite u odjeljku **Upravljanje niza veze za račune za pohranu** u temi [Konfiguriraj ulogama za Azure Oblaku s Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Da biste promijenili postavke niz veze, odaberite **...** gumb uz postavku. Pojavit će se dijaloški okvir **Stvaranje niza za povezivanje za pohranu** .

1. U odjeljku **Povezivanje korištenjem**, odaberite mogućnost **pretplate** .

1. Na popisu **Pretplata** odaberite pretplatu. Ako popis pretplata ne sadrži onaj koji želite, odaberite vezu za **Preuzimanje postavke objavljivanja** .

1. Na popisu **naziv računa** odaberite naziv računa spremišta. Alati za Azure automatski dohvaća vjerodajnice za pohranu računa pomoću datoteke .publishsettings. Da biste ručno odredili vjerodajnice za pohranu račun, odaberite mogućnost **ručno unijeli vjerodajnice** , a zatim nastavite s ovim postupkom. Naziv računa za pohranu i primarni ključ možete pristupiti iz [Azure klasični portal](http://go.microsoft.com/fwlink/p/?LinkID=213885). Ako ne želite da biste odredili prostora za pohranu postavke računa odaberite ručno, gumb **u redu** da biste zatvorili dijaloški okvir.

1. Odaberite vezu za **račun Enter za pohranu** vjerodajnica.

1. U okvir **naziv računa** unesite naziv vašeg računa za pohranu.

    >[AZURE.NOTE] Prijavite se na [portal za Azure klasični](http://go.microsoft.com/fwlink/?LinkID=213885), a zatim odaberite gumb **za pohranu** . Na portalu prikazuje popis računa za pohranu. Ako odaberete račun, otvorit će se stranica za njega. Naziv računa spremišta možete kopirati s ove stranice. Ako koristite prethodnu verziju portalu klasični naziv računa spremišta prikazuje u prikazu **Račune za pohranu** . Da biste kopirali ovim nazivom, isticanje u prozoru **Svojstva** ovog prikaza, a zatim tipke Ctrl-C. Da biste zalijepili naziv u Visual Studio, odaberite tekstni okvir **naziv računa** , a zatim tipke Ctrl + V.

1. U okvir **ključ za račun** unesite primarni ključ ili kopirajte i zalijepite ga s [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).
    Da biste kopirali sljedećeg ključa:

    1. Pri dnu stranice za račun odgovarajuće prostora za pohranu, odaberite gumb **Upravljanje tipke** .

    1. Na stranici **Upravljanje tipki pristupa** odaberite tekst primarni tipkovni prečac, a zatim tipke Ctrl + C.

    1. U odjeljku Alati za Azure zalijepite ključ u okvir **ključ za račun** .

    1. Morate odabrati jednu od sljedećih mogućnosti da biste odredili način na koji će servis pristup računu za pohranu:
        - **Korištenje HTTP-a**. To je standardni mogućnost. Na primjer, `http://<account name>.blob.core.windows.net`.
        - **Korištenje HTTPS** za sigurnu vezu. Na primjer, `https://<accountname>.blob.core.windows.net`.
        - **Navedite prilagođeni krajnje točke** za svaki od tri servisa. Zatim možete upisati te krajnje točke u polje za određenu uslugu.

        >[AZURE.NOTE] Ako stvorite prilagođeni krajnje točke, možete stvoriti složenije niz za povezivanje. Kada koristite ovaj oblik niza, možete odrediti krajnje točke servisa za pohranu koji sadrže prilagođeni naziv domene koji ste se registrirali za vaš račun za pohranu sa servisom Blob. Također možete dodijeliti pristup samo onim blob resursa u jedan spremnik kroz zajednički pristup potpis. Dodatne informacije o stvaranju prilagođene krajnje točke potražite u članku [Konfiguriranje nizove veze Azure prostora za pohranu](storage-configure-connection-string.md).

1. Da biste spremili promjene niza veze, odaberite gumb **u redu** , a zatim gumb **Spremi** na alatnoj traci. Nakon što spremite promjene, možete dobiti vrijednost niz za povezivanje u kodu pomoću [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Kad objavite aplikaciju Azure, odaberite konfiguracija servisa koja sadrži račun za Azure prostora za pohranu za niz za povezivanje. Nakon objavljivanja aplikacija aplikacije provjerite funkcionira li očekivani protiv servisa Azure prostora za pohranu

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o objavljivanju web-aplikacije za Azure s Visual Studio, potražite u članku [Objavljivanje neki servis u Oblaku pomoću alata za Azure](vs-azure-tools-publishing-a-cloud-service.md).
