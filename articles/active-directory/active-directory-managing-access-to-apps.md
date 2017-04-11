<properties
  pageTitle="Upravljanje pristupom pomoću Azure AD |  Microsoft Azure"
  description="U članku se opisuje kako Azure Active Directory omogućuje tvrtke ili ustanove da biste odredili aplikacije kojem svaki korisnik ima pristup."
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>Upravljanje pristupom aplikacije

Upravljanje stalnog pristupa, korištenje procjenu i izvješćivanje o pogreškama i dalje biti u test nakon aplikacije je integriran u vašoj tvrtki ili ustanovi identitet sustava. U mnogim slučajevima, administratori ili služba za morati poduzeti tijeku aktivna uloga u upravljanju pristup aplikacijama. Dodjela ponekad se izvoditi Općenito ili odjela IT timom. Često je namijenjen odluka dodjele biti delegirana odluka maker tvrtke potrebno njihovo odobrenje prije nego što čini IT dodjelu.  Drugim tvrtkama ili ustanovama ulažete uz integraciju s postojećeg automatiziranog identiteta i pristup upravljanja sustavom, kao što su na temelju uloga kontrole pristupa (RBAC) ili utemeljenu na atribut kontrole pristupa (ABAC). Integracija i razvoj pravilo često specijalizirane i skupi. Nadzor ili izvješćivanje o pogreškama na svakom pristupu upravljanje je vlastitu zasebne, skup i složene ulaganja.

## <a name="how-does-azure-active-directory-help"></a>Kako pridonosi Azure Active Directory

 Azure AD podržava upravljanje opsežne pristup za konfigurirani aplikacije Omogućivanje tvrtke ili ustanove da biste jednostavno postigli pravilnike za pristup desno od dodjele automatsko, atributa (ABAC ili RBAC scenariji) kroz delegiranje i uključujući administrator upravljanja. S Azure AD možete jednostavno postigli složene pravila Kombiniranje više modela upravljanja za jednu aplikaciju, a možete čak i ponovno koristiti pravila upravljanja preko aplikacije s istom ciljne skupine.

 - [Dodavanje novih ili postojećih aplikacija](active-directory-sso-integrate-saas-apps.md)


 Dodjela aplikacije Azure AD usredotočuje se na dva načina primarni dodjele:

- **Pojedinačne dodjele** Administrator s dozvolama globalnog administratora direktorija možete odabrati pojedinačne korisničke račune i im dodijeliti pristup aplikaciji.
- **Grupne dodjele (plaćeni Azure AD)** Administrator s dozvolama globalnog administratora direktorija možete dodijeliti grupi aplikacija. Pristup određene korisnike određuje jesu li članovi grupe u trenutku pokušaju pristupiti aplikacije. Drugim riječima, administrator učinkovito stvarali pravilo dodjele koja kaže "bilo koji član dodijeljenog grupi trenutno ima pristup aplikaciji". Pomoću ove mogućnosti dodjele administratori može koristiti s bilo kojeg Azure AD grupe mogućnosti upravljanja, uključujući [utemeljen na atribut dinamički grupe](active-directory-accessmanagement-manage-groups.md), vanjskog sustava grupe (na primjer, lokalnog imeničkog servisa Active Directory ili dana) ili grupe kojima upravlja Administrator ili samostalno-service upravlja. Jedne grupe možete jednostavno dodijeliti više aplikacije jamči da aplikacija pomoću afiniteta dodjele možete omogućiti zajedničko korištenje pravila za dodjelu smanjivanje složenosti cjelokupan upravljanje. Uzmite u obzir tu ugniježđenu grupu za grupne dodjele aplikacijama trenutno nisu podržani članstva.

Pomoću ovih dodjele dva načina rada, administratori možete postići bilo koji način upravljanja poželjno dodjele.

S Azure AD korištenje i izvješćivanje o dodjele nije posve integriran, omogućivanje administratorima jednostavno izvješće na stanje dodjele, dodjele pogrešaka i čak i korištenje.

## <a name="complex-application-assignment-with-azure-ad"></a>Dodjela složene aplikacije s Azure AD

