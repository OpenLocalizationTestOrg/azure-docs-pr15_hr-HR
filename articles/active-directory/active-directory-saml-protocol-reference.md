<properties
    pageTitle="Referenca protokol Azure AD SAML | Microsoft Azure"
    description="Ovaj članak sadrži pregled jedinstvene prijave i jedan Sign-Out SAML profili servisa Azure Active Directory."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Kako Azure Active Directory koristi protokol SAML

Protokol koristi SAML 2.0 Azure Active Directory (Azure AD) da biste omogućili aplikacije možete unijeti na jednom za prijave svojim korisnicima. [Jedinstvenu prijavu](active-directory-single-sign-on-protocol-reference.md) i [jednu Sign-Out](active-directory-single-sign-out-protocol-reference.md) SAML profile Azure AD objašnjavaju kako koristiti assertions SAML, protokoli i povezivanja u servis davatelja identiteta.

Protokol SAML potreban je web-mjesto davatelja identiteta (Azure AD) i u okvir za davatelja usluga (aplikaciju) za razmjenu informacija o sami.

Kada se aplikacija registrira Azure AD, proizvođač registrira informacije vezane uz vanjski pristup s Azure AD. To obuhvaća **Preusmjeravanje URI** i **Metapodataka URI** aplikacije.

Azure AD koristi **URI metapodataka** servisa u oblaku za dohvaćanje potpisivanja ključ i odjavite URI servisa u oblaku. Ako aplikacija ne podržava metapodataka URI, programer mora obratite se Microsoftovoj podršci za davanje odjavite URI i potpisivanje ključ.

Azure Active Directory izlaže specifične za klijenta i uobičajenih (klijentu neovisno) jedan prijave i jedan sign-out krajnje točke. URL-ove predstavljaju moguće adresirati mjesta--nisu samo za identifikatore – tako da možete prijeći na krajnjoj točki čitati metapodatke.

 - Krajnja točka specifične za klijent nalazi se na `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  Na <TenantDomainName> predstavlja rezervirano mjesto za naziv domene registrirane ili TenantID GUID klijent za Azure AD. Na primjer, vanjski pristup metapodatke klijentu s domenom contoso.com je pri: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- Krajnja točka klijentu neovisno nalazi se na `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. U tu adresu krajnjoj točki **uobičajenih** pojavit će se, umjesto naziva domene za klijenta ili ID-a.

Informacije o dokumentima za vanjski pristup metapodataka koje objavljuje Azure AD potražite u članku [Metapodataka za vanjski pristup](active-directory-federation-metadata.md).
