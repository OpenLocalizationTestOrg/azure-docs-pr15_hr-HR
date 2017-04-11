<properties
    pageTitle="Azure AD popis kompatibilnosti vanjski pristup"
    description="Ova stranica sadrži davatelji identiteta drugih proizvođača koji se mogu koristiti za implementaciju jedinstvenu prijavu."
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
    ms.date="09/12/2016"
    ms.author="billmath"/>

# <a name="azure-ad-federation-compatibility-list"></a>Azure AD popis kompatibilnosti vanjski pristup
Azure Active Directory nudi jedinstvene prijave na i poboljšane sigurnosti programa access aplikacije za Office 365 i drugi Microsoft Online services za hibridno i samo oblak implementacije bez svakog rješenja drugih proizvođača. Office 365, kao što je većina Microsoftovim internetskim servisima, integriran s Azure Active Directory za imeničkim servisima, provjere autentičnosti i autorizacije. Azure Active Directory nudi i jedinstvenu prijavu na tisuće SaaS aplikacije i lokalne web-aplikacije. Pogledajte u galeriju aplikacija Azure Active Directory za podržane aplikacije SaaS.

Tvrtkama ili ustanovama koje su uložiti u rješenja drugih proizvođača Federacija, ova tema sadrži upute za konfiguriranje jedinstvene prijave za svoje korisnike Windows Server Active Directory s Microsoft Online services pomoću davatelja identiteta trećih s "Azure Active Directory federation kompatibilnosti popisa" ispod. 


