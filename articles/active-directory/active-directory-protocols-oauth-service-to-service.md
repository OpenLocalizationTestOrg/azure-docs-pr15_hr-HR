<properties
    pageTitle="Azure AD servis za servis provjera autentičnosti pomoću OAuth2.0 | Microsoft Azure"
    description="U ovom se članku opisuje kako implementirati provjeru autentičnosti servis za servis pomoću grant tijek OAuth2.0 klijent vjerodajnice pomoću HTTP poruke."
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

# <a name="service-to-service-calls-using-client-credentials"></a>Servis za pozive za servis pomoću vjerodajnica za klijenta

Na OAuth 2.0 klijentskog vjerodajnice Grant tijeka omogućuje web-servisa ( *povjerljive klijenta*) da biste koristili vlastiti vjerodajnice za provjeru autentičnosti prilikom pozivanja drugih servisa za web, umjesto oponaša korisnika. U ovom scenariju klijent je obično srednjem sloju web-servisa, daemon servisa ili web-mjesta.

## <a name="client-credentials-grant-flow-diagram"></a>Vjerodajnice za klijent dodijeliti dijagrama toka

Na sljedećem su dijagramu objašnjava kako dodijeliti vjerodajnice za klijent tijek funkcioniranja u Azure Active Directory (Azure AD).

![Klijent OAuth2.0 vjerodajnice tijek Grant](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. Klijentska aplikacija potvrđuje krajnjoj tokena izdavanja Azure AD i zahtijeva token za pristup.
2. Krajnja točka za Azure AD tokena izdavanja problemi token za pristup.
3. Pristupni token koristi se za provjeru autentičnosti zaštićenim resursa.
4. Podaci iz zaštićenim resursa se vraćaju u web-aplikaciji.

## <a name="register-the-services-in-azure-ad"></a>Registrirati servisa Azure AD

Registrirajte pokrenuti servis i primanju servisa Azure Active Directory (Azure AD). Detaljne upute potražite u članku [Dodavanje, ažuriranje, i uklanjanje aplikacije](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>Zahtjev za Token za pristup

Da biste zatražili token za pristup, koristite HTTP POST na klijentu specifične Azure AD krajnjoj točki.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Tokena zahtjeva za pristup za servis za servis

Na servis za servis tokena zahtjeva za pristup sadrži sljedećih parametara.

| Parametar | | Opis |
|-----------|------|------------|
| response_type | obavezno | Navodi vrstu tražene odgovor. Ta vrijednost mora biti u tijeku Grant vjerodajnice za klijenta, **client_credentials**.|
| client_id | obavezno | Određuje id klijenta za Azure AD pozivanja web-usluge. Da biste pronašli ID klijenta pokrenuti aplikaciju, na portalu za upravljanje Azure, kliknite **Servisa Active Directory**, kliknite imenika, pritisnite aplikaciju i kliknite **Konfiguriraj**.|
| client_secret | obavezno |  Unesite ključ koji je registriran za pozivanja web-servisa u Azure AD. Da biste stvorili ključ, na portalu za upravljanje Azure, kliknite **Servisa Active Directory**, kliknite imenika, pritisnite aplikaciju i kliknite **Konfiguriraj**. |
| resurs | obavezno | Unesite ID URI aplikacije primanju web-usluge. Da biste pronašli ID URI aplikacije na portalu za upravljanje Azure, kliknite **Servisa Active Directory**, kliknite imenika, pritisnite aplikaciju i kliknite **Konfiguriraj**. |

## <a name="example"></a>Primjer

Sljedeće HTTP POST zahtjeve token za pristup za web-servis https://service.contoso.com/. Na `client_id` služi za identifikaciju web-servisa koji zahtijeva token za pristup.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Servis za servis pristup tokena odgovor

Odgovor za uspjeh sadrži odgovor JSON OAuth 2.0 pomoću sljedećih parametara.

| Parametar   | Opis |
|-------------|-------------|
|access_token |Token tražene programa access. Upućivanje poziva web-servisa možete koristiti ovaj token za provjeru autentičnosti primanju web-servisa. |
|access_type  | Označava vrijednost tokena vrste. Samo vrsta s podrškom za Azure AD je **nošenja**. Dodatne informacije o nošenja tokeni potražite u [OAuth 2.0 autorizacije Framework: korištenje tokena nošenja (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).
|expires_in   | Koliko dugo token za pristup vrijedi (u sekundama).|
|expires_on   |Vrijeme isteka token za pristup. Datum predstavljen kao broj sekundi iz 1970-01-01T0:0:0Z UTC-a dok vrijeme isteka. Ta vrijednost koristi se za određivanje vijek predmemorirani tokena. |
|resurs     | ID URI aplikacije primanju web-usluge. |

## <a name="example"></a>Primjer

Sljedeći primjer prikazuje uspjeh odgovor na zahtjev za token za pristup web-uslugu.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Vidi također

* [OAuth 2.0 u Azure AD](active-directory-protocols-oauth-code.md)
