<properties
    pageTitle="Azure jedan Odjava protokol SAML | Microsoft Azure"
    description="U ovom se članku opisuju jednu protokol SAML Sign-Out u servisu Azure Active Directory"
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
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="single-sign-out-saml-protocol"></a>Jedan Sign-Out SAML protokol

Azure Active Directory (Azure AD) podržava SAML 2.0 web-pregledniku jedan sign-out profil. Za jednu sign-out da biste funkcionira ispravno, Azure AD morate registrirati njezin URL metapodataka tijekom Registracija aplikacija. Azure AD s metapodacima dohvaća odjavite URL-a i tipku potpisivanja servis u oblaku. Azure AD koristi tipku potpisivanja da biste provjerili potpis na dolazne LogoutRequest i koristi na LogoutURL za preusmjeravanje korisnika kad su odjavljeni.

Ako servis u oblaku ne podržava na krajnjoj točki metapodataka kada se aplikacija registrira, programer mora obratite se Microsoftovoj podršci odjavite URL i potpisivanja ključ.

Ovaj dijagram prikazuje tijek rada Azure AD jedan sign-out postupka.

![Jedan Odjava tijeka rada](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

Servis šalje oblak na `LogoutRequest` poruku Azure AD da biste naznačili da je prekinuta sesije. Sljedeći isječak prikazuje uzorak `LogoutRequest` element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

Na `LogoutRequest` element koji se šalju Azure AD zahtijeva sljedećim atributima:

- `ID`: To označava sign-out zahtjev. Vrijednost `ID` moraju započinjati brojem. Uobičajeni praksa je da biste dodali **id** prikaz niza GUID.

- `Version`: Postavite vrijednost ovog elementa **2,0**. Potreban je tu vrijednost.

- `IssueInstant`: To je na `DateTime` niz vrijednost koordiniranje univerzalno vrijeme (UTC) i [round-trip oblik ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD očekuje vrijednost ove vrste, ali provesti.

- U `Consent`, `Destination`, `NotOnOrAfter` i `Reason` atribute zanemaruju se ako se nalaze na `LogoutRequest` element.

### <a name="issuer"></a>Izdavač

Na `Issuer` element u na `LogoutRequest` mora u potpunosti odgovarati nešto **ServicePrincipalNames** u servis u oblaku u Azure AD. Obično to je postavljeno na **Aplikaciju ID URI** koji je naveden tijekom Registracija aplikacija.

### <a name="nameid"></a>NameID

Vrijednost na `NameID` element mora u potpunosti odgovarati na `NameID` korisnika koji je u tijeku odjavljeni.
## <a name="logoutresponse"></a>LogoutResponse

Azure AD šalje na `LogoutResponse` u odgovoru na `LogoutRequest` element. Sljedeći isječak prikazuje uzorak `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure AD skupove na `ID`, `Version` i `IssueInstant` vrijednosti u na `LogoutResponse` element. Njome se postavlja u `InResponseTo` element vrijednosti u `ID` atribut u `LogoutRequest` koji elicited odgovor.

### <a name="issuer"></a>Izdavač

Azure AD postavlja tu vrijednost `https://login.microsoftonline.com/<TenantIdGUID>/` gdje <TenantIdGUID> je ID klijenta za Azure AD klijenta.

Da biste procijenili vrijednost u `Issuer` element, koristi vrijednost **Aplikacije ID URI** vode Registracija aplikacija.

### <a name="status"></a>Status

Azure AD koristi u `StatusCode` element u na `Status` element da biste naznačili uspjelo ili nije od sign-out. Kada sign-out pokušaj ne uspije, u `StatusCode` element može sadržavati prilagođene poruke o pogrešci.
