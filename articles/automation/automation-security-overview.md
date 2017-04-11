<properties
   pageTitle="Sigurnost Azure Automatizacija | Microsoft Azure"
   description="Ovaj članak sadrži pregled Automatizacija sigurnost i na različite načine provjere autentičnosti dostupne za automatizaciju računima u Azure automatizaciju."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="Automatizacija sigurnosti, sigurne automatizacije" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Azure Automatizacija sigurnosti
Azure automatizaciju omogućuje automatizaciju zadataka na temelju resursa u Azure, informacije o lokalnom i s drugim davateljima oblaku kao što su Amazon Web Services (AWS).  Kako bi runbook da biste izvršili potrebne Akcije, mora imati dozvole da biste sigurno pristupali resursa s minimalnim pravima potrebne unutar pretplate.  
U ovom se članku obrađuje različite podržava automatizaciju Azure scenariji za provjeru autentičnosti i vidjet ćete kako započeti ovisno o okruženju ili okruženja u kojima želite upravljati.  

## <a name="automation-account-overview"></a>Pregled računa za automatizaciju
Kada prvi put pokrenete Azure Automatizacija, morate stvoriti barem jedan račun za automatizaciju. Automatizacija računi omogućuju izdvajanja Automatizacija resursa (runbooks, resursi, konfiguracije) iz resursa koje se nalaze u drugim računima za automatizaciju. Pomoću računa za automatizaciju razdvojiti resursa u zasebnom logičke okruženja. Na primjer, jedan račun koji koristite za razvoj, drugi za proizvodnju i drugi za lokalnog okruženja.  Račun za Azure Automatizacija razlikuje se od Microsoftov račun ili računi stvoriti u vašoj pretplati Azure.

Automatizacija resursi za svaki račun za automatizaciju pridružuju jedno područje Azure, ali Automatizacija računa možete upravljati resursa u bilo kojem regija. Glavna razloga za stvaranje računa za automatizaciju u različitim područjima bi ako imate pravila koje su potrebne podatke i resurse za odvajanja određenu regiju.

>[AZURE.NOTE]Automatizacija računi i resursi koji sadrže stvaraju se na portalu za Azure, nije moguće pristupiti na portalu za Azure klasični. Ako želite da biste upravljali tim računima ili njihovim resursi s komponentom Windows PowerShell, morate koristiti modula Azure Voditelj resursa.

Svi zadaci koje izvršavate protiv resurse pomoću upravitelja resursa Azure i Azure cmdleta u automatizaciji Azure morate provjeriti autentičnost Azure pomoću provjere autentičnosti za utemeljene na vjerodajnica Azure Active Directory identitet tvrtke ili ustanove.  Provjera autentičnosti na temelju certifikat je izvorni način provjere autentičnosti s načinom rada za upravljanje servisom Azure, ali je složeno postavljanje.  Provjera autentičnosti Azure s korisnikom Azure AD je uvedeni natrag u 2014 ne samo pojednostavili postupak da biste konfigurirali račun za provjeru autentičnosti, ali podržavaju mogućnost koje nisu-interaktivno provjeru autentičnosti Azure s jednog korisničkog računa radili s Voditelj resursa Azure i klasični resursi.   

Trenutno prilikom stvaranja novog računa za automatizaciju na portalu za Azure, automatski stvara:

-  Pokrenite kao račun koji stvara novi Upravitelj servisa Azure Active Directory, certifikat, a dodjeljuje suradnika na temelju uloga kontrolu pristupa (RBAC), koji će se koristiti za upravljanje resursima resurse pomoću runbooks.
-  Klasični Pokreni kao račun tako da prenesete certifikat za upravljanje, koji će se koristiti za upravljanje Upravljanje servisom Azure ili klasični resurse pomoću runbooks.  

Kontrola pristupa na temelju uloga dostupna s Azure Voditelj resursa dopustiti dopušteno akcije Azure AD korisnički račun i izvođenje kao račun i autentičnost glavnicu taj servis.  Pročitajte [Kontrola pristupa na temelju uloga u članku Azure Automatizacija](../automation/automation-role-based-access-control.md) podrobnije pomognu pri razvoju modela za upravljanje dozvolama za automatizaciju.  

Runbooks sustavom tempiranja Runbook hibridnog u vašem podatkovnog centra ili računalstvo servisi u AWS ne možete koristiti klikanje stanja tijeka rada koji se obično koristi za provjeru autentičnosti runbooks Azure resursi.  To je zato tih resursa koristite izvan Azure i stoga ćete morati vlastite sigurnosne vjerodajnice definirano u automatizaciji za provjeru autentičnosti resursa koje oni će imati pristup lokalno.  

## <a name="authentication-methods"></a>Metoda provjere autentičnosti

U sljedećoj su tablici navedene u različitim metoda provjere autentičnosti za svako okruženje podržava automatizaciju Azure i u u članku se opisuje kako postaviti provjere autentičnosti za vaše runbooks.

Način  |  Okruženje  | Članak
----------|----------|----------
Azure AD korisničkog računa | Azure Voditelj resursa i upravljanje servisom Azure | [Autentičnost Runbooks Azure AD korisnički račun](../automation/automation-sec-configure-aduser-account.md)
Azure Pokreni kao račun | Azure Voditelj resursa | [Autentičnost Runbooks s računom za Azure Pokreni kao](../automation/automation-sec-configure-azure-runas-account.md)
Azure klasični Pokreni kao račun | Upravljanje servisom Azure | [Autentičnost Runbooks s računom za Azure Pokreni kao](../automation/automation-sec-configure-azure-runas-account.md)
Provjera autentičnosti sustava Windows | Lokalni podatkovnim centrom | [Autentičnost Runbooks za zaposlenici zaduženi za hibridno Runbook](../automation/automation-hybrid-runbook-worker.md)
AWS vjerodajnice | Amazon web-servisi | [Autentičnost Runbooks s Amazon web-usluge (AWS)](../automation/automation-sec-configure-aws-account.md)



