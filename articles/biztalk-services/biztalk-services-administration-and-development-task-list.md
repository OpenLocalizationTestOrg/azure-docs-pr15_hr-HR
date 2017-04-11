<properties
    pageTitle="Popis BizTalk Services zadataka administracije i razvoj | Microsoft Azure"
    description="Planiranje i posao olakšali za implementaciju web-servisa Azure BizTalk Services."
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="administration-and-development-task-list-in-biztalk-services"></a>Administracija i popis zadataka za razvoj u BizTalk servisa  

## <a name="getting-started"></a>Početak rada
Kada radite s Microsoftovim servisima za BizTalk Azure, postoje nekoliko lokalnom i u oblaku komponente treba uzeti u obzir. Da bismo započeli, imajte na umu sljedeće tok procesa:  

|Korak|Zaduženog|Zadatak|Srodne veze|
|----|----|----|----|
|1.|Administrator|Stvaranje pretplate Microsoft Azure pomoću Microsoftov račun ili račun tvrtke ili ustanove|[Azure klasični portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)|
|2.|Administrator|Stvaranje i dodjela BizTalk servisa.|[Stvaranje BizTalk servisa pomoću Azure klasični portala](http://go.microsoft.com/fwlink/p/?LinkID=302280)|
|3.|Administrator|Registriranje ili implementacije BizTalk usluge tvrtke|[Prijava i ažuriranje implementacije BizTalk servisa na portalu servisa BizTalk](https://msdn.microsoft.com/library/azure/hh689837.aspx)|
|4.|Administrator|Ako aplikacija koristi BizTalk prilagodnik servisa za povezivanje s lokalnim sustavom za redak specifični za poslovanje (LOB) ili koristi odredišnu temu ili reda čekanja.  Stvaranje naziva Azure Service Bus. Programer dati ovaj prostor naziva, naziv izdavača Bus servisa i vrijednosti ključa izdavač Bus usluge.|[Kako: Stvaranje i izmjena polje naziva servisa Bus servisa](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) i [dobiti naziv izdavača i izdavatelj ključ vrijednosti](biztalk-issuer-name-issuer-key.md)|
|5.|Za razvojne inženjere|Instalirajte SDK i stvaranje projekta BizTalk servisa u Visual Studio.|[Instaliranje servisa Azure BizTalk SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx) i [stvaranje obogaćenih razmjenu krajnje točke na Azure](https://msdn.microsoft.com/library/azure/hh689766.aspx)|
|6.|Za razvojne inženjere|Implementacija usluge BizTalk projekta u funkcioniranju servisa BizTalk hostirane na Azure.|[Implementacija i osvježavanje projekt usluga BizTalk](https://msdn.microsoft.com/library/azure/hh689881.aspx)|
|7.|Administrator|Primjenjuje ako koristite uređivanje.  Možete dodati partnera i stvaranje ugovore na portalu Microsoft Azure BizTalk Services. Kada stvorite ugovor, možete dodati most i/ili pretvaranja stvorio programiranje postavke ugovor.|[Konfiguriranje uređivanje, AS2 i EDIFACT na portalu servisa BizTalk](https://msdn.microsoft.com/library/azure/hh689853.aspx)|
|8.|Administrator|Pomoću portala za Azure klasični praćenje stanja BizTalk servisa, uključujući metriku performansi.|[BizTalk servisa: Kartica nadzorne ploče, Monitor i skaliranje](http://go.microsoft.com/fwlink/p/?LinkID=302281)|
|9.|Administrator|Pomoću portala za servisa Microsoft Azure BizTalk, upravljanje artefakte koriste BizTalk servise i praćenje poruka tijekom obrade most datotekama.|[Pomoću portala za usluge BizTalk](https://msdn.microsoft.com/library/azure/dn874043.aspx)|
|10.|Administrator|Stvaranje sigurnosne kopije plana sigurnosne kopije BizTalk servis.|[Neprekidno poslovanje i oporavak Izrada BizTalk Services](https://msdn.microsoft.com/library/azure/dn509557.aspx) |  
## <a name="next-steps"></a>Daljnji koraci
[Vodiči za i uzorke](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Stvaranje projekta u Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Instaliranje servisa Azure BizTalk SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>Koncepti
[Stvaranje projekta u Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[Uređivanje, AS2 i EDIFACT poruka (tvrtke za tvrtke)](https://msdn.microsoft.com/library/azure/hh689898.aspx)  
## <a name="other-resources"></a>Ostali resursi  
[Dodavanje izvora, odredište i most poruka krajnje točke](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[Saznajte i stvaranje mape za poruke i pretvorbe](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[Putem servisa za prilagodnik BizTalk (te)](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)
