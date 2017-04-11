<properties
    pageTitle="Atribut aplikaciju dodjele resursa pomoću filtara za određivanje opsega | Microsoft Azure"
    description="Saznajte kako koristiti scoping filtre da biste spriječili objekata u aplikacijama koje podržavaju automatiziranog korisnika dodjele resursa zapravo se dodjeli ako objekt ne zadovoljava s poslovnim potrebama."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="attribute-based-app-provisioning-with-scoping-filters"></a>Atribut aplikaciju dodjele resursa pomoću filtara za određivanje opsega

Cilj ovaj odjeljak je objašnjavaju kako koristiti scoping filtre da biste definirali pravila temeljena na atributa koji će odrediti korisnike koji će biti dodijeljena da aplikacija.





## <a name="clauses-and-scope-groups"></a>Rečenice i grupa dosega


![Određivanje opsega filtra][1] 




Određivanje opsega filtri definira jednu ili više **grupa opseg**, od kojih svaka držite jednu ili više **uvjeta**. Da biste vidjeli uvjete za dani doseg grupu, proširite ga tako da kliknete strelicu lijevo od naziva grupe.

**Uvjet** određuje koje korisnici mogu proći kroz scoping filtar analizom atribute za svakog korisnika. Ako, na primjer, možda imate jedan uvjet koji zahtijeva da se atribut '-Savezna korisničke jednako New York, što znači da samo korisnici New York dodijelit će se Resursi u aplikaciju.

![Određivanje opsega naziv grupe][2] 



Svaka **grupa opseg** počinje jedan obavezno **uvjet**, kao što je prikazano u snimku zaslona koja se nalazi iznad. Ovaj uvjet jednostavno navodi da korisnik mora prvo biti dodijeljena aplikaciji prije nego što je procijeniti scoping filtre. Izraz ne može izbrisati ili izmijenjene.

Možete dodati nove rečenice ili novih grupa opseg pritiskom na odgovarajući gumb. Možete dati naziv svake grupe opseg tako da uredite svojstva **Doseg naziva grupe** .





## <a name="how-scoping-filters-are-evaluated"></a>Kako se procjenjuju određivanje opsega filtara

Prilikom dodjele resursa, ispitivanja svaki dodijeljenog korisnika na temelju scoping filtre da biste odredili zaslužuje li korisnik pristup aplikaciji. Možete shvatiti svaki uvjet kao testu koji se mora proslijediti redoslijedom za korisnika da bi se izbjeglo početak filtrira. 

Ako imate više opseg grupa definirani, svaki korisnik mora barem jedan od njih proći da biste pristupili aplikacije. Unutar svake grupe opseg, međutim, korisnik mora proći svaki jedan uvjet da bi se proslijediti toj grupi određene opseg. 

Drugim riječima, možete shvatiti opseg grupe kao ili način zajedno i mislite uvjeta u njima, kao i želite zajedno. Na primjer, razmotrite scoping filtar ispod:


![Određivanje opsega naziv grupe][2]  


Prema scoping filtar, korisnici mora zadovoljavati sljedeće kriterije, da bi se dodjeli:

1. Mora biti dodijeljena aplikaciji.

2. Morate funkcioniraju u inženjering odjelu

3. Moraju biti rad u Pula ili Kanadi.


##<a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Automatiziranje korisniku dodjele resursa i Deprovisioning SaaS aplikacijama](active-directory-saas-app-provisioning.md)
- [Prilagodba mapiranja atributa za dodjelu resursa korisnika](active-directory-saas-customizing-attribute-mappings.md)
- [Pisanje izraza za mapiranja atributa](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Račun dodjeljivanje obavijesti](active-directory-saas-account-provisioning-notifications.md)
- [Koristite SCIM da biste omogućili automatsko dodjeljivanje korisnika i grupa iz servisa Azure Active Directory aplikacijama](active-directory-scim-provisioning.md)
- [Popis vodiče za integraciju SaaS aplikacije](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png
