<properties 
   pageTitle="Ispravljanje pogrešaka servise u Oblaku Azure | Microsoft Azure"
   description="Ispravljanje pogrešaka servisa Azure oblaka"
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

# <a name="debugging-cloud-services"></a>Ispravljanje pogrešaka servise u oblaku

Možete koristiti različite postupke za ispravljanje pogrešaka Azure aplikacije pomoću alata za Azure za Microsoft Visual Studio i Azure SDK:

- Možete ispraviti pogreške Azure aplikacije Visual Studio kada radite, kao što biste bilo koju aplikaciju Visual C# ili Visual Basic. Dodatne informacije potražite u članku [ispravljanje pogrešaka u oblaku na lokalnom računalu](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

- Dijagnostika Azure možete koristiti da biste se prijavili detaljne informacije iz kod koji se izvodi unutar uloge, hoće li uloge se izvode u okruženje za razvoj ili Azure. Dodatne informacije potražite u članku [Prikupljanje zapisivanje podataka korištenjem dijagnostike Azure](http://go.microsoft.com/fwlink/p/?LinkId=400450).

- Ako koristite Visual Studio Enterprise pisati uloge ciljani pri 4 za .NET Framework ili 4,5 za .NET Framework, možete omogućiti IntelliTrace trenutku implementacije Azure oblaka servisa Visual Studio. IntelliTrace sadrži zapis koji koristite za Visual Studio za ispravljanje pogrešaka aplikacije kao da je pokrećete u Azure. Dodatne informacije potražite u članku [objavljene oblaku s IntelliTrace i Visual Studio za ispravljanje pogrešaka]( http://go.microsoft.com/fwlink/p/?LinkId=623016).

- Možete omogućiti daljinsko uklanjanje programskih pogrešaka na servise u oblaku u trenutku kada Implementacija servisa u oblaku s Visual Studio. Ako odaberete da biste omogućili daljinsko uklanjanje programskih pogrešaka za implementaciju, servise udaljene pogrešaka su instalirani na virtualnim strojevima koji se izvode svaku instancu uloge. Tih servisa, kao što su msvsmon.exe, utjecati na performanse ili rezultirati dodatnih troškova. Dodatne informacije potražite u članku [servis u oblaku u Azure za ispravljanje pogrešaka](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).



