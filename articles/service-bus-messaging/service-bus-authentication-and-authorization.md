<properties 
    pageTitle="Usluge provjere autentičnosti Bus i autorizacije | Microsoft Azure"
    description="Pregled provjere autentičnosti zajednički pristup potpis (SAS)."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="service-bus-authentication-and-authorization"></a>Bus usluge provjere autentičnosti i autorizacije

Aplikacije možete provjeriti autentičnost Azure servisa Bus pomoću provjere autentičnosti ili zajednički pristup potpis (SAS) ili preko Azure Active Directory kontrole pristupa (poznat i kao servisa za kontrolu pristupa ili ACS). Zajednički pristup potpis provjere autentičnosti omogućuje aplikacije za provjeru autentičnosti Bus servis pomoću tipkovnog konfigurirali prostor za naziv ili onaj s kojim su povezane određena prava. Taj ključ možete koristiti za stvaranje tokena zajednički pristup potpis koji klijenti mogu koristiti za provjeru autentičnosti Bus servisa.

>AZURE. Važno SAS preporučuje se putem ACS, kao što je nudi shemu jednostavni, fleksibilne i jednostavno pomoću provjere autentičnosti za Bus servisa. SAS aplikacije mogu se koristiti u slučajevima u kojima ne moraju da biste upravljali notion ovlaštenog "korisnika."

## <a name="shared-access-signature-authentication"></a>Zajednički pristup potpis za provjeru autentičnosti

[Provjera autentičnosti SAS](service-bus-sas-overview.md) omogućuje vam da biste dodijelili korisničkog pristupa resursima servisa Bus određena prava. Provjera autentičnosti SAS u Bus servisa uključuje konfiguracije ključa šifriranja s pridružene pravima na servis Bus resursa. Klijenti pa može ostvariti pristup resursa tako da prezentaciju SAS token koji se sastoji od resursa URI pristupa, a zatim je isteka prijavljeni pomoću tipku konfiguriran.

Tipke za SAS možete konfigurirati na servis Bus prostora za naziv. Tipku primjenjuje se na sve razmjenu entiteti u tog naziva. Tipke možete konfigurirati i na servis Bus redovima i teme. SAS podržano je i na servis Bus preusmjeri.

Da biste koristili SAS, možete konfigurirati [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) objekt na prostor naziva, reda čekanja ili temu koja se sastoji od sljedećeg:

- *KeyName* koja služi za identifikaciju pravilo.

- *Primarni ključ* je šifrirana ključ koristi za prijavu/provjeru SAS tokena.

- *SecondaryKey* je šifrirana ključ koristi za prijavu/provjeru SAS tokena.

- *Prava* na koji predstavlja skup Preslušavanja, slanje ili upravljanje pravima Odobreno.

Pravila za provjeru autentičnosti konfiguriran na razini prostor naziva može dati pristup Svi entiteti u prostor naziva za klijente s tokeni prijavljeni pomoću odgovarajućeg ključa. Do 12 takve autorizacije pravila može se konfigurirati na servis Bus prostor naziva, reda čekanja ili temu. Prema zadanim postavkama [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) ima sva prava je konfiguriran za svaki prostor naziva kada prvi put dodjeli.

Da biste pristupili entitet, klijent zahtijeva SAS tokena koje generira pomoću određenih [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). SAS token generira HMAC SHA256 niza resursa koji se sastoji od resursa URI-JA na kojoj se pristupa potraživati i do isteka pomoću ključa šifriranja pridružene pravilo za provjeru autentičnosti.

Podrška za provjeru autentičnosti SAS za servis Bus je uvršteno na Azure .NET SDK verzije 2.0 ili novijim. SAS uključuje podršku za [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). API-ji za sve koji prihvatiti niza za povezivanje kao parametar obuhvaćaju podršku za nizove SAS veze.

## <a name="acs-authentication"></a>Provjera autentičnosti ACS

Provjere autentičnosti Bus servisa putem ACS upravlja se putem na pomoćnom "-sb" ACS prostor naziva. Ako želite pomoćnom ACS prostor naziva će biti stvoren za servis Bus prostora za naziv, ne možete stvoriti servisa Bus prostor naziva pomoću Azure klasični portala; morate stvoriti pomoću cmdleta ljuske PowerShell [Novo AzureSBNamespace](https://msdn.microsoft.com/library/azure/dn495165.aspx) prostora za naziv. Ako, na primjer:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Da biste izbjegli stvaranje programa ACS prostor naziva, problema sljedeću naredbu:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Ako, na primjer, ako stvorite servisa Bus prostor naziva pod nazivom **contoso.servicebus.windows.net**, pomoćnom ACS prostor naziva pod nazivom **contoso sb.accesscontrol.windows.net** je dodjeli automatski. Za svi se prostori naziva stvorenih prije kolovoz 2014., popratnu ACS prostor naziva i stvorena.

Zadanog identiteta servisa "vlasnik" ima sva prava, dodjeli je po zadanom u ACS imenski suradnika. Možete dobiti preciznije kontrolu za svaki servis Bus entitet putem ACS konfiguriranjem odgovarajuće pouzdanim odnosima. Možete konfigurirati identiteta dodatnih servisa za upravljanje pristupom entiteti Bus servisa.

Da biste pristupili entitet, klijent zahtijeva token za SWT iz ACS s odgovarajućim zahtjevima tako da prezentaciju njegov vjerodajnice. SWT token mora se mogu poslati kao dio zahtjev tržišta servisa za da biste omogućili autorizacija klijenta za pristup entitet.

Podrška za provjeru autentičnosti ACS za servis Bus je uvršteno na Azure .NET SDK verzije 2.0 ili novijim. Ova provjera autentičnosti uključuje podršku za [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx). API-ji za sve koji prihvatiti niza za povezivanje kao parametar obuhvaćaju podršku za nizove ACS veze.

## <a name="next-steps"></a>Daljnji koraci

Nastavite čitanje [provjere autentičnosti zajednički pristup potpis sa servisa Bus](service-bus-shared-access-signature-authentication.md) dodatne pojedinosti o SAS.

Više razine pregled SAS u servis Bus potražite u članku [Zajedničko korištenje programa Access potpisa](service-bus-sas-overview.md).

Možete pronaći dodatne informacije o ACS tokena u [Kako: od ACS putem protokola za PRELAMANJE OAuth zatražiti Token](https://msdn.microsoft.com/library/hh674475.aspx).



