<properties
    pageTitle="Azure AD vanjski pristup metapodataka | Microsoft Azure"
    description="U ovom se članku opisuju dokument metapodataka za vanjski pristup koje objavljuje Azure Active Directory za servise koji prihvaćaju tokeni Azure Active Directory."
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


# <a name="federation-metadata"></a>Vanjski pristup metapodataka

Azure Active Directory (Azure AD) objavljuje vanjski pristup dokumentu metapodataka za servise koji je konfiguriran tako da prihvaća sigurnosnih tokena koje izdaje Azure AD. Oblik dokumenta metapodataka za vanjski pristup je opisano u [Web Services vanjski pristup Language (WS-vanjski pristup) verzija 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), proširuje [metapodataka za 2.0 ORGANIZACIJA sigurnost pridruživanju oznake jezika (SAML)](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Specifične za klijenta i krajnje točke klijentu neovisno metapodataka

Azure AD objavljuje specifične za klijenta i klijentu neovisno krajnje točke.

Krajnje točke specifične za klijenta odnose na određene klijenta. Vanjski pristup specifične za klijent metapodataka sadrži informacije o klijent, uključujući informacije specifične za klijent izdavač i krajnjoj točki. Aplikacije koje ograničiti pristup na jednom klijent koristite krajnje točke specifične za klijenta.

Krajnje točke klijentu neovisno sadrže podatke koji su zajednički klijenata za sve Azure AD. Ove informacije odnosi se na klijenata hostira pri *login.microsoftonline.com* i je zajednički koristiti u svim korisnicima. Krajnje točke klijentu neovisno preporučuje se za više klijentske aplikacije, jer nisu pridružene neki određeni klijenta.

## <a name="federation-metadata-endpoints"></a>Krajnje točke metapodataka za vanjski pristup

Azure AD objavljuje vanjski pristup metapodatke na `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Za **krajnje točke specifične za klijenta**u `TenantDomainName` može biti jedna od sljedećih vrsta:

- Naziv domene registrirane Azure AD klijent, kao što su: `contoso.onmicrosoft.com`.
- Na immutable smjernice za ID-a domene, kao što su `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Za **klijent neovisno krajnje točke**u `TenantDomainName` je `common`. Ovaj dokument sadrži popis samo metapodataka za vanjski pristup elemente koji su zajednički sve klijenata za Azure AD koji se nalaze na login.microsoftonline.com.

Ako, na primjer, možda je krajnje točke specifične za klijent `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Krajnja točka klijentu neovisno je [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Vanjski pristup dokumentu metapodataka možete pogledati tako da upišete URL u pregledniku.

## <a name="contents-of-federation-metadata"></a>Sadržaj vanjski pristup metapodataka

Sljedeći odjeljak sadrži informacije potrebne servisima koje tokena koje izdaje Azure AD.

### <a name="entity-id"></a>ID entitet

U `EntityDescriptor` sadrži element programa `EntityID` atribut. Vrijednost na `EntityID` atribut predstavlja izdavača, odnosno servis sigurnosnih tokena (STS) koja je izdala token. Važno da biste provjerili valjanost izdavač kada primite token je.

Sljedeće metapodataka prikazuje uzorak specifične za klijent `EntityDescriptor` element s programa `EntityID` element.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
ID klijenta na krajnjoj točki klijentu neovisno možete zamijeniti svoj ID klijenta za stvaranje klijentu specifične `EntityID` vrijednost. Rezultat će biti jednaki tokena izdavač. Na strategije omogućuje više klijentske aplikacije za provjeru valjanosti izdavača za zadani klijent.

Sljedeće metapodataka prikazuje uzorak klijentu neovisno `EntityID` element. Napomena, u `{tenant}` je literal ne rezerviranog mjesta.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Tokena potpisnog certifikata

Kada servis primi tokena koje izdaje u klijentu za Azure AD, potpis tokena moguće provjeriti valjanost s ključem potpisivanja koje se objavljuju u dokumentu metapodataka za vanjski pristup. Vanjski pristup metapodataka obuhvaća javno dio certifikate koji se koristi u klijenata za potpisivanje tokena. Bajtova neobrađenog certifikatu obojane na `KeyDescriptor` element. Vrijedi token za potpisivanje certifikata za potpisivanje samo kada vrijednosti u `use` atribut `signing`.

Vanjski pristup dokumentu metapodataka objavljuje Azure AD može imati više tipki potpisa, kao što su kada se Azure AD Priprema da biste ažurirali potpisnog certifikata. Kada dokument metapodataka vanjski pristup obuhvaća više certifikata, servis za provjeru valjanosti tokena treba podržavaju svi certifikati u dokumentu.

Sljedeće metapodataka prikazuje uzorak `KeyDescriptor` element s ključem potpisivanja.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

Na `KeyDescriptor` element pojavljuje se na dva mjesta u dokumentu metapodataka vanjski pristup; u odjeljku WS vanjski pristup specifičnih i SAML određene sekcije. Certifikati objavljenim u obje sekcije će biti jednaki.

U odjeljku WS vanjski pristup specifičnih čitaču metapodataka WS Federacija biste pročitali certifikata iz programa `RoleDescriptor` element s na `SecurityTokenServiceType` vrsta.

Sljedeće metapodataka prikazuje uzorak `RoleDescriptor` element.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

U odjeljku SAML specifične čitač metapodataka WS Federacija biste pročitali certifikata iz programa `IDPSSODescriptor` element.

Sljedeće metapodataka prikazuje uzorak `IDPSSODescriptor` element.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Postoje nema razlike u obliku specifične za klijenta i klijentu neovisno certifikata.

### <a name="ws-federation-endpoint-url"></a>URL WS Federacija krajnje točke

Vanjski pristup metapodataka sadrži URL koji se koristi za Azure AD za jednu prijave i jednu sign-out u WS Federacija protokolu. Pojavit će se ovaj krajnje točke u na `PassiveRequestorEndpoint` element.

Sljedeće metapodataka prikazuje uzorak `PassiveRequestorEndpoint` element za krajnju točku specifične za klijenta.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Za krajnju točku klijentu neovisno URL WS Federacija pojavit će se u krajnju točku WS Federacija kao što prikazuje sljedeći primjer.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML protokol krajnjoj točki URL-a

Vanjski pristup metapodataka sadrži URL-a koji koristi Azure AD za jednu prijave i jednu sign-out u SAML 2.0 protokolu. Obojane te krajnje točke na `IDPSSODescriptor` element.

URL adrese za prijavu i sign-out pojavljuju se u na `SingleSignOnService` i `SingleLogoutService` elemente.

Sljedeće metapodataka prikazuje uzorak `PassiveResistorEndpoint` za krajnju točku specifične za klijenta.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Na sličan način krajnje točke za uobičajene SAML 2.0 protokol krajnje točke se objavljuju u metapodacima vanjski pristup klijentu neovisno kao što prikazuje sljedeći primjer.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
