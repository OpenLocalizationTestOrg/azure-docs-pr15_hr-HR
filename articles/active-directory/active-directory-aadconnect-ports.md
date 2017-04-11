<properties
    pageTitle="Azure AD Connect: Priključci | Microsoft Azure"
    description="Ta je stranica na stranicu Tehničke reference priključke koji moraju biti otvoreni za Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-required-ports-and-protocols"></a>Identitet hibridnog obavezno priključci i protokoli
Sljedeći dokument je Tehnička referenca da biste dobili informacije na potreban priključci i protokoli koji su potrebni za implementaciju rješenja za identitet hibridnog. Korištenje na sljedećoj slici i uputite odgovarajuće tablice.

![Što je Azure AD Connect](./media/active-directory-aadconnect-ports/required1.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tablica 1 - Azure AD povezati i lokalnog servisa Active Directory
U ovoj su tablici opisuju priključci i protokoli koji su potrebni za komunikaciju između poslužitelj za Azure AD Connect i lokalnog servisa Active Directory.

Protokol | Priključci | Opis
--------- | --------- |---------
DNS-A|53 (TCP/UDP)| DNS pretraživanja na skup stabala odredište.
Kerberos|88 (TCP/UDP)| Provjera autentičnosti Kerberos za skup stabala AD.
MS RPC |135 (TCP/UDP)| Koristi se tijekom početne konfiguracije čarobnjaka za Azure AD Connect kada ga povezuje skup stabala AD.
LDAP|389 (TCP/UDP)| Koristi se za uvoz podataka iz servisa Active Directory. Podaci šifriraju Kerberos znak & nepouzdano.
LDAP/SSL|636 (TCP/UDP)| Koristi se za uvoz podataka iz servisa Active Directory. Prijenos podataka je potpisan i šifrirana. Koristiti samo ako koristite SSL.
RPC |49152 do 65535 (slučajno visoke RPC Port)(TCP/UDP)| Koristi se tijekom početne konfiguriranja Azure AD Connect kada ga povezuje šuma AD. Dodatne informacije potražite u [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017)i [KB224196](https://support.microsoft.com/kb/224196) .

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Povezivanje tablice 2 – Azure AD i Azure AD
U ovoj su tablici opisuju priključci i protokoli koji su potrebni za komunikaciju između poslužitelj za Azure AD Connect i Azure AD.

Protokol |Priključci |Opis
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Služi za preuzimanje CRL-ovi (popisi opozvanih certifikata) da biste provjerili SSL certifikata.
HTTPS|443(TCP/UDP)| Koristi se za sinkronizaciju s Azure AD.

Popis URL-ovi i IP adrese morate otvoriti u vatrozidu, pročitajte članak [Office 365 URL-ovi i rasponi IP adresa](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-federation-serverswap"></a>Povezivanje tablice 3 – Azure AD i poslužitelji WAP vanjski pristup
U ovoj su tablici opisuju priključci i protokoli koji su potrebni za komunikaciju između Azure AD Connect poslužitelj i vanjski pristup/WAP poslužitelja.  

Protokol |Priključci |Opis
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Služi za preuzimanje CRL-ovi (popisi opozvanih certifikata) da biste provjerili SSL certifikata.
HTTPS|443(TCP/UDP)| Koristi se za sinkronizaciju s Azure AD.
WinRM|5985| Ga Slušatelj WinRM

## <a name="table-4---wap-and-federation-servers"></a>Tablica 4 - WAP i vanjskim poslužiteljima
U ovoj su tablici opisuju priključci i protokoli koji su potrebni za komunikaciju među vanjskim poslužiteljima i WAP poslužitelja.

Protokol |Priključci |Opis
--------- | --------- |---------
HTTPS|443(TCP/UDP)| Koristi se za provjeru autentičnosti.

## <a name="table-5---wap-and-users"></a>Tablica 5 - WAP i korisnika
U ovoj su tablici opisuju priključci i protokoli koji su potrebni za komunikaciju između korisnika i WAP poslužitelja.

Protokol |Priključci |Opis
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Koristi se za provjeru autentičnosti uređaja.
TCP|49443 (TCP)| Koristi se za provjeru autentičnosti certifikata.

## <a name="table-6a--6b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tablica 6a & 6b – povežite zdravlje Azure AD agent za (AD FS/Sync) i Azure AD
Tablice u nastavku opisuju krajnje točke, priključci i protokoli koji su potrebni za komunikaciju između agente Azure AD povežite zdravlje i Azure AD

### <a name="table-6a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tablica 6a - priključci i protokoli za Azure AD povežite zdravlje agent za (AD FS/Sync) i Azure AD
U ovoj su tablici opisuju na sljedeće izlazni priključci i protokoli koji su potrebni za komunikaciju između agenata Azure AD povežite zdravlje i Azure AD.  

Protokol |Priključci  |Opis
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Izlazni
Azure Service Bus|5671 (TCP/UDP)| Izlazni

### <a name="6b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>6b - krajnje točke za Azure AD povežite zdravlje agent za (AD FS/Sync) i Azure AD
Popis krajnjih točaka, u odjeljku [preduvjeti za agent za Azure AD povezati stanja](active-directory-aadconnect-health-agent-install.md#requirements).
