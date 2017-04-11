<properties 
    pageTitle="Kako dodati kodiranja jedinice" 
    description="Saznajte kako kako dodati kodiranja jedinice s .NET"  
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Upute za promjenu veličine kodiranje s .NET SDK

> [AZURE.SELECTOR]
- [Portal](media-services-portal-scale-media-processing.md )
- [.NET](media-services-dotnet-encoding-units.md)
- [OSTALE](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Pregled

>[AZURE.IMPORTANT] Provjerite je li da biste pregledali [Pregled](media-services-scale-media-processing-overview.md) tema da biste saznali više o skaliranje media obrade temu.
 
Da biste promijenili vrstu rezervirane jedinica i broj kodiranje rezervirane jedinice pomoću .NET SDK, učinite sljedeće:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>Otvorite zahtjev za podršku možete

Prema zadanim postavkama svaki račun Media Services možete mjerilo za najviše 25 šifriranja i 5 na zahtjev strujanje rezervirane jedinice. Možete zatražiti veća ograničenja tako da otvorite zahtjev za podršku možete.

###<a name="open-a-support-ticket"></a>Otvorite zahtjev za podršku možete

Da biste otvorili podršku karata učinite sljedeće:

1. Kliknite [Pomoć](https://manage.windowsazure.com/?getsupport=true). Ako niste prijavljeni u, morat ćete unijeti vjerodajnice.

1. Odaberite pretplatu.

1. U odjeljku Vrsta podršku, odaberite "Tehnički".

1. Kliknite "Stvaranje karata".

1. Odaberite "Servisa Azure Media Services" na popisu proizvoda prikazane na sljedećoj stranici.

1. Odaberite "Problem vrsta" je prikladan za problem.

1. Kliknite Nastavi.

1. Slijedite upute na sljedećoj stranici, a zatim unesite detalje o problem.

1. Kliknite Pošalji da biste otvorili ulaznice.



##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
