<properties 
    pageTitle="Korištenje ReportViewer na Web-mjestu | Microsoft Azure"
    description="U ovoj se temi opisuje način stvaranja Microsoft Azure Web-mjesta s kontrolom za Visual Studio ReportViewer koji prikazuje izvješćem spremljenim na sustava Microsoft Azure virtualnog računala."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Korištenje ReportViewer u Web-mjesto koje se nalaze u Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Možete izrađivati Microsoft Azure Web-mjesta s kontrolom za Visual Studio ReportViewer koji prikazuje izvješćem spremljenim na sustava Microsoft Azure virtualnog računala. Kontrola ReportViewer je u web-aplikaciji sastavljanje korištenje u ASP.NET predloška web-aplikacije.

>[AZURE.IMPORTANT] Predlošci ASP.NET MVC web-aplikacija ne podržavaju kontrola ReportViewer.

Da biste ReportViewer ugraditi u programu Microsoft Azure Web-mjesta, morate dovršiti sljedeće zadatke.

- **Dodavanje** Skupovi za paketa za implementaciju

- **Konfiguriranje** Provjera autentičnosti i autorizacije

- **Objavljivanje** ASP.NET web-aplikaciji Azure

## <a name="prerequisites"></a>Preduvjeti

Pročitajte odjeljak "općenite preporuke i najbolje prakse" u [SQL Server poslovno obavještavanje u Azure virtualnih računala](virtual-machines-windows-classic-ps-sql-bi.md).

>[AZURE.NOTE] Kontrola ReportViewer su poslane s Visual Studio, Standard Edition ili noviji. Ako koristite Web za razvojne inženjere Express Edition, morate instalirati [MICROSOFT izvješće PREGLEDNIK 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) korištenja značajki za vrijeme izvođenja ReportViewer.
>
>ReportViewer konfiguriran u načinu rada s obradom lokalne nije podržan u Microsoft Azure.

Pregledajte studiju [kontrole viewer izvješća servisa Reporting Services i Microsoft Azure virtualnog računala na temelju poslužiteljima izvješća](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx).

## <a name="adding-assemblies-to-the-deployment-package"></a>Dodavanje sklopova paketa za implementaciju

Kada hostira vaš ASP.NET aplikacija na lokaciji, sklopova ReportViewer obično instaliraju izravno u predmemoriji sklopova (GAC) poslužitelja za IIS tijekom instalacije za Visual Studio, a mogu pristupiti izravno u aplikaciji. Međutim, kad hostira ASP.NET aplikacija u oblaku, Microsoft Azure ne dopušta ništa da biste instalirali u GAC, pa provjerite jesu li sklopova ReportViewer dostupan lokalno za svoju aplikaciju. Možete to učiniti tako da dodate reference na njih u projektu i konfigurirati da lokalno kopiraju.

U načinu udaljene obrada kontrola ReportViewer koristi sljedeće skupine:

- **Microsoft.ReportViewer.WebForms.dll**: sadrži kod ReportViewer koje je potrebno da biste koristili ReportViewer na stranici. Vodič za ovaj skup dodaje se projekta kada odbacite kontrola ReportViewer premjestite ASP.NET stranica u projektu.

- **Microsoft.ReportViewer.Common.dll**: sadrži klase koristi kontrola ReportViewer vrijeme izvođenja. Ona se neće automatski dodaje u projekt.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Da biste dodali referenca Microsoft.ReportViewer.Common

- Desnom tipkom miša kliknite čvor projekta **reference** i odaberite **Dodaj referenca**, odaberite skupa na kartici .NET i kliknite **u redu**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Da biste na sklopova lokalno pristupačnih aplikacija platforme ASP.NET

1. U mapi s **referencama** kliknite u sklopu Microsoft.ReportViewer.Common tako da se prikazuju svojstva u oknu svojstva.

1. U oknu svojstva postavite **Lokalnu kopiju** na True.

