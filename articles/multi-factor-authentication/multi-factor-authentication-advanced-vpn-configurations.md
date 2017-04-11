<properties
    pageTitle="Napredne scenarije s Azure višestruke provjere autentičnosti i 3 zabavu VPN-ovi"
    description="Ova stranica sadrži informacije o konfiguraciji detaljnog postavljanja za Azure MFA s 3 prodcuts proizvođača."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban" 
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-3rd-party-vpn"></a>Napredne scenarije s Azure višestruke provjere autentičnosti i 3 zabavu VPN-a
Azure višestruka provjera autentičnosti mogu se jednostavno povezati s raznim 3 strana VPN rješenja.  To obuhvaća potražite Cisco® ASA VPN, potražite Citrix NetScaler SSL VPN i tvrtke Juniper mreža sigurnog pristupa/servisom Pulse sigurne povezivanje sigurne SSL VPN-a potražite.

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA VPN potražite i Azure višestruka provjera autentičnosti
Azure višestruka provjera autentičnosti jednostavno integrira vaš uređaj Cisco® ASA VPN-a nudi dodatne sigurnosti za VPN® Cisco AnyConnect prijave i pristupa portalu.  To možete učiniti pomoću protokola LDAP ili POLUMJER.  Odaberite nešto od sljedećeg da biste preuzeli vodilice detaljne detaljne konfiguracije.

Vodič za konfiguraciju  | Opis
------------- | ------------- |
[Cisco ASA konfiguraciji Anyconnect VPN-a i Azure MFA za LDAP](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Vaš uređaj Cisco ASA VPN jednostavno integrirati MFA Azure pomoću LDAP|
[Cisco ASA konfiguraciji Anyconnect VPN-a i Azure MFA za POLUMJER](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Vaš uređaj Cisco ASA VPN jednostavno integrirati MFA Azure pomoću POLUMJER

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN i Azure višestruka provjera autentičnosti
Azure višestruka provjera autentičnosti jednostavno integrira u vašem Citrix NetScaler SSL VPN uređaj da biste prikazali dodatne sigurnosti Citrix NetScaler SSL VPN prijave i pristupa portalu.  To možete učiniti pomoću protokola LDAP ili POLUMJER.  Odaberite nešto od sljedećeg da biste preuzeli vodilice detaljne detaljne konfiguracije.

Vodič za konfiguraciju  | Opis
------------- | ------------- |
[Citrix NetScaler SSL VPN i Azure MFA konfiguracije za LDAP](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Integracija sustava Citrix NetScaler SSL VPN integrirati Azure MFA potražite pomoću LDAP|
[Citrix NetScaler SSL VPN i Azure MFA konfiguracije za POLUMJER](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Vaš uređaj Citrix NetScaler SSL VPN jednostavno integrirati MFA Azure pomoću POLUMJER

##<a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Web-mjesto tvrtke Juniper/servisom Pulse sigurne SSL VPN potražite i u okvir za Azure višestruka provjera autentičnosti
Azure višestruka provjera autentičnosti jednostavno integrira vaše tvrtke Juniper/servisom Pulse sigurne SSL VPN uređaj da biste prikazali dodatne sigurnosti tvrtke Juniper/servisom Pulse sigurne SSL VPN prijave i pristupa portalu.  To možete učiniti pomoću protokola LDAP ili POLUMJER.  Odaberite nešto od sljedećeg da biste preuzeli vodilice detaljne detaljne konfiguracije.

Vodič za konfiguraciju  | Opis
------------- | ------------- |
[Web-mjesto tvrtke Juniper/servisom Pulse sigurne SSL VPN-a i u okvir za Azure MFA konfiguracije za LDAP](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx)| Jednostavno vaše tvrtke Juniper/servisom Pulse sigurne SSL VPN integrirati Azure MFA potražite pomoću LDAP|
[Web-mjesto tvrtke Juniper/servisom Pulse sigurne SSL VPN-a i u okvir za Azure MFA konfiguracije za POLUMJER](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Vaš uređaj tvrtke Juniper/servisom Pulse sigurne SSL VPN jednostavno integrirati MFA Azure pomoću POLUMJER
