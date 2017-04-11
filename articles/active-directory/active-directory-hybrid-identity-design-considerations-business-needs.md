<properties
    pageTitle="Azure Active Directory hibridnog identiteta zahtjevi dizajna - procjeni uvjeta identiteta | Microsoft Azure"
    description="Odredite poslovne potrebe tvrtke koji će voditi definirati zahtjeve za dizajn identiteta hibridnog."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Određivanje identiteta preduvjeti za hibridno rješenje identiteta
Prvi korak u dizajniranje rješenja za identitet hibridnog je da biste odredili preduvjeti za tvrtku ili ustanovu tvrtke koji će korištenje rješenja.  Identitet hibridnog pokreće kao podrške ulogu (podržava sva druga rješenja oblak tako da omogućuje provjeru autentičnosti) i prelazi na pružaju nove i zanimljivih mogućnosti koje otključavanje novi radnih opterećenja za korisnike.  Ove radnih opterećenja ili servise koje želite prihvaćaju za korisnike se određuje preduvjeti za dizajn identiteta hibridnog.  Ove usluge i radnih opterećenja moraju odražava oba lokalnog hibridnog identiteta i u oblaku.  

Morate ići preko ove ključne aspekte tvrtke da biste shvatili što je sada preduvjet i što tvrtke tarife u budućnosti. Ako nemate vidljivost strategije dugoročnu hibridnog identiteta dizajna, vjerojatno da rješenje neće biti skalabilni kao tvrtke potrebno rasti i promijeniti.   T he dijagram niže prikazano je primjer arhitekturu hibridnog identiteta i radnih opterećenja koji su se otključana za korisnike. To je samo na primjer sve nove mogućnosti koje se mogu otključati i koja se isporučuje s strategije identiteta pune hibridnog. 
 
