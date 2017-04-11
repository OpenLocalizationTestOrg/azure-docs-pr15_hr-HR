<properties
   pageTitle="Postupci upravljanja paket sigurnost i nadzora rješenje osnovne | Microsoft Azure"
   description="Ovaj dokument objašnjava kako pomoću OMS sigurnost i nadzor rješenja da biste izvršili osnovne procjenu svih nadziranim računala svrhu usklađenost i sigurnost."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>

# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Procjena osnovne operacije upravljanja paketa sigurnost i rješenje nadzora

Ovaj dokument pomaže vam da biste koristili mogućnosti procjenu osnovne [operacije upravljanja paket (OMS) sigurnost i nadzora rješenja](operations-management-suite-overview.md) da biste pristupili stanja sigurnosti nadziranim resurse.

## <a name="what-is-baseline-assessment"></a>Što je rizika referentne vrijednosti?

Microsoft, zajedno s industrijskih i državne tvrtkama ili ustanovama diljem svijeta, definira konfiguracija sustava Windows koji predstavlja implementacije iznimno sigurne server. Tu konfiguraciju je skup ključeva registra, postavke pravilnika nadzora i sigurnosne postavke pravilnika zajedno s Microsoftovim preporučenih vrijednosti te postavke. Taj skup pravila naziva osnovne sigurnost. Sigurnost OMS i nadzora mogućnost procjenu osnovne jednostavno možete skenirati svim svojim računalima radi usklađenosti. 

Postoje tri vrste pravila:

- **Pravila registra**: Provjerite se ispravno postavljeno ključevima registra.
- **Nadzor pravila**: pravila koja se odnose pravila nadzora.
- **Sigurnosna pravila**: pravila koja se odnose na korisničkih dozvola na računalu.

> [AZURE.NOTE] Pročitajte [Korištenje OMS sigurnost da biste procijenite osnovne konfiguracije sigurnosti](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) Kratak pregled ovu značajku.

## <a name="security-baseline-assessment"></a>Procjena osnovne sigurnosti

Možete pregledati trenutni procjenu osnovne sigurnosti na svim računalima nadzire OMS sigurnost i nadzora na nadzornoj.  Izvršavanje možete pristupiti na nadzornoj ploči rizika osnovne sigurnost pomoću sljedećih koraka:

1. U **Microsoft operacije upravljanja paketa** glavna nadzorna ploča, kliknite pločicu **Sigurnost i nadzora** .
2. **Sigurnost i nadzora** nadzornoj ploči kliknite **Osnovne procjenu** u odjeljku **Sigurnost domene**. Nadzorna ploča za **Procjenu osnovne sigurnost** pojavit će se kao što je prikazano na sljedećoj slici:
    
    ![Sigurnost OMS i procjenu osnovne nadzora](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Ove nadzorne ploče podijeljen u tri glavna područja:

- **Računala u usporedbi s osnovne**: u ovom se odjeljku daje sažetak broj računala na kojima su pristupiti i postotak računala koja se preneseni na vrednovanje. Pruža također gornji 10 računala i rezultat postotak za vrednovanje.
- **Obavezno pravila stanja**: Ova sekcija sadrži namjerom da bi se prikazala Informiranost nije uspjelo pravila prema težinu i nije uspjela pravila po vrsti. Zatrebaju prvi grafikonu brzo možete prepoznati ako većina nije uspjelo pravila ključnih, ili ne. Pruža i popis 10 nije uspjelo pravila i njihove težinu. Drugi graph prikazuje vrstu pravilo koje nije uspjela tijekom na vrednovanje. 
- **Računala nedostaje osnovne procjenu**: u ovom se odjeljku popis računala na kojima su pristupiti zbog nekompatibilnost programa operacijski sustav ili neuspješna. 

### <a name="accessing-computers-compared-to-baseline"></a>Pristup računalima u usporedbi s osnovne

Najbolje sva računala nalaze biti usklađen s osnovne procjenu sigurnost. No se očekuje da u nekim okolnostima to ne događa. Postupak upravljanja sigurnost, dio je važno da biste uključili pregledavanje računala prenesite svi testovi za procjenu sigurnost nije uspjelo. Brz način vizualizaciju koja je odabirom mogućnosti **pristupa računala** koja se nalazi u odjeljku **računala u usporedbi s osnovne** . Trebali biste vidjeti rezultat pretraživanja zapisnika prikazom računala kao što je prikazano na sljedećem zaslonu:

![Računalo pristupiti rezultata](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Rezultat pretraživanja prikazuju se u obliku tablice, gdje prvi stupac ima naziv računala i drugu boju sadrži broj pravila koja nije uspjelo. Dohvaćanje informacija o vrsti pravilo koje nije uspjela, kliknite u okvir broj neuspjelih pravila osim naziv računala. Trebali biste vidjeti rezultat slično kao ono koje je prikazano na sljedećoj slici:

![Detalji o računalu pristupiti rezultata](./media/oms-security-baseline/oms-security-baseline-fig3.png)

U ovom rezultat pretraživanja imate zbroj pristupa, broj ključnih pravila koja nije uspjela, pravila upozorenje i na informacije nije uspjela pravila.

### <a name="accessing-required-rules-status"></a>Pristup pravila potrebnih stanja

Nakon dobivanja podatak postotak broj računala koji prosljeđuju na vrednovanje, preporučujemo vam da biste dobili dodatne informacije o koja su pravila neuspješnih prema kritičnosti. Ovu vizualizaciju pomaže prioritet računalu na kojem se trebali pozabaviti prvi put da biste bili sigurni da će biti usklađen na vrednovanje sljedeće. Pokazivač miša postavite iznad ključnih dio grafikonu koja se nalazi na pločici **nije uspjela pravila prema težinu** , u odjeljku **potrebna pravila stanja** i kliknite je. Trebali biste vidjeti slično kao na sljedećem zaslonu rezultat:

![Nije uspjela pravila prema težinu pojedinosti](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

U taj rezultat zapisnika vidite vrstu osnovne pravilo koje nije uspjela, opis ovo pravilo i ID uobičajenih konfiguracije Enumeracije (CCE) ovo pravilo za sigurnost. Ta atributa mora biti dovoljno za izvođenje korektivne akcije da biste riješili taj problem u ciljnom računalu.

> [AZURE.NOTE] Dodatne informacije o CCE pristupiti [Nacionalna slabe baze podataka](https://nvd.nist.gov/cce/index.cfm).

### <a name="accessing-computers-missing-baseline-assessment"></a>Pristup računalima nedostaje osnovne procjenu

OMS podržava profil za osnovne član domene u sustavu Windows Server 2008 R2 do Windows Server 2012 R2. Windows Server 2016 osnovne još nije konačni i dodat će se čim objavljivanja. U odjeljku **računala nedostaje osnovne procjenu** prikazat će se svi drugi operacijski sustavi skenirana putem osnovne procjenu OMS sigurnost i nadzora.

## <a name="see-also"></a>Vidi također

U ovom dokumentu saznali OMS sigurnost i nadzora osnovne rizika. Dodatne informacije o sigurnosti OMS potražite u sljedećim člancima:

- [Pregled operacije upravljanja paket (OMS)](operations-management-suite-overview.md)
- [Praćenje i odgovarati na njih sigurnosnih upozorenja u operacije upravljanja paket sigurnost i rješenje nadzora](oms-security-responding-alerts.md)
- [Nadzor resursa u operacije upravljanja paket sigurnost i rješenje nadzora](oms-security-monitoring-resources.md)

