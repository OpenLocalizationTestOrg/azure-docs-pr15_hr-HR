<properties
   pageTitle="Brzi početak rada za Azure AD Graph API | Microsoft Aure"
   description="Graph API za Active Directory Azure omogućuje pristup programski Azure AD pomoću OData REST API-JA krajnje točke. API grafikonu aplikacije mogu se koristiti za izvođenje stvaranje, čitanje, ažuriranje i brisanje operacije (CRUD) u imeniku podatke i objekte."
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>Brzi početak rada za Azure AD Graph API-JA

Graph API Azure Active Directory (AD) omogućuje pristup programski Azure AD pomoću OData REST API-JA krajnje točke. API grafikonu aplikacije mogu se koristiti za izvođenje stvaranje, čitanje, ažuriranje i brisanje operacije (CRUD) u imeniku podatke i objekte. Na primjer, možete koristiti API grafikonu stvorite novog korisnika, prikaz ili ažuriranje svojstava korisnika, promijeniti korisničke lozinke, provjerite članstvo u grupi za pristup na temelju uloga, onemogućivanje i brisanje korisnika. Dodatne informacije o grafikonu API značajke i scenariji aplikacije potražite u članku [API Azure AD grafikonu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) i [preduvjeti API Azure AD grafikonu](https://msdn.microsoft.com/library/hh974476.aspx). 

> [AZURE.IMPORTANT] Azure AD grafikonu API funkcionalnost je dostupan putem [Programa Microsoft Graph](https://graph.microsoft.io/), objedinjenih API koja obuhvaća API-ji iz druge Microsoftove servise kao što su Outlook, servisa OneDrive, OneNote, Planer i grafikona sustava Office može pristupiti putem jednog krajnjoj točki i s jednom pristup token.

## <a name="how-to-construct-a-graph-api-url"></a>Upute za sastavljanje grafikonu API URL-a

U grafikonu API-JA da biste pristupili direktorija podatke i objekte (drugim riječima, resursa ili entiteti) prema kojima želite izvršiti CRUD operacije, možete koristiti URL-ovi koji se temelji na protokola Open Data (OData). URL-ovi koriste u grafikonu API sastojati od četiri glavna dijela: servisa korijen, identifikator klijent, put resursa i mogućnosti niza upita: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Iskoristite primjer sljedeći URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

- **Servis korijenski**: Azure AD grafikonu API-JA, korijenski servis je u uvijek https://graph.windows.net.
- **Identifikator klijentu**: to može biti naziv provjerene domene (registriranu), u primjeru iznad contoso.com. Može biti i ID objekta za klijenta ili "myorganiztion" ili "mi" pseudonim. Dodatne informacije potražite u članku [adresiranja entiteti i postupke u grafikonu API -JA](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
- **Put resursa**: u ovom odjeljku URL-a označava resurs biti komunicirao (korisnike, grupe, određenom korisniku, ili u određenoj grupi itd.) U gornjem primjeru je najviše razine "grupe" na adresu koju postavite resursa. Možete i navesti određeni entitet, primjerice "korisnika / {ID objekta}" ili "korisnika na userPrincipalName".
- **Parametri upita**:? dijeli odjeljak put resursa iz odjeljka parametara upita. Parametar upita "api-verzije" potreban je na sve zahtjeve u grafikonu API-JA. API grafikonu podržava i sljedeće mogućnosti upita OData: **$filter**, **$orderby**, **proširite $**, **$top**i **$format**. Sljedeće mogućnosti upita trenutno nisu podržani: **$count**, **$inlinecount**i **$skip**. Dodatne informacije potražite u članku [podržane upita, filtre, i označavanje stranica mogućnosti u grafikonu Azure AD API-JA](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graph API verzija

Odredite verzije za zahtjev za API na grafikonu u parametra upita "api-verzije". Za verziju 1,5 i noviji, koristite verziju numeričke vrijednosti; API-verzija = 1.6. Za starije verzije, koristite datumski niz koji se pridržava oblik gggg-MM-DD; na primjer, api-verzija = 2013-11-08. Za pretpregled značajke, koristite niz "beta;" na primjer, api-verzija = beta. Dodatne informacije o razlike između verzija API grafikonu potražite u članku [Azure AD grafikonu API za rad s verzijama](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Grafikon API metapodataka

Da biste vratili datoteku metapodataka API grafikonu dodajte segment "$metadata" nakon klijentu identifikatora u URL-a za primjer, sljedeći URL vraća metapodataka za tvrtku pokazni videozapis koristi Explorer grafikonu: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Možete unijeti URL u adresnu traku web-preglednik da biste vidjeli metapodatke. Dokument metapodataka CSDL vraća opisuju entiteti i složene vrste, njihova svojstva i funkcije i akcije koji prikazuje verziju API grafikonu koji ste tražili. Ispuštanje parametar api-verzija će vratiti metapodataka za najnoviju verziju.

## <a name="common-queries"></a>Zajednički upiti

[Uobičajene upite za Azure AD grafikonu API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) popis uobičajenih upite koji se mogu koristiti u sklopu Azure AD grafikonu, uključujući upite koji se mogu koristiti za pristup najviše razine resursa u direktoriju te za izvođenje operacije u direktoriju.

Na primjer, `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` vraća podataka za contoso.com imenik tvrtke.

Ili `https://graph.windows.net/contoso.com/users?api-version=1.6` popis svih objekata korisnika u imeniku contoso.com.

## <a name="using-the-graph-explorer"></a>Pomoću programa Explorer grafikonu

Explorer Graph za API Azure AD grafikon možete koristiti upit podatke directory tijekom stvaranja aplikacija.

> [AZURE.IMPORTANT] Graph Explorer ne podržava pisanje ili brisanje podataka iz imenika. Možete izvršiti samo čitanju operacije u direktoriju Azure AD pomoću programa Explorer grafikonu.

Slijedi izlaz vidjeti su dođite do Graph Explorer, odaberite koristi poduzeća i unesite `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` za prikaz svih korisnika u imeniku pokazni videozapis:

![Azure AD explorer api grafikonu](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Učitavanje Explorer grafikonu**: da biste učitali alata, dođite do [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/). Kliknite **Tvrtka koristi pokazni videozapis** da biste pokrenuli Explorer grafikonu podataka iz uzorka klijenta. Ne morate vjerodajnice da biste koristili tvrtke pokazni videozapis. Osim toga, kliknite **prijavite se** i prijavite se pomoću vjerodajnica za račun za Azure AD pokrećete Graph Explorer za vaš klijent. Ako pokrenete Graph Explorer protiv vlastiti klijent, vi ili vaš administrator morat ćete pristanak prilikom prijave. Ako imate pretplatu na Office 365, automatski imati klijent za Azure AD. Vjerodajnice koje koristite za prijavu u Office 365 su, zapravo Azure AD računi i koristite te vjerodajnice pomoću programa Explorer grafikonu.

**Izvođenje upita**: da biste pokrenuli upit, upišite upit u tekstni okvir zahtjev i kliknite **PREUZMI** ili kliknite **Unesi** ključ. Rezultati prikazuju se u okviru odgovor. Na primjer, `https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6` prikazat će se popis svih grupa objekata u direktoriju pokazni videozapis.

Imajte na umu sljedeće značajke i ograničenja programa Explorer grafikona:
- Mogućnost Samodovršavanja na skupova resursa. Da biste vidjeli, kliknite **Koristi poduzeća** , a zatim kliknite tekstni okvir zahtjev (gdje URL tvrtke pojavljuje). Možete odabrati resursa postavite s padajućeg popisa.

- Podržava "ja" i "myorganization" adresiranja pseudonima. Na primjer, možete koristiti `https://graph.windows.net/me?api-version=1.6` da biste se vratili u korisničkom objektu prijavljeni korisnik ili `https://graph.windows.net/myorganization/users?api-version=1.6` da biste se vratili svi korisnici u trenutnom direktoriju. Imajte na umu da "ja" pseudonim pomoću vraća pogrešku za tvrtku pokazni videozapis jer ne postoji prijavljeni korisnik upućivanje zahtjeva.

- Sekcije zaglavlja odgovor. To se može koristiti za pomoć pri otklanjanju poteškoća koji se pojavljuju prilikom pokretanja upita.

- JSON preglednik za odgovor s mogućnostima za proširivanje i sažimanje.

- Nema podrške za prikaz minijatura slika.

## <a name="using-fiddler-to-write-to-the-directory"></a>Korištenje Fiddler pisanja u direktorij

Za potrebe ovog vodiča za brzi početak rada, možete koristiti Fiddler Web program za ispravljanje pogrešaka da biste vježbali predstavljanja "pisanje" operacija na temelju koje Azure AD direktorija. Dodatne informacije i instalirati Fiddler potražite u članku [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

U primjeru u nastavku da biste stvorili novi sigurnosne grupe 'MyTestGroup' u direktoriju Azure AD koristit će Fiddler Web ispravljanje.

**Nabavljanje pristupnog tokena**: da biste pristupili Azure AD Graph, klijenti su potrebne za uspješno provjeru autentičnosti Azure AD prvi put. Dodatne informacije potražite u članku [Provjera autentičnosti scenariji za Azure AD](active-directory-authentication-scenarios.md).

**Sastavljanje i pokretanje upita**: poduzeti sljedeće korake.

1. Otvorite program za ispravljanje pogrešaka Fiddler Web i prijeđite na karticu **Skladatelj** .
2. Budući da želite stvoriti novu sigurnosnu grupu, odaberite **Objavi** kao način HTTP povlačite na padajućem izborniku. Dodatne informacije o operacije i dozvole na objekta grupe potražite u članku [grupe](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) u [grafikonu Azure AD REST API Reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. U okvir pokraj **članka**, upišite u nastavku kao zahtjev za URL: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.

    > [AZURE.NOTE] Morate zamijeniti mytenantdomain s nazivom domene Azure AD imenik.

4. U polju neposredno ispod objave povlačite prema dolje, upišite sljedeće:

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] SUBSTITUTE vaše &lt;token za pristup&gt; s token za pristup za Azure AD direktorija.

5. U polju **zahtjev tijelo** upišite sljedeće:

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    Dodatne informacije o stvaranju grupe potražite u članku [Stvaranje grupe](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Dodatne informacije o Azure AD entiteti i vrste koje se koji prikazuje grafikon i informacije o operacije koje možete izvršiti na njima s grafikonu potražite u članku [Azure AD grafikonu REST API Reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o [API Azure AD grafikonu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- Dodatne informacije o [opsega dozvola API Azure AD grafikonu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
