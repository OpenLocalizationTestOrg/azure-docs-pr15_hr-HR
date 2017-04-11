<properties
    pageTitle="Stvaranje prostor naziva Bus servis pomoću portala za Azure | Microsoft Azure"
    description="Za početak rada s Bus servisa, trebat će vam prostor naziva. Evo kako ga možete stvoriti pomoću portala za Azure."
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="jotaub"/>

# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Stvaranje polja naziva Bus servis pomoću portala za Azure

Prostor naziva je uobičajenih spremnik za sve razmjenu komponente. Više redova i teme mogu se nalaziti u jednom prostora za naziv, a zatim se prostori naziva često služe kao spremnika aplikacije. Trenutno postoje dva načina da biste stvorili prostor naziva Bus servisa.

1.  Azure portal (Ovaj članak)

2.  [Voditelj resursa predložaka][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Stvaranje prostor naziva na portalu za Azure

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Čestitamo! Sada ste stvorili polje naziva servisa Bus poruka.

## <a name="next-steps"></a>Daljnji koraci

Pogledajte naš [GitHub uzoraka] [ github-samples] koji pokazuju neke dodatne značajke Bus Azure usluge razmjene poruka.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples