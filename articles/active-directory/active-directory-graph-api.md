<properties
   pageTitle="Azure Active Directory grafikonu API | Microsoft Azure"
   description="Za pregled i brzi početak rada vodič za API grafikonu koji omogućuje programatski pristup Azure AD pomoću krajnje točke REST API-JA."
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Azure Active Directory grafikonu API-JA

> [AZURE.IMPORTANT] Azure AD grafikonu API funkcionalnost je dostupan putem [Programa Microsoft Graph](https://graph.microsoft.io/), objedinjenih API koja obuhvaća API-ji iz druge Microsoftove servise kao što su Outlook, servisa OneDrive, OneNote, Planer i grafikona sustava Office može pristupiti putem jednog krajnjoj točki i s jednom pristup token.

Graph API za Active Directory Azure omogućuje pristup programski Azure AD pomoću krajnje točke REST API-JA. API grafikonu aplikacije mogu se koristiti za izvođenje stvaranje, čitanje, ažuriranje i brisanje operacije (CRUD) u imeniku podatke i objekte. Na primjer, API grafikonu podržava sljedeće uobičajene postupke za korisničkom objektu:

- Stvaranje novog korisnika u imeniku

- Početak korisničke detaljne svojstva, primjerice svoje grupe

- Ažurirajte korisničke svojstva, primjerice svoje mjesto i telefonski broj ili promijenili lozinku

- Provjera članstvo u grupi korisničkog pristupa na temelju uloga

- Onemogućivanje na korisnički račun i izbrisati u potpunosti

Osim korisničke objekte možete izvoditi slične operacije na druge objekte kao što su grupe i aplikacije. Da biste nazvali API grafikonu na direktorij, aplikacija mora biti registriran u Azure AD i biti konfigurirano tako da omogućuje pristup direktoriju. To je obično postići kroz tijek pristanak korisnik ili administrator.

Da biste počeli koristiti Azure grafikonu API za Active Directory, potražite u članku [Vodič za brzi početak rada API grafikonu](active-directory-graph-api-quickstart.md)ili pročitajte dokumentaciju o [interaktivne grafikonu API referentnu dokumentaciju](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).


## <a name="features"></a>Značajke

API grafikonu omogućuje sljedeće značajke:

- **Krajnje točke REST API -JA**: API-JA na grafikonu je RESTful sastoji od krajnje točke koje je moguće pristupiti pomoću standardnih HTTP zahtjeva za uslugu. API grafikonu podržava XML-a ili notaciju Javascript objekta (JSON) vrste sadržaja za zahtjeve i odgovore. Dodatne informacije potražite u članku [Referenca za Azure AD grafikonu REST API-JA](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- **Provjera autentičnosti s Azure AD**: svaki zahtjev za API grafikonu morate moguće provjeriti autentičnost dodavanjem na JSON Web tokena (JWT) u zaglavlju autorizacije zahtjev. Ovaj token je dobiven upućivanje zahtjeva za Azure AD tokena krajnjoj točki i pružanje valjanih se vjerodajnica. Možete koristiti tijek vjerodajnice za klijent OAuth 2.0 ili kod autorizacije dodijeliti tijek dobiti token da biste uputili poziv na grafikonu. Dodatne informacije potražite [OAuth 2.0 u Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

- **Na temelju uloga autorizacije (RBAC)**: sigurnosne grupe koriste se za izvođenje RBAC u grafikonu API-JA. Ako, na primjer, ako želite da biste odredili ima li korisnik pristup određenog resursa, aplikacija možete nazvati operacije [Provjerite članstvo u grupi (korištenje)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) koja se vraća true ili false.

- **Diferencijal upit**: Ako želite da biste potražili promjene u direktoriju između dva razdoblja vrijeme bez potrebe da biste često korištenih upita API grafikonu, možete poslati zahtjev za razlikovno upita. Ta vrsta zahtjev će vratiti samo promjene između prethodni zahtjev razlikovno upita i trenutni zahtjev. Dodatne informacije potražite u članku [Azure AD grafikonu API razlikovno upita](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).

- **Proširenja direktorija**: Ako razvijate aplikacije koje su potrebne za čitanje ili pisanje jedinstvene svojstva za objekte direktorija, registrirali i koristiti kućni broj vrijednosti pomoću API grafikonu. Na primjer, aplikacije potreban je svojstvo Skype ID-a za svakog korisnika, registrirate novo svojstvo u imeniku i bit će dostupan na svaki korisnički objekt. Dodatne informacije potražite u članku [proširenja shema za Azure AD grafikonu API direktorija](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).

- **Secured pomoću dosega dozvola**: AAD grafikonu API izlaže opsega dozvola koje Omogućivanje sigurnog/pristanak pristup podacima AAD i podrške za široku lepezu vrstama klijenata za aplikaciju, uključujući:
 - osobe s korisničkog sučelja koji su dani većem broju pristup podataka putem autorizacija korisnika prijavljeni u (uz prijenos ovlasti)
  - one koje koriste aplikacije – definiranje kontrola pristupa na temelju uloga kao što su klijenti servisa/daemon (aplikacija uloge)

    Uz prijenos ovlasti i aplikacije uloga dozvola opsega predstavljaju ovlast koji prikazuje API grafikonu, a možete zatražio klijentske aplikacije pomoću aplikacije Registracija dozvole za [značajke na portalu Azure klasični](https://manage.windowsazure.com). Klijenti možete provjeriti opsega dozvola dodijeliti ih provjerom opseg ("pronađenim") zahtjeva primljene u token ovlaštenog dozvola za pristup i uloge ("ulogama") zahtjeva za dozvole za aplikaciju uloge. Saznajte više o [opsega dozvola API Azure AD grafikonu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).


## <a name="scenarios"></a>Scenariji

API grafikonu omogućuje mnogo scenarija aplikacije. Najčešći su sljedeće scenarije:

- **Redak poslovne (jednog klijenta) aplikacije**: U ovom scenariju programa enterprise za razvojne inženjere radi tvrtkom ili ustanovom koja sadrži pretplatu na Office 365. Programer sastavlja web-aplikacije koji podržava interakciju s Azure AD za obavljanje zadataka takve dodjeljujete licencu korisniku. Ovaj zadatak zahtijeva pristup API grafikonu da bi Registri za razvojne inženjere u jednom smjernice za aplikaciju u Azure AD i konfigurira za čitanje i pisanje dozvole za API grafikonu. Zatim aplikacija je konfiguriran za korištenje vlastitu vjerodajnice ili one korisnik trenutno prijavu potrebne token da biste uputili poziv API grafikonu.

- **Softver kao servisne aplikacije (više klijentsko)**: U ovom scenariju proizvođači softvera dobavljača (Neovisni) je razvoj nalazi više klijentu web-aplikacije koje nudi značajke upravljanja korisnika za drugim tvrtkama ili ustanovama koje koriste Azure AD. Te su značajke potrebni pristup objektima direktorija, a potrebno aplikacije da biste uputili poziv API grafikonu. Razvojni registrira aplikacije Azure AD konfigurira je obavezan čitanje i pisanje dozvole za API grafikonu te omogućuje vanjskog pristupa, tako da možete drugim tvrtkama ili ustanovama pristanak za korištenje aplikacije u svoje direktorija. Kada korisnik u tvrtki ili ustanovi drugi potvrđuje aplikaciju prvi put, oni se prikazuju pristanak dijaloški okvir s dozvolama zahtijeva aplikacije.  Date pristanak potom dodijelite aplikacije one potrebne dozvole za API grafikonu u korisnikove mape. Dodatne informacije o framework pristanak potražite u članku [Pregled Framework pristanak](active-directory-integrating-applications.md).

## <a name="see-also"></a>Vidi također

[Vodič za brzi početak rada Azure AD grafikonu API-JA](active-directory-graph-api-quickstart.md)

[Dokumentacija AD REST grafikonu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Vodič za Azure Active Directory programiranje](active-directory-developers-guide.md)
