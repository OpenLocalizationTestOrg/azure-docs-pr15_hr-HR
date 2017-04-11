<properties
    pageTitle="Azure jedine prijave SAML protokol | Microsoft Azure"
    description="U ovom se članku opisuju protokol za jedan znak na SAML servisa Azure Active Directory"
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

# <a name="single-sign-on-saml-protocol"></a>Jedan protokol SAML za prijavu

U ovom se članku opisuje SAML 2.0 provjere autentičnosti zahtjeve i odgovore koji podržava Azure Active Directory (Azure AD) za jedinstvenu prijavu.

Protokol dijagramu u nastavku opisuju jedan niz za prijavu. Servis u oblaku (davatelj usluga) koristi za povezivanje HTTP preusmjeravanje za prosljeđivanje na `AuthnRequest` element (zahtjev za provjeru autentičnosti) za Azure AD (davatelja identiteta). Zatim Azure AD koristi programa HTTP post povezivanje da biste je objavili na `Response` element na servis u oblaku.

![Jedine prijave tijeka rada](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Da biste zatražili provjere autentičnosti korisnika, oblaka usluge slanja programa `AuthnRequest` element Azure AD. Uzorak SAML 2.0 `AuthnRequest` može izgledati ovako:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parametar | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| ID-A | obavezno | Azure AD atribut koristi za popunjavanje u `InResponseTo` atribut vraćeni odgovor. ID-a mora započeti s brojem, pa prepend niz kao što su "id" za predstavljanje niza GUID uobičajenih strategije. Na primjer, `id6c1c178c166d486687be4aaf5e482730` je valjan ID-a. |
| Verzija | obavezno | Trebali biste **2.0**.|
| IssueInstant | obavezno | Ovo je niz DateTime vrijednost UTC-a i [round-trip oblik ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD očekuje vrijednost datuma i vremena te vrste, ali neće rezultirati ili upotrijebite vrijednost. |
| AssertionConsumerServiceUrl | Neobavezno | Ako je navedena to moraju se podudarati s `RedirectUri` od servisa u oblaku u Azure AD. |
| ForceAuthn | Neobavezno | Ako je navedena to mora biti false. Bilo koja vrijednost uzrokuje pogrešku.|
| IsPassive | Neobavezno | Ako je navedena to mora biti false. Bilo koja vrijednost uzrokuje pogrešku. |  

Svi ostali `AuthnRequest` atributi, kao što su dozvole, odredište, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex i ProviderName se **zanemaruju**.

Azure AD i zanemaruje se `Conditions` element u `AuthnRequest`.

### <a name="issuer"></a>Izdavač

Na `Issuer` element u programa `AuthnRequest` mora u potpunosti odgovarati nešto **ServicePrincipalNames** u servis u oblaku u Azure AD. Obično to je postavljeno na **Aplikaciju ID URI** koji je naveden tijekom Registracija aplikacija.

Na primjer SAML isječak koji sadrži na `Issuer` element izgleda ovako:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Taj element zahtjeve obliku ID određeni naziv u odgovoru i nisu obavezni u `AuthnRequest` elemente koji se šalju Azure AD.

Uzorak `NameIdPolicy` element izgleda ovako:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Ako `NameIDPolicy` se isporučuje možete uključiti njegov neobavezno je `Format` atribut. Na `Format` atribut može imati samo jedan od sljedećih vrijednosti; bilo koja vrijednost rezultira pogreškom.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory problemi zahtjeva NameID kao para identifikator.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory problemi zahtjeva NameID u obliku adrese e-pošte.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Tu vrijednost omogućuje Azure Active Directory da biste odabrali oblik zahtjeva. Azure Active Directory problemi s NameID kao para identifikator.

Ne uključuj se `SPNameQualifer` atribut. Zanemaruje Azure AD na `AllowCreate` atribut.

### <a name="requestauthncontext"></a>RequestAuthnContext

Na `RequestedAuthnContext` element određuje u željenom metoda provjere autentičnosti. To nije obavezno u `AuthnRequest` elemente koji se šalju Azure AD. Azure AD podržava samo jedan `AuthnContextClassRef` vrijednost: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Određivanje opsega

Na `Scoping` element koji sadrži popis davatelja identiteta, nije obavezno u `AuthnRequest` elemente koji se šalju Azure AD.

Ako je navedena uključiti u `ProxyCount` atribut `IDPListOption` ili `RequesterID` element, kao što je nisu podržane.

### <a name="signature"></a>Potpis

Nemojte uvrstiti u `Signature` element u `AuthnRequest` elemenata, kao što je Azure AD ne podržava prijavljeni zahtjeva za provjeru autentičnosti.

### <a name="subject"></a>Predmet

Azure AD zanemaruje se `Subject` element `AuthnRequest` elemente.

## <a name="response"></a>Odgovor

Kada se dovrši uspješno je tražene prijave, Azure AD objava odgovor na servis u oblaku. Ogledni odgovor na uspješan pokušaj prijave izgleda ovako:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Odgovor

Na `Response` element uključuje rezultat zahtjev za provjeru autentičnosti. Azure AD skupove na `ID`, `Version` i `IssueInstant` vrijednosti u na `Response` element. Postavlja sljedećim atributima:

- `Destination`: Prilikom prijave dovršava uspješno, ovo je postavljeno na `RedirectUri` davatelja usluga (u oblaku).
- `InResponseTo`: Ovo je postavljeno na `ID` atribut u `AuthnRequest` element koji se pokreće odgovor.

### <a name="issuer"></a>Izdavač

Azure AD skupove na `Issuer` element `https://login.microsoftonline.com/<TenantIDGUID>/` gdje <TenantIDGUID> je ID klijenta za Azure AD klijenta.

Na primjer, odgovor uzorka elementom izdavač može izgledati ovako:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Status

Na `Status` element ostavlja uspjelo ili nije prijave. Obuhvaća na `StatusCode` element koji sadrži kod ili ugniježđene šifre koji predstavljaju status zahtjeva. Uključuje i na `StatusMessage` element koji sadrži prilagođeni poruke o pogreškama koje generiraju tijekom postupka prijave.

<!-- TODO: Add a authentication protocol error reference -->

Slijedi odgovor SAML neuspješnih pokušaja prijave.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Pridruživanju

Uz na `ID`, `IssueInstant` i `Version`, Azure AD postavlja sljedeće elemente u `Assertion` element odgovora.

#### <a name="issuer"></a>Izdavač

Ovo je postavljeno `https://sts.windows.net/<TenantIDGUID>/`gdje <TenantIDGUID> je ID klijenta za Azure AD klijenta.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Potpis

Azure AD potpisuje pridruživanju u odgovoru na uspješno prijave. Na `Signature` element sadrži digitalni potpis koji servis u oblaku možete koristiti za provjeru autentičnosti izvorišnog web-mjesta da biste provjerili integritet tipkom pridruživanju.

Da biste generirali digitalni potpis, Azure AD koristi tipku potpisa u na `IDPSSODescriptor` element svog metapodataka dokumenta.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Predmet

Određuje glavnicu koji je predmet izvješća u tipkom pridruživanju. Sadrži na `NameID` element koji predstavlja korisnika čija je autentičnost provjerena. Na `NameID` ciljano identifikator koji je smjer samo s davateljem usluga koja je namijenjena token je vrijednost. Je stalni – možete opozvati, ali će se nikad ne ponovno dodijeliti. Preporučuje se i neprozirne, u tom je otkrivanje ništa o korisniku i ne mogu se koristiti kao identifikator za atribut upita.

Na `Method` atribut u `SubjectConfirmation` element uvijek je postavljeno na `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Uvjeta

Taj element navodi uvjete koji definiraju prihvatljiva korištenje SAML assertions.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

Na `NotBefore` i `NotOnOrAfter` atribute navedite razdoblje tijekom kojeg je valjan tipkom pridruživanju.

- Vrijednost na `NotBefore` atribut jednak do ili malo (manje od jedne sekunde) kasnije vrijednost `IssueInstant` atribut u `Assertion` element. Azure AD računa za bilo koje vrijeme razliku između sa samim sobom i servis u oblaku (davatelj usluga) i dodati sve međuspremnik ovaj put.
- Vrijednost na `NotOnOrAfter` je atribut 70 minuta kasnije od vrijednosti u `NotBefore` atribut.

#### <a name="audience"></a>Ciljne skupine

Sadrži URI koja služi za identifikaciju ciljnu skupinu. Azure AD postavlja vrijednost ovog elementa vrijednosti `Issuer` elementa u `AuthnRequest` koja pokreće prijavite se na. Da biste procijenili na `Audience` vrijednost, upotrijebite vrijednost u `App ID URI` koji je naveden tijekom Registracija aplikacija.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Kao što su u `Issuer` vrijednosti, u `Audience` vrijednost mora u potpunosti odgovarati jedno od naziva glavni servisa predstavlja servisa u oblaku u Azure AD. Međutim, ako vrijednost u `Issuer` element nije vrijednost URI u `Audience` je vrijednost u odgovoru na `Issuer` vrijednost mjestu s `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Sadrži zahtjevima o predmetu ili korisnika. Sljedeći isječak sadrži uzorka `AttributeStatement` element. Tri točke upućuje na to da se element mogu sadržavati više atributa i vrijednosti atributa.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **Zahtjev za naziv** : vrijednost u `Name` atributa (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) kao što je korisničko ime za upravitelja korisnika čija je autentičnost provjerena `testuser@managedtenant.com`.
- **ObjectIdentifier zahtjeva** : vrijednost u `ObjectIdentifier` atributa (`http://schemas.microsoft.com/identity/claims/objectidentifier`) je na `ObjectId` direktorija objekta koji predstavlja korisnika čija je autentičnost provjerena u Azure AD. `ObjectId`je programa immutable, globalno jedinstven i iskoristite sigurni identifikator korisnika čija je autentičnost provjerena.

#### <a name="authnstatement"></a>AuthnStatement

Taj element nametanja da predmet pridruživanju je autentičnost provjerena prema određenom srednje vrijednosti u određenom trenutku.

- Na `AuthnInstant` atribut, Azure AD određuje vrijeme autentičnost korisnika.
- Na `AuthnContext` element određuje kontekst provjere autentičnosti za provjere autentičnosti korisnika.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
