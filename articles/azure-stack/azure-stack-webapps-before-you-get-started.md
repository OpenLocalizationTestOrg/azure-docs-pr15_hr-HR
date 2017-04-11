<properties
    pageTitle="Azure stogu aplikacije servisa za Tehnički pretpregled 1 prije nego što počnete | Microsoft Azure"
    description="Korake da biste dovršili prije no što implementirate Web Apps na hrpu Azure"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>Prije nego što počnete s Azure stogu web-aplikacije

> [AZURE.NOTE] Sljedeće informacije odnosi se samo na Azure stogu TP1 implementacije.

Potrebno je nekoliko stavki da biste instalirali Azure stogu web-aplikacije:

- Dovršeni implementacije [Azure stogu Tehnički pretpregled 1](azure-stack-run-powershell-script.md)
- U sustavu Azure stog za small implementacije Azure stogu web-aplikacije dovoljno prostora.  Potreban prostor je otprilike 20GB prostora na tvrdom disku.
- [Poslužitelj sa sustavom SQL Server](#SQL-Server).

>[AZURE.NOTE] Sljedeći koraci sve odvijaju na VM klijenta.

## <a name="before-you-deploy-azure-stack-web-apps"></a>Prije implementacije Azure stogu web-aplikacije

Da biste implementirali davatelja resursa, morate pokrenuti na PowerShell integrirani skriptiranje Environment(ISE) kao administrator. Zbog toga morate dopustiti kolačiće i JavaScript u pregledniku Internet Explorer profilom koji koristite za prijavu na Azure Active Directory.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Isključivanje preglednika Internet Explorer Poboljšana sigurnost

1.  Prijavite se na računalo (PNA) za Azure stogu dokaz pojam kao **AzureStack/administrator**, a zatim otvorite **Upravitelj poslužitelja**.
2.  Isključite **Poboljšana konfiguracija sigurnosti** administratorima i korisnicima.
3.  Prijavite se u ClientVM.AzureStack.local virtualnog računala kao administrator, a zatim otvorite **Upravitelj poslužitelja**.
4.  Isključite **Poboljšana konfiguracija sigurnosti** administratorima i korisnicima.

## <a name="enable-cookies"></a>Omogućivanje kolačića

1.  Odaberite **Start**, > **sve aplikacije**> **Pomagala za Windows**. Desnom tipkom miša kliknite **Internet Explorer** > **Dodatne** > **Pokreni kao administrator**.
2.  Ako se od vas zatraži, odaberite **Koristi preporučene sigurnost**, a zatim odaberite **u redu**.
3.  U pregledniku Internet Explorer odaberite **Alati** (ikonu zupčanika), > **Internetske mogućnosti** > **izjave o zaštiti privatnosti** > **Dodatno**.
4.  Odaberite **Napredno**. Provjerite je li odabrano oba potvrdna okvira **prihvaćanje** , a zatim odaberite **u redu** , a **u redu** ponovno.
5.  Zatvorite Internet Explorer, a zatim ponovno pokrenite Očisti filtar kao administrator.

## <a name="install-the-latest-version-of-azure-powershell"></a>Instalirajte najnoviju verziju Azure PowerShell

1.  Prijavite se na računalo Azure stogu PNA kao **AzureStack/administrator**.
2.  Korištenje udaljene radne površine da biste se prijavili ClientVM.AzureStack.local virtualnog računala kao administrator.
3.  Otvorite **Upravljačku ploču** , a zatim odaberite **Deinstaliraj program**. 
4.  Desnom tipkom miša kliknite **Microsoft Azure PowerShell – 2015 studenom** , a zatim odaberite **Deinstaliraj**.
5.  Nakon što instalirate deinstalirajte završi, ponovno ClientVM.AzureStack.local virtualnog računala
6.  Preuzmite i instalirajte najnovije [Azure PowerShell](http://aka.ms/azstackpsh).


## <a name="sql-server"></a>SQL Server

Prema zadanim postavkama, installer Azure stogu web-aplikacije postavljen da biste koristili SQL Server koji je instaliran uz davatelja resursa za SQL Azure stogu. Kada instalirate davatelja resursa SQL Server (SQL to) provjerite je li vam uzeti u obzir baze podataka administratorskog korisničkog imena i lozinke. Kada instalirate Azure stogu web-aplikacije potrebno oboje.
Napomena: I imat ćete mogućnost implementacije i koristite neki drugi poslužitelj za pokretanje sustava SQL Server. Možete odabrati instancu sustava SQL Server da biste koristili kada dovršite mogućnosti installer Azure stogu web-aplikacije.