Neke komponente koje su dio identiteta arhitektura hibridnog![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Određivanje poslovne potrebe
Svako poduzeće će različito tretiraju, čak i ako su te tvrtke dio iste industrijskih, realni tvrtke preduvjeti mogu se razlikovati. I dalje možete koristiti najbolja iskustva iz na industrijskih, ali konačni je poslovne potrebe tvrtke koji će voditi definirati zahtjeve za dizajn identiteta hibridnog. 

Provjerite odgovaraju na sljedeća pitanja da biste odredili poslovnim potrebama:

- Vaša tvrtka gleda izrezivanje IT radu trošak?
- Vaša tvrtka gleda sigurne oblaka imovine (SaaS aplikacije, Infrastruktura)?
- Je li vaša tvrtka želite li modernize vaš IT?
  - Su korisnici mobilne uređaje i zahtjevne IT da biste stvorili iznimke u svoje DMZ da biste omogućili različite vrste promet da biste pristupili različite resursima?
  - Mora li vaša tvrtka naslijeđene aplikacije koje potrebne za objavljivanje na te Moderna korisnike, ali nisu lako dopune?
  - Vaša tvrtka mora izvršiti sljedeće zadatke i Premjesti u odjeljku kontrola u isto vrijeme?
- Vaša tvrtka gleda sigurne identiteti korisnika i smanjiti rizik tako da nove alate pomoću kojih iskoristite poznavanje jezika Microsoft Azure sigurnosti stručna znanja lokalnog programa?
- Vaša tvrtka pokušava riješiti dreaded "vanjski" računi lokalno i premjestite ih u oblak gdje se nalazili više Neaktivni prijetnju unutar lokalnog okruženja?

## <a name="analyze-on-premises-identity-infrastructure"></a>Analiza infrastrukture lokalnih identiteta
Sad kad ste ideju o poslovnim potrebama tvrtke, morate procijeniti vaše lokalne identiteta infrastrukture. Ovaj procjenu važno je za definiranje Tehnički preduvjeti Integracija trenutnog identiteta rješenja sustava upravljanja oblaka identitet. Provjerite odgovaraju na sljedeća pitanja:

- Rješenje koje provjere autentičnosti i autorizacije li vaša tvrtka koristi lokalni? 
- Vaša tvrtka trenutno mora li servisi za sinkronizaciju lokalnog?
- Vaša tvrtka koristi neki davatelji identiteta treće strane (IdP)?

Morate imati na umu servisa u oblaku koje možda imaju vaše tvrtke. Važno je izvođenje procjenu da biste shvatili trenutni Integracija s SaaS, IaaS ili PaaS modela u svom okruženju. Provjerite je li tijekom ovog procjeni odgovaraju na sljedeća pitanja:
- Mora li vaša tvrtka integraciju kod davatelja usluge oblak?
- Ako je da, usluga koje se koriste?
- Trenutno se ta Integracija proizvodnje ili je probnog?


>[AZURE.NOTE]
Ako ne imate točne mapiranje sve aplikacije i servise u oblaku, možete koristiti alat za otkrivanje aplikacije oblaka. Ovaj alat može dati uvid u sve svoje tvrtke ili ustanove tvrtke i potrošača oblaka aplikacije svom IT odjelu. Koji se lakše nego ikada da biste otkrili sjene IT u tvrtki ili ustanovi, uključujući detalje o uzoraka korištenja i svi korisnici pristup oblaku aplikacija. Da biste pristupili ovaj alat otvorite [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>Analiza identiteta Integracija preduvjeti
Ćete morati procijeniti zahtjevi integriranja identitet. Ovaj procjenu je važno da biste definirali Tehnički preduvjeti za kako će provjere autentičnosti korisnika, kako će izgledati u tvrtki ili ustanovi prisutnosti u oblaku, kako tvrtki ili ustanovi omogućuje provjeru autentičnosti i što korisničko sučelje će biti. Provjerite odgovaraju na sljedeća pitanja:

- Vaša tvrtka ili ustanova koristit će vanjski pristup, standardne provjere autentičnosti ili oboje?
- Preduvjet je pridruženi pristup?  Zbog na sljedeći način:
 - Kerberos temelje SSO
 - Vaša tvrtka ima programa lokalnog aplikacije (ili ugrađen sesije ili 3 strana) koji koristi SAML ili slične vanjski pristup mogućnostima.
 - MFA putem pametne kartice. RSA SecurID itd.
 - Pristup pravila klijenta tu adresu pitanja u nastavku:
     1. Je li moguće blokirati vanjskog pristupa u Office 365 na temelju IP adresa klijenta?
     1. Je li moguće blokirati sve vanjski pristup sustavu Office 365, osim sustava Exchange ActiveSync?
     1. Je li moguće blokirati sve vanjski pristup sustavu Office 365, osim u aplikacijama utemeljenima na pregledniku (OWA, SPO)
     1. Možete blokirati mogu sve vanjski pristup sustavu Office 365 za određenu AD grupe
- Sigurnost i nadzor opasnosti
- Postojećih ulaganja u pridruženim provjere autentičnosti
- Što je naziv našu tvrtke ili ustanove koristit će naš domene u oblaku?
- Mora li se tvrtka ili ustanova prilagođenu domenu?
    1. Je tu domenu javnih i jednostavno koje se provjeravaju putem DNS-a?
    1. Ako nije, zatim imate javno domene koje je moguće koristiti da biste registrirali zamjenski UPN u AD?
- Dosljedni identifikatora korisnika za prikaz oblak? 
- Mora li tvrtke ili ustanove aplikacije koje je potrebno za integraciju s servise u oblaku?
- U tvrtki ili ustanovi ima više domena te će svi će koristiti standardno ili pridruženim provjere autentičnosti?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Analiza programa koji se pokreću u svom okruženju
Sad kad ste ideju o lokalnih i infrastrukture u oblaku, morate procijeniti programa koji se pokreću u te okruženja. Ovaj procjenu je važno da biste definirali Tehnički preduvjeti za integraciju te aplikacije sustav za upravljanje oblaka identiteta. Provjerite odgovaraju na sljedeća pitanja:

- Gdje će live naš aplikacije?
- Će korisnicima biti pristupa lokalnim aplikacije?  U oblaku? Ili oboje?
- Postoje li tarife da biste snimili postojećih radnih opterećenja za aplikaciju i premjestite ih u oblak?
- Postoje li tarife za razvoj nove aplikacije koje će se nalaziti bilo lokalno ili u oblak kojima će se koristiti u oblaku provjeru autentičnosti?

## <a name="evaluate-user-requirements"></a>Analiza korisnički preduvjeti
Morate procijeniti korisnički preduvjeti. U ovom procjenu je važno da biste definirali koraka koji će biti potrebno za na boarding i nije korisnicima dok prelaze u oblak. Provjerite odgovaraju na sljedeća pitanja:

- Će korisnicima biti pristupanja aplikacije lokalnim?
- Će korisnicima biti pristup aplikacijama u oblaku?
- Kako korisnicima obično prijavite se na svoje lokalnog okruženja?
- Kako će korisnici prijaviti u oblak?

>[AZURE.NOTE]
Provjerite je li bilješke svaki odgovor i razumijevanje rationale iza odgovor. [Preduvjeti za određivanje incidenta odgovor](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) će ići preko mogućnosti dostupne i profesionalci/protiv svake mogućnosti.  Tako da imate odgovoriti na ta pitanja ćete odabrati koju mogućnost najbolje odgovara vašem poslovanju mora.

## <a name="next-steps"></a>Daljnji koraci
[Određivanje preduvjeti za sinkronizaciju direktorija](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)