1. Ponovite korake 1 i 2 za Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>Da biste dobili ReportViewer jezičnog paketa

1. Instalirajte odgovarajuće slobodnu distribuciju paketa Microsoft izvješće preglednik 2012 Runtime iz [Microsoftova centra za preuzimanje](http://go.microsoft.com/fwlink/?LinkId=317386).

1. S padajućeg popisa odaberite jezik i stranice dobiva ćete preusmjereni na stranicu centra za preuzimanje.

1. Kliknite **Preuzmi** da biste pokrenuli preuzimanje ReportViewerLP.exe.

1. Kada preuzmete ReportViewerLP.exe, kliknite **Pokreni** da biste instalirali odmah ili kliknite **Spremi** da biste spremili na računalo. Ako kliknete **Spremi**, imajte na umu naziva mape koja spremiti datoteku.

1. Pronađite mapu u koju ste spremili datoteku. Desnom tipkom miša kliknite ReportViewerLP.exe, kliknite **Pokreni kao administrator**, a zatim kliknite **da**.

1. Nakon izvođenja ReportViewerLP.exe, prikazat će se na c:\windows\assembly sadrži resurs datoteke **Microsoft.ReportViewer.Webforms.Resources** i **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Da biste konfigurirali za lokalizirani kontrola ReportViewer

1. Preuzmite i instalirajte paket za slobodnu distribuciju Microsoft izvješće preglednik 2012 Runtime slijedeći gore navedene upute.

1. Stvaranje <language> mape u programu project i Kopiraj u sklopu pridružene resurse datoteka postoje. Datoteka skup resursa za kopiranje: **Microsoft.ReportViewer.Webforms.Resources.dll** i **Microsoft.ReportViewer.Common.Resources.dll**. Odaberite datoteke za skup resursa, a u oknu svojstva postavite **Kopiraj izlaznog direktorija** **uvijek kopirati"**.

1. Postavite kulture & UICulture za web project. Dodatne informacije o postavljanju kulture i Kultura korisničkog Sučelja ASP.NET podrške za potražite u članku [Kako: postavite kulture i Kultura korisničkog Sučelja za Globalizacija web-stranica platforme ASP.NET](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Konfiguriranje provjere autentičnosti i autorizacije

Na ReportViewer treba koristiti odgovarajuće vjerodajnice za provjeru autentičnosti s poslužiteljem za izvješće, a vjerodajnice morate biti ovlašteni na poslužitelju izvješća da biste pristupili izvješća koje želite. Informacije o autentičnosti, potražite u članku studiju [kontrole viewer izvješća servisa Reporting Services i Microsoft Azure virtualnog računala na temelju poslužiteljima izvješća](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>Objavljivanje ASP.NET web-aplikaciji Azure

Upute o objavljivanju ASP.NET Web-aplikacije za Azure potražite u članku [Kako: migriranje i objavljivanje web-aplikaciju za Azure s Visual Studio](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) i [Početak rada s web-aplikacije i ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Ako naredbu Dodaj projekta za implementaciju Azure ili dodavanje Azure oblaka servisa Project ne pojavi na izborniku prečaca u pregledniku rješenja, možda ćete morati promijenite framework ciljni projekta u .NET Framework 4.
>
>Dvije naredbe – zapravo ista funkcija. Jedna ili se naredba pojavit će se na izborničkom prečacu ovisno o verziji sustava Microsoft Azure SDK ste instalirali.

## <a name="resources"></a>Resursi

[Microsoft izvješća](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server poslovno obavještavanje u Azure virtualnim strojevima](virtual-machines-windows-classic-ps-sql-bi.md)

[Korištenje ljuske PowerShell za stvaranje Azure VM poslužitelju za izvješća u Nativnom načinu rada](virtual-machines-windows-classic-ps-sql-report.md)

[Reporting Services izvješće viewer kontrola i Microsoft Azure virtualnog računala na poslužiteljima izvješća](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
