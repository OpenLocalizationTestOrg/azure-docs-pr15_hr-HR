<properties
    pageTitle="Što je novo u stogu Azure | Microsoft Azure"
    description="Što je novo u stogu Azure"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>Što je novo u Azure stogu Tehnički pretpregled 2
Ovo izdanje sadrži nove značajke za klijenata i administratore.

## <a name="network"></a>Mreža   
   - [IDN-ovi](azure-stack-understanding-dns-in-tp2.md) nudi Registracija naziva internoj mreži i rješenja za upravljanje pravima na informacije (DNS-Domain Name System) bez dodatna Infrastruktura DNS-a.
   - [Virtualne mreže pristupnika](azure-stack-create-vpn-connection-one-node-tp2.md) pružiti mogućnosti povezivanja VPN-a za Azure ili lokalnog resursa.
   - Korisnički definirane usmjerava mogu usmjeravati mrežni promet putem vatrozida, sigurnost, drugi aparata i ostale servise.
   - Mrežni resursi možete stvoriti iz trgovine.   

## <a name="storage"></a>Prostor za pohranu
 - [Azure redova](https://msdn.microsoft.com/library/dd179353.aspx) Omogući servis pouzdanog i trajni razmjene poruka.
 - [Prostor za pohranu analize](https://msdn.microsoft.com/library/azure/hh343270.aspx) snimanje prostora za pohranu podataka o performansama. Ove podatke možete koristiti praćenje zahtjeve, analizi trendova korištenje i dijagnosticiranje problema s računom za pohranu.
 - Spremište blobova platforme podržava [Dodavanje bloka operacija](https://msdn.microsoft.com/library/azure/mt427365.aspx).
 - Podrška za račun API-JA za pohranu Premium.
 - Smjernice za pohranu službe za uobičajene alate i SDK-ovi, kao što su EŽA Azure, PowerShell, .NET, Python i Java SDK. 
 - [Prostor za pohranu računa zajednički pristup potpis](https://msdn.microsoft.com/library/azure/mt584140.aspx) omogućiti zrnastog delegiranje pristupa servise za pohranu bez potrebe za zajedničko korištenje ključ cijeli račun.  
 - Servise za pohranu koristite [Grupe upravljanih računa servisa](https://technet.microsoft.com/library/hh831477.aspx) za istaknuti vrijednosnice s indirektni niska upravljanje.
 - Možete vratiti Neiskorišteni klijentu resursa na zahtjev.
 - Izbrisane prostora za pohranu računa zadržavanja duljina je konfigurirati.
 - Možete oporaviti izbrisane klijenta za pohranu računa.

## <a name="compute"></a>Izračun
- Možete deallocate virtualnih računala.
- Možete implementirati virtualnog računala proširenja za otklanjanje poteškoća i konfiguraciji upravljanja svrhe.
- Možete promijeniti veličinu diskova virtualnog računala.
- Virtualnim strojevima može imati više mreža sučelja.

### <a name="portal-experience"></a>Sučelje za portala
 - Azure područja stoga su logički jedinica promjenom veličine i upravljanje u stogu Azure. U pretpregledu, možete pregledati podatke na servise kao što su računalnim, mreže i prostora za pohranu po regijama.
 - Sada možete pretpregledati [azure-stogu-updates.md] sučelja (ažuriranja).

## <a name="key-vault"></a>Ključni zbirke ključeva
- [Ključ sigurnog u stogu Azure](azure-stack-kv-intro.md) pruža mogućnost sigurnog upravljanja ključeva i lozinke za oblak aplikacije.
- Možete nadzirati i nadzor korištenja ključa aplikacije i VMs.

## <a name="billing-and-usage"></a>Naplata i korištenje
 - [Naplata i potrošnje API-ji](azure-stack-billing-and-chargeback.md) ponudili podatke na način na koji su servisa potrošena.  
 - Delegirana davatelji omogućiti distributere nudi servisa Azure stogu svojim korisnicima.

## <a name="monitoring-and-health"></a>Nadzor i stanja
 - Nove mogućnosti dostupne u portal i API-ji za nadzor omogućuju doći prikaz i upravljanje upozorenjima o okruženju sustava.  

## <a name="next-steps"></a>Daljnji koraci
- [Razumijevanje Azure stogu PNA arhitekture](azure-stack-architecture.md)      
- [Objašnjenje preduvjeta za implementaciju](azure-stack-deploy.md)
- [Implementacija Azure stogu](azure-stack-run-powershell-script.md)

  