![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Grupa računala Oxford](http://oxfordcomputergroup.com/)drugog proizvođača, Microsoft, testirati ove jedan prijave sučelja davatelji identiteta proizvođača uspoređuje sa skupom slučajeva korištenje uobičajenih pomoću servisa Azure Active Directory.

Informacije o kako davatelja identiteta treće strane ovdje naveden, obratite se Oxford računala na [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com).

>[AZURE.IMPORTANT] Grupa računala Oxford testirati samo za vanjski pristup funkcionalnost te prijavu scenarija jedinstvene. Grupa računala Oxford jeste li izvođenje testiranja sinkronizacije, dvostruka provjera autentičnosti, komponenti te prijavu scenarija jedinstvene itd.

>Korištenje prijavu tako da svaki drugi ID-a s UPN-a ne i testirati u ovaj program.



- [Azure Active Directory](#azure-active-directory)
- [Optimalnih IDM virtualne identiteta poslužitelja Federation Services](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [PingFederate 7,2](#pingfederate-72) 
- [PingFederate 8.x](#pingfederate-8x)
- [Centrify](#centrify) 
- [IBM Tivoli vanjskog Upravitelj identiteta 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
- [CA SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
- [RadiantOne CFS 3.0](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [Upravitelj pristupom NetIQ 4.0.1](#netiq-access-manager-401) 
- [VELIKI IP s VELIKI IP Upravitelj pravila pristupom ver. 11.3 x – 11.6 x](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [Portal za radni prostor VMware verzija 2.1](#vmware-workspace-portal-version-21) 
- [Prijavite se i otvorite 5,3](#signampgo-53) 
- [Vanjski pristup za IceWall verzije 3.0](#icewall-federation-version-30) 
- [CA sigurne oblaka](#ca-secure-cloud) 
- [V7.1 Dell jedan Upravitelj pristup Oblaku identiteta](#dell-one-identity-cloud-access-manager-v71) 
- [Prijava AuthAnvil jedne 4,5](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Budući da to su proizvode treće, Microsoft ne nudi podršku za implementacije, konfiguracije, otklanjanje poteškoća, najbolje prakse, itd. problemi i pitanja vezana uz ove davatelji identiteta. Podrška i pitanja vezana uz ove davatelji identiteta, zatražite od podržanih stranama treće izravno.

>Ove davatelji identiteta treće strane su testirati komunikaciji s Microsoftovim pomoću WS Federacija i protokoli WS pouzdanost samo servisima u oblaku. Testiranje niste uključili pomoću protokola SAML.

## <a name="azure-active-directory"></a>Azure Active Directory 
Azure Active Directory možete provjeru autentičnosti korisnika po združivanju s s lokalnog aktivno-imenika ili bez lokalnog poslužitelja vanjski pristup pomoću sinkronizaciju lozinke. 

Ovo je matricu scenarija podrška za ovaj sučelje za prijavu: 


| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Ništa|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|
|Moderna aplikacije koje se koriste ADAL kao što je Office 2016| Podržana|Ništa|

Dodatne informacije o korištenju Azure Active Directory AD fs potražite u članku [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

Dodatne informacije o korištenju Azure Active Directory za sinkronizaciju lozinke potražite u članku [Azure AD Connect](active-directory-aadconnect.md).


## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimalne IDM virtualne identiteta poslužitelja Federation Services 
Optimalnih IDM virtualne identiteta poslužitelja Federation Services možete provjeru autentičnosti korisnika koji se nalaze u klijenata lokalni Active direktorija.

Ovo je scenarij podržava matrice ovaj jedan za prijave:


| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Provjera autentičnosti integrirani sustava Windows|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Za dodatne informacije o klijentski pristup pravila potražite u članku [ograničavanje pristupa za Office 365 usluge na temelju lokacije klijenta.](https://technet.microsoft.com/library/hh526961.aspx)|



## <a name="pingfederate-611"></a>PingFederate 6.11 

PingFederate 6.11 implementira široku vanjski pristup WS identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:


| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Ništa (starije verzije morate nadograditi 6.11|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

PingFederate upute o konfiguriranju to STS pružanje jedan sučelje za prijavu za korisnike servisa Active Directory, preuzmite pdf [ovdje.](http://go.microsoft.com/fwlink/?LinkID=266321)

## <a name="pingfederate-72"></a>PingFederate 7,2 
PingFederate 7,2 implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:


| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Ništa|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Upute PingFederate kako konfigurirati ovaj STS možete unijeti jednu sučelje za prijavu korisnika servisa Active Directory potražite u članku [ovdje.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## <a name="pingfederate-8x"></a>PingFederate 8.x 
PingFederate 8.x implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:


| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Ništa|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Upute PingFederate kako konfigurirati ovaj STS možete unijeti jednu sučelje za prijavu korisnika servisa Active Directory potražite u članku [ovdje.](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="centrify"></a>Centrify 
Centrify pridonosi odabiru s pridruženim jedan prijave za Office 365 bez preduvjet nalaze na vanjskom poslužitelju lokalnog.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:


| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Ništa|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Kontrola pristupa klijent nije podržana 

Dodatne informacije o Centrify potražite u članku [ovdje.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli vanjskog Upravitelj identiteta 6.2.2 
IBM Tivoli vanjskog Upravitelj identiteta 6.2.2 programa IBM sigurnosti programa Access Manager for 1.4 aplikacije Microsoft implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave: 

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Ništa|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o IBM Tivoli omogućila vanjski pristup identitetu upravitelja, potražite u članku [IBM sigurnosti programa Access Manager for Microsoft Applications.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedan sučelje za prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave: 

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Ništa|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o SecureAuth potražite u članku [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12.52 SP1 kumulativne izdanje 4
CA SiteMinder vanjski pristup 12.52 implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave: 

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Ništa|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o CA SiteMinder potražite u članku [CA SiteMinder Federacija.](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0 
RadiantOne oblaka vanjski pristup usluge (CFS) 3.0 implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave: 

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Provjera autentičnosti integrirani sustava Windows|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o RadiantOne CFS potražite u članku [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## <a name="okta"></a>Okta 
Okta implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave: 


| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Integrirano provjeru autentičnosti sustava Windows zahtijeva postavljanje dodatnih web-poslužitelj i Okta aplikacije.|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Provjera autentičnosti integrirani sustava Windows|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o Okta potražite u članku [Okta.](https://www.okta.com/)
 
## <a name="onelogin"></a>OneLogin 
OneLogin kao testirati u svibnja 2014 implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave: 

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Provjera autentičnosti integrirani sustava Windows|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Provjera autentičnosti integrirani sustava Windows|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o OneLogin potražite u članku [OneLogin.](https://www.onelogin.com/)

## <a name="netiq-access-manager-401"></a>Upravitelj pristupom NetIQ 4.0.1 
Upravitelj pristupom NetIQ 4.0.1 implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |* Podržani Kerberos ugovora|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Nije podržano integriranu provjeru autentičnosti sustava Windows|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

* NetIQ podržava provjeru autentičnosti Kerberos putem konfiguracije Kerberos ugovora.  Za pomoć za tu konfiguraciju, obratite se NetIQ ili prikaz vodič za postavljanje. Dodatne informacije o upravitelju za NetIQ Access potražite u članku [Upravitelj pristupom NetIQ.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>VELIKI IP s ver. VELIKI IP Upravitelj pravilima programa Access 11.3 x – 11.6 x 
VELIKI-IP s pristupom pravilnika za upravljanje (APM) ver. VELIKI IP 11.3 x – 11.6 x implementira široku standardna identiteta SAML jedan sučelje za prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave: 

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Nije podržano |Nije podržano|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o upravitelju pravila VELIKI IP Access potražite u članku [Upravitelj pravila pristupom za VELIKI IP.](https://f5.com/products/modules/access-policy-manager) 

Upravitelj pravila pristupom za VELIKI IP upute o konfiguriranju to STS pružanje jedan sučelje za prijavu za Active Directory Users preuzimanje pdf [ovdje.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## <a name="vmware--workspace-portal-version-21"></a>Portal za radni prostor VMware verzija 2.1 
Portal za radni prostor VMware verzija 2.1 implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Nije podržano integriranu provjeru autentičnosti sustava Windows|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM | Podržana |Nije podržano integriranu provjeru autentičnosti sustava Windows|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o VMware radnog prostora Portal verzija 2.1 preuzeli pdf [ovdje.](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)

## <a name="signgo-53"></a>Prijavite se i otvorite 5,3 
Prijavite se i otvorite 5.3 primjenjuje široku identiteta WS vanjski pristup/WS-pouzdanost standardne jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ugovori Kerberos podržana |
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM | Podržana |Ništa|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|


Znak & Idi 5,3 podržava provjeru autentičnosti Kerberos putem konfiguracije Kerberos ugovora.  Za pomoć za tu konfiguraciju, obratite se Ilex ili prikaz vodič za postavljanje [ovdje.](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)


## <a name="icewall-federation-version-30"></a>Vanjski pristup za IceWall verzije 3.0 
IceWall vanjski pristup verzije 3.0 implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Nije podržano integriranu provjeru autentičnosti sustava Windows|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM | Podržana |Nije podržano integriranu provjeru autentičnosti sustava Windows|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o vanjski pristup IceWall potražite [ovdje](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) i [ovdje.](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="ca-secure-cloud"></a>CA sigurne oblaka 

Oblak sigurne CA implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Nije podržano integriranu provjeru autentičnosti sustava Windows|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM | Podržana |Nije podržano integriranu provjeru autentičnosti sustava Windows|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o oblaka sigurne CA potražite u članku [CA sigurne oblaka.](http://www.ca.com/us/products/security-as-a-service.aspx)

## <a name="dell-one-identity-cloud-access-manager-v71"></a>V7.1 Dell jedan Upravitelj pristup Oblaku identiteta 
Dell jedan oblaka pristup Upravitelj identiteta implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Ništa|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM |  Podržana |Ništa|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|

Dodatne informacije o Dell jedan oblaka pristup Upravitelj identiteta potražite u članku [Dell jedan oblaka pristup Upravitelj identiteta.](http://software.dell.com/products/cloud-access-manager)

 Upute za konfiguriranje ovaj STS možete unijeti jednu sučelje za prijavu korisnika sustava Office 365, potražite u članku [Konfiguriranje korisnike sustava Office 365.](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365) 

## <a name="authanvil-single-sign-on-45"></a>Prijava AuthAnvil jedne 4,5 
AuthAnvil jedan znak na 4,5 implementira široku WS vanjski pristup/WS-pouzdanost identiteta standardu jedinstvenu prijavu i framework atribut sustava exchange.

Ovo je matricu scenarija podrška za ovaj jedan za prijave:

| Klijent |Podrška  |Iznimke|
| --------- | --------- |--------- |
| Koji se temelji na web klijentima poput Exchange Web Access i SharePoint Online | Podržana |Nije podržano integriranu provjeru autentičnosti sustava Windows|
| Obogaćeni klijentskim aplikacijama kao što je Lync, pretplata na Office, a zatim CRM | Podržana |Nije podržano integriranu provjeru autentičnosti sustava Windows|
| Klijenti za e-pošte obogaćenog kao što su Outlook i ActiveSync |  Podržana |Ništa|


Dodatne informacije potražite u članku [AuthAnvil jedinstvenu prijavu.](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)
