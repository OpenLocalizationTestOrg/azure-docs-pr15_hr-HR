<properties
    pageTitle="Azure Active Directory hibridnog identiteta dizajniranje pitanja vezana uz - određivanje zadatke upravljanja identiteta hibridnog | Microsoft Azure"
    description="S kontrolom uvjetno pristup Azure Active Directory provjerava određenim uvjetima koje ste odabrali prilikom provjere autentičnosti korisnika i prije omogućivanja pristupa aplikaciji. Kada su ti uvjeti zadovoljeni, korisnik je autentičnost provjerena i dopuštenje za pristup aplikaciji."
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

# <a name="plan-for-hybrid-identity-lifecycle"></a>Plan za životni ciklus hibridnog identiteta 

Identitet jedan je od foundations mobilnost i aplikacije programa access strategije enterprise. Hoće li se prijave mobilnom uređaju ili aplikaciji SaaS, vaš identitet je ključ za pristup sve. Na najvišoj razini rješenje za upravljanje za identitet obuhvaća objedinjujućih i sinkroniziranje između spremištima vaš identitet koji uključuje Automatizacija i središnje postupka dodjele resursa resursi. Rješenje identiteta mora biti središnje identiteta putem lokalnog i oblaka i koristiti neki oblik vanjski pristup identitetu u održavanje središnje provjeru autentičnosti i sigurno zajedničko korištenje i Suradnja s vanjskim korisnicima i tvrtke. Resursi u rasponu od operacijskih sustava i aplikacije korisnika u ili povezan s tvrtkom ili ustanovom. Strukturi tvrtke ili ustanove moguće je izmijeniti kako bi odgovarao pravila i postupci za dodjelu resursa.

Također važno je imati usmjerena na učine korisnici unosom ih pomoću samoposlužnog sučelja da biste zadržali produktivni rješenje za identitet. Robusne ako omogućuje jedinstvenu prijavu korisnika preko svih resursa koje su im potrebne uopće pristup administratorima razine možete koristiti standardizirani postupke za upravljanje korisničke vjerodajnice je rješenje identitet. Neke razine Administracija možete smanjiti ili ukloniti, ovisno o breadth dodjele resursa rješenje za upravljanje. Osim toga, možete sigurno distribuirati značajke administracije, automatski ili ručno između različitih tvrtke ili ustanove. Na primjer, administrator domene vam može poslužiti samo osoba i resursa u toj domeni. Taj korisnik može izvršavati zadatke administratora i dodjelu resursa, ali nemate ovlasti za obavljanje zadataka za konfiguraciju, kao što su stvaranje tijekova rada.


## <a name="determine-hybrid-identity-management-tasks"></a>Određivanje zadatke upravljanja identiteta hibridnog
Raspodjela administrativne zadatke u tvrtki ili ustanovi poboljšava točnost i učinkovitost Administracija web-mjesta i poboljšava saldo radno opterećenje tvrtke ili ustanove. Slijede zaokretne tablice koje definiranje sustava upravljanja robusne identitet.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)


Da biste definirali zadatke upravljanja identiteta hibridnog, morate razumjeti neke ključne značajke koji će usvojio hibridnog identitet tvrtke ili ustanove. Je važno je znati trenutni spremištima koristi izvore identitet. Kada zna elementi core sadržavat će foundational preduvjeti, a na temelju ćete morati zatražiti precizniji pitanja na koja će vas voditi da biste bolje odluke dizajna za rješenje identitet.  

Prilikom definiranja tih zahtjeva, pobrinite se da barem su odgovori na sljedeća pitanja

- Dodjeljivanje mogućnosti: 
 - Podržava li rješenje identiteta hibridnog Upravljanje pristupom robusne računima i sustava za dodjelu resursa?
 - Po čemu se korisnike, grupe i lozinke da se upravlja?
 - Životni ciklus upravljanje identitetom se odgovara? 
      - Koliko dugo ne lozinku ažuriranja računa obustavljanje poduzeti?
      
- Upravljanje licencama: 
 - Označava upravljanje licencama za hibridno identiteta rješenje ručice?
     - Ako je da, koje mogućnosti dostupne su?
- Ne rješenje rukovati upravljanje licencama grupne? 
      – Ako je da, je li moguće dodati sigurnosne grupe? 
       – Ako je da, će direktorija oblaka automatski dodjeljivati licence za sve članove grupe? 
        – Što se događa ako je korisnik naknadno dodati ili ukloniti iz grupe, će licence se automatski dodijeliti ili ukloniti sukladno situaciji? 

- Integracija s drugih davatelja identiteta treće strane:
- Može li se rješenje hibridnog integrirati s davatelji identiteta treće strane implementaciju jedinstvenu prijavu?
- Je li moguće ujednačavanja svih proizvođača drugog identiteta u sustavu povezujuće identiteta?
- Ako da, kako i koji su oni i koje mogućnosti dostupne su?

## <a name="synchronization-management"></a>Upravljanje sinkronizacijom
Jedan od ciljeva programa Upravitelj identiteta omogućiti da bi svi davatelji identiteta, a zatim ih držati sinkronizirati. Zadržavanje podataka koji se sinkroniziraju na temelju davateljem mjerodavne osnovne identiteta. U identitet scenarija hibridnog, s modelom sinkroniziranim upravljanja, upravljanja svim korisnicima i uređajima na lokalni poslužitelj i sinkronizacija računi i, Neobavezno, lozinke u oblak. Korisnik unosi u istu lozinku lokalnog kao on ili ona ne u oblaku i pri prijavi, lozinka je potvrđena rješenje identitet. Ovaj model koristi alat za sinkronizaciju imenika.
 
![](./media/hybrid-id-design-considerations/Directory_synchronization.png)Proper dizajn sinkronizacije rješenje identiteta hibridnog bili sigurni da se odgovori na sljedeća pitanja: • koji su dostupni za identitet rješenje hibridnog rješenja za sinkronizaciju?
• Koji su jedine prijave mogućnosti dostupne?
• Mogućnosti za vanjski pristup identitetu između B2B i B2C?

## <a name="next-steps"></a>Daljnji koraci
[Određivanje strategiju prihvaćanja hibridnog identiteta](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)


## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)