Razmislite o programa kao što su Salesforce. U mnogo tvrtkama ili ustanovama Salesforce prvenstveno koristi prodajnim i marketinškim tvrtke ili ustanove. Često članovi tima za marketing iznimno imati ovlasti pristup Salesforce, dok članovi prodajnog tima imaju ograničen pristup. U mnogim slučajevima općenite populacije zaposlenici zaduženi za podatke ste ograničili pristup aplikaciji. Iznimke ta pravila zakomplicirati stvari. Često je prerogative vodstvo timovima marketinga ili prodaje da biste dodijelili korisničkog pristupa ili promijeniti njihove uloge nezavisno od tih generički pravila.

Kod Azure AD aplikacije kao što su Salesforce može biti unaprijed konfigurirana za jedinstvenu prijavu (SSO) i automatiziranog dodjele resursa. Kada aplikacija je konfiguriran, Administrator može potrajati jednokratni akcija stvaranje i dodjela odgovarajuće grupe. U ovom primjeru administrator može izvršiti sljedeće zadatke:

- [Dinamični grupe](active-directory-accessmanagement-manage-groups.md) može se definirati automatski predstavlja svim članovima prodajnim i marketinškim timovima pomoću atribute kao što su odjel ili uloga:

    - Svi članovi grupa marketinške želite dodijeliti "marketinške" uloga u Salesforce

    - Svi članovi prodajnog tima grupe želite dodijeliti ulogu "Prodaja" Salesforce. Daljnje sužavanja može koristiti više grupa koje predstavljaju regionalne prodaje timovima dodijeliti različite uloge od prodaje.

- Da biste omogućili mehanizam iznimku, grupu samostalno se može stvoriti za svaku ulogu. Ako, na primjer, grupi "Salesforce marketing iznimke" moguće grupno samostalno. Grupe koje se mogu dodijeliti ulogu zainteresiranih Salesforce i marketing tim vodstvo kakvih vlasnici. Članovi tima za marketing vodstvo nije moguće dodavanje i uklanjanje korisnika, postavljanje pravilnika za uključivanje ili čak i odobravanje ili odbijanje zahtjeva za pojedinačne korisnike da biste se pridružili. Podržan je kroz pruža odgovarajuće tempiranja informacije koje je potrebno specijalizirane obuka za vlasnika i članove.

U ovom slučaju, svi korisnici dodijeljeni će biti automatski dodijeljena da Salesforce, kako se dodaju na različitim grupama njihove Dodjela uloge će se ažurirati Salesforce. Korisnici će moći da biste otkrili i pristup Salesforce putem Microsoft aplikacije programa access ploče, Office web-klijentima ili čak i tako da odete na svoje tvrtke ili ustanove stranicu za prijavu Salesforce. Administratori mogu bi jednostavno pregled korištenja i dodjela statusa pomoću Azure AD izvješćivanje.

Administratori mogu koristiti [Azure AD uvjetnog pristup](active-directory-conditional-access.md) da biste postavili pravilnike za pristup za određene uloge. Ta pravila možete uključiti li pristup dopušten je izvan korporacijskom okruženju i čak i višestruka provjera autentičnosti ili uređaju preduvjeti da biste postigli pristupa u različitim slučajeva.

## <a name="how-can-i-get-started"></a>Kako mogu početi?

Prvo ako trenutno ne koristite Azure AD niste administrator:

 - [Isprobajte!](https://azure.microsoft.com/trial/get-started-active-directory/) -možete prijaviti se za besplatnu 30-dnevnu probnu verziju danas i implementaciju prvi oblaka rješenja u odjeljku pet minuta pomoću ove veze

Azure AD značajke koje omogućuju zajedničko korištenje računa obuhvaćaju sljedeće:

- [E-pošta](active-directory-accessmanagement-self-service-group-management.md)
- Dodavanje aplikacije za Azure AD
- Početak rada sa zadacima
- Dodjela aplikacije najčešća Pitanja
- [Izvješća o/aplikacije korištenju nadzorne ploče](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Gdje mogu saznati više?

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Zaštita aplikacije pomoću uvjetnog programa access](active-directory-conditional-access.md)
- [Upravljanje SSAA samostalno grupe](active-directory-accessmanagement-self-service-group-management.md)
