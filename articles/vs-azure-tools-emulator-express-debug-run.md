<properties
   pageTitle="Da biste pokrenuli i ispravljanje pogrešaka u oblaku na lokalnom stroju pomoću Emulator Express | Microsoft Azure"
   description="Da biste pokrenuli i ispravljanje pogrešaka u oblaku na lokalnom stroju pomoću Emulator Express"
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


# <a name="using-emulator-express-to-run-and-debug-a-cloud-service-on-a-local-machine"></a>Da biste pokrenuli i ispravljanje pogrešaka u oblaku na lokalnom stroju pomoću Emulator Express

Pomoću Emulator Express možete testirati i servise u oblaku za ispravljanje pogrešaka bez izvođenja Visual Studio kao administrator. Možete postaviti projekta postavke da biste koristili Emulator Express ili cijelog emulator, ovisno o preduvjetima servis u oblaku. Dodatne informacije o cijelog emulator potražite u članku [pokretanje programa Azure u Emulator izračunati](./storage/storage-use-emulator.md). Azure SDK 2.1 najprije obuhvaćeno Emulator Express i od Azure SDK 2,3 je emulator zadani.

## <a name="using-emulator-express-in-the-visual-studio-ide"></a>Korištenje Emulator Express u Visual Studio IDE

Prilikom stvaranja novog projekta u Azure SDK 2.3 ili novijim Emulator Express je već odabran. Za postojeće projekte koje su stvorene pomoću starije verzije programa SDK-a, slijedite ove korake da biste odabrali Emulator Express.

### <a name="to-configure-a-project-to-use-emulator-express"></a>Da biste konfigurirali projekta da biste koristili Emulator Express

1. Na izborniku prečaca za Azure projekt odaberite **Svojstva**, a zatim odaberite karticu **Web** .

1. U odjeljku **Lokalni poslužitelj za razvoj**, odaberite gumb **Mogućnosti za korištenje IIS Express** . Emulator Express nije kompatibilan s IIS web-poslužitelj.

1. U odjeljku **Emulator**, odaberite gumb mogućnosti za **Korištenje Emulator Express** .

    ![Emulator Express](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## <a name="launching-emulator-express-at-a-command-prompt"></a>Pokretanje Emulator Express naredbeni redak

U naredbeni redak možete pokrenuti eksplicitnih verziju na Azure izračunati Emulator, csrun.exe, pomoću mogućnosti /useemulatorexpress.

## <a name="limitations"></a>Ograničenja

Prije korištenja Emulator Express, imajte na umu nekoliko ograničenja:

- Emulator Express nije kompatibilan s IIS web-poslužitelj.

- Servis u oblaku može sadržavati više uloge, ali ulogama ograničeno je na jednu instancu.

- Ne možete pristupiti brojeve priključaka ispod 1000. Na primjer, ako koristite davatelj usluge provjere autentičnosti koje obično koristi priključak ispod 1000, možda ćete morati tu vrijednost promijeniti broj priključka koji se nalazi iznad 1000.

- Ograničenja koji se odnose na Emulator za izračunavanje Azure primijeniti Emulator Express. Na primjer, ne možete imati više od 50 uloga instanci po implementacije. Potražite u članku [pokretanje programa Azure Emulator računalnim](http://go.microsoft.com/fwlink/p/?LinkId=623050)

## <a name="next-steps"></a>Daljnji koraci

[Ispravljanje pogrešaka servise u Oblaku](https://msdn.microsoft.com/library/azure/ee405479.aspx)
