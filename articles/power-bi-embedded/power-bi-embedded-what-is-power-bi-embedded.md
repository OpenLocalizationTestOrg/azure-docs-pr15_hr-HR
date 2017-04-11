<properties
   pageTitle="Što je Microsoft Power BI ugrađene?"
   description="Power BI ugrađene omogućuje integraciju izvješćima servisa Power BI na web i mobilne aplikacije pa ne morate stvoriti prilagođena rješenja za vizualni prikaz podataka za korisnike"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="what-is-microsoft-power-bi-embedded"></a>Što je Microsoft Power BI ugrađene?

Pomoću **Dodatka Power BI ugrađene**, možete integrirati izvješćima servisa Power BI ugrađene u weba ili mobilne aplikacije.

![](media\powerbi-embedded-whats-is\what-is.png)

Power BI ugrađeni je **Azure servis** koji omogućuje ISV-ovi i aplikacije inženjerima omogućuje plošni sučelja podataka servisa Power BI u svojim aplikacijama. Kao programer, već sastavljali aplikacije, a te aplikacije imaju vlastite korisnika i distinct skup značajki. Tim aplikacijama i može se dogoditi da bi se neke elemente ugrađene podatke kao što su grafikoni i izvješća koja možete odmah se pokreće Microsoft Power BI ugrađeni. Korisnici ne moraju račun servisa Power BI za korištenje aplikacije. Možete nastaviti prijavite se u aplikaciji baš kao i prije, i prikaz i interakciju s dodatkom Power BI izvješćivanje u značajki bez dodatnih licenci.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Ugrađeni licenciranju za Microsoft Power BI

U **Microsoft Power BI ugrađene** modelu korištenje licenciranje za Power BI nije odgovornosti krajnji korisnik.  Umjesto toga **prikazuje** su kupljenje putem programiranje aplikacije koja koristi poboljšanjima i se naplaćuju pretplatu koja je vlasnik tih resursa.

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI ugrađene konceptualni modela

![](media\powerbi-embedded-whats-is\model.png)

Kao što je bilo koji drugi servis servisu Azure resursi za Power BI ugrađene su dodjeli kroz [Azure ARM API -ji](https://msdn.microsoft.com/library/mt712306.aspx). U ovom slučaju resursa koji vam Dodjela je **Prikupljanje radnog prostora za Power BI**.

## <a name="workspace-collection"></a>Zbirka radnog prostora

**Radni prostor zbirka** je najviše razine Azure spremnik za resurse koji sadrži 0 ili više **radnih prostora**.  **Radni prostor** **zbirke** sadrži sve standardna svojstva Azure, kao i sljedeće:

-   **Tipke za pristup** – ključevi koji se koriste prilikom sigurno pozivanja Power BI API-ji (što je opisano u nastavku).
-   **Korisnici** – Azure Active Directory (AAD) korisnika s administratorskim pravima za upravljanje u zbirci radnog prostora za Power BI putem portala za Azure ili ARM API-JA.
-   **Regija** – kao dio dodjeljivanje **Zbirke radnog prostora**, odaberite područje da biste se resursi. Dodatne informacije potražite u članku [Azure područja](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Radni prostor

**Radni prostor** je spremnik sadržaj servisa Power BI, što obuhvaća skupova podataka, izvješća i nadzorne ploče. **Radni prostor** je prazan kada prvog unosa. Tijekom pretpregleda, ćete autor sav sadržaj pomoću dodatka Power BI Desktop, a zatim ćete je prijenos na jedan od radnih prostora pomoću [Dodatka Power BI REST API -ji](http://docs.powerbi.apiary.io/reference).

## <a name="using-workspace-collections-and-workspaces"></a>Korištenje radnog prostora zbirke i radne prostore
**Radni prostor zbirke** i **radni prostori** su spremnici sadržaja koji se koriste i organizirani u bez obzira na način najbolje odgovara dizajn aplikacije koje izrađujete. Pojavit će se brojne načine nije li razmještaj sadržaja unutar njih. Možete odabrati da biste stavili sav sadržaj u jedan radni prostor pa zatim kasnije pomoću aplikacije tokeni dodatno Podjela sadržaja između klijenata. Također možete odabrati sve klijentima pohraniti u zasebnom radnom prostoru tako da postoji neki odvojenosti između njih. Ili, možete odabrati da biste organizirali korisnicima putem regija umjesto klijenta. Ovaj fleksibilne dizajn omogućuje vam da biste odabrali najbolji način da biste organizirali sadržaj.

## <a name="cached-datasets"></a>Predmemorirani skupova podataka

Predmemorirani skupove podataka može se koristiti u pretpregledu.  Međutim, nije moguće osvježiti predmemoriranim podacima kada je učitana u **Microsoft Power BI ugrađeni**.

## <a name="authentication-and-authorization-with-app-tokens"></a>Provjere autentičnosti i autorizacije pomoću aplikacije tokena

**Microsoft Power BI ugrađene** preusmjerava na aplikacije da biste izvršili provjeru autentičnosti potrebno korisnika i autorizacije. Postoji bez izričitog zahtjeva da krajnji korisnici moraju biti kupci Azure Active Directory (Azure AD).  Umjesto toga aplikacije izražava **Microsoft Power BI ugrađenu** provjeru autentičnosti za prikaz izvješća servisa Power BI pomoću **Aplikacije tokeni za provjeru autentičnosti (tokene aplikacije)**.  Ove **Aplikacije tokeni** stvaraju se prema potrebi pri aplikacijom želi za prikaz izvješća.  U odjeljku [tokeni aplikacije](power-bi-embedded-get-started-sample.md#key-flow).

![](media\powerbi-embedded-whats-is\app-tokens.png)

**Tokeni za provjeru autentičnosti aplikacije (aplikacije tokene)** se koriste za provjeru autentičnosti **Sustava Microsoft Power BI ugrađeni**.  Postoje tri vrste **Tokeni aplikacije**:

1.  Dodjeljivanje tokeni - koristi prilikom dodjele resursa za novog **radnog prostora** u **Zbirku radnog prostora**
2.  Tokeni razvoj - koristi pri upućivanju poziva izravno u **Power BI REST API -ji**
3.  Ugrađivanje tokeni - koristi pri upućivanju poziva za prikaz izvješća u ugrađeni iframe

Tokena koriste se za različite faze interakcije s **Microsoft Power BI ugrađeni**.  Tokena osmišljene su tako da možete dodijeliti dozvole iz aplikacije Power bi. Dodatne informacije potražite u članku [Aplikacije tokena tijeka](power-bi-embedded-app-token-flow.md).

## <a name="see-also"></a>Vidi također
- [Uobičajeni scenariji za Microsoft Power BI ugrađeni](power-bi-embedded-scenarios.md)
- [Uvod u Microsoft Power BI ugrađeni](power-bi-embedded-get-started.md)
