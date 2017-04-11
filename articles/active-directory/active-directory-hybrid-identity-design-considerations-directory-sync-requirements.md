<properties
    pageTitle="Preduvjeti za sinkronizaciju direktorija za određivanje Azure Active Directory hibridnog identiteta zahtjevi dizajna - | Microsoft Azure"
    description="Prepoznavanje preduvjeti za koje su vam potrebne za sinkroniziranje sve korisnike između na = lokalno i oblaka za tvrtke."
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

# <a name="determine-directory-synchronization-requirements"></a>Određivanje preduvjeti za sinkronizaciju direktorija
Sve o omogućuje korisnicima identitet u oblaku koja se temelji na svoj identitet lokalnog je sinkronizacija. Hoće li sinkroniziranim račun će se koristiti za provjeru autentičnosti ili pridruženim autentičnosti, korisnici i dalje moraju imati identitet u oblaku.  Identitet morat ćete se održava i ažurirati povremeno.  Ažuriranja može potrajati više obrazaca, naslov promjene promjene lozinki.  

Najprije procjene tvrtkama ili ustanovama lokalnih identiteta rješenje i korisnik zahtjevima. Ovaj procjenu je važno da biste definirali Tehnički preduvjeti za kako stvoriti i zadržavaju u oblaku identitetima korisnika.  Za većinu tvrtki ili ustanova servisa Active Directory je lokalno i sinkronizirat će se lokalnog imenika koji će korisnicima tako da šalje, no u nekim slučajevima to neće biti predmet.  

Provjerite odgovaraju na sljedeća pitanja:


- Imate jedan skup stabala AD, više ili ništa?
 - Koliko Azure AD direktorija će vam biti sinkronizaciju?
 
    1. Koristite filtriranje?
    2. Imate li više Azure AD Connect poslužitelja planirano?
  
- Trenutno imate li sinkronizacije alata lokalnog?
  - Ako je da, ne korisnika Ako korisnici imaju na virtualne/Integracija direktorija identiteta?
- Imate li neki drugi lokalno koje želite sinkronizirati (npr. LDAP imenik, HR baze podataka i itd.)?
  - Se premještaju se način sve GALSync?
  - Što je trenutno stanje UPNs u tvrtki ili ustanovi? 
  - Imate li neki drugi direktorij koje korisnici provjeru autentičnosti?
  - Vaša tvrtka koristi Microsoft Exchange?
    - Namjeravaju pamtiti hibridnu implementaciju sustava exchange?

Sad kad ste ideju o preduvjetima za sinkronizaciju, morate odrediti koji alat je onaj zadovoljavati sljedeće preduvjete.  Microsoft pruža nekoliko alata za obavljanje Integracija direktorija i sinkronizacija.  Pogledajte [hibridnog identiteta direktorija Integracija Alati tablicu](active-directory-hybrid-identity-design-considerations-tools-comparison.md) dodatne informacije. 
   
Sad kad imate preduvjeti za sinkronizaciju i alat koji će to postigli u vašoj tvrtki, morate procijeniti aplikacije koje koriste te imeničkim servisima. Ovaj procjenu je važno da biste definirali Tehnički preduvjeti za integraciju te aplikacije u oblak. Provjerite odgovaraju na sljedeća pitanja:

- Te aplikacije premještaju se u oblak i korištenje imenika?
- Postoje li posebnih atribute koje su potrebne za sinkronizaciju u oblak tako da ih te aplikacije mogli uspješno koristiti?
- Te aplikacije morat ćete ponovno zapisivanje da iskoristite prednost oblaka autorizacija?
- Te aplikacije bit će live lokalnog dok korisnici pristupaju ih pomoću identiteta oblak?

Morate odrediti na sigurnost zahtjevi i ograničenja sinkronizacije direktorija. U ovom procjenu je važno dobit ćete popis uvjete koji će biti potrebno da biste mogli stvoriti i održavati identitet korisnika u oblaku. Provjerite odgovaraju na sljedeća pitanja:

- Gdje poslužitelja sinkronizacije nalazit će se?
- Hoće li biti domene pridruženo?
- Će poslužitelj se nalaziti na mreži ograničenih iza vatrozida, kao što je DMZ?
  - Će moći otvoriti priključke potreban Vatrozid za podršku sinkronizacije?
- Imate li tarifu za oporavak Izrada za poslužitelj za sinkronizaciju?
- Imate li račun s odgovarajuće dozvole za sve šuma koju želite sinkronizirati s?
 - Ako vaša tvrtka ne znate odgovora za ovo pitanje, pročitajte odjeljak "Dozvole za sinkronizaciju lozinke" u članku [Instalacija Azure sinkronizaciju servis za Active Directory](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) i odrediti ako već imate postavljen račun s tih dozvola ili ako vam je potrebna da biste je stvorili.
- Ako imate mutli-skupa stabala sinkronizacija je poslužitelj za sinkronizaciju moći pristupiti svaki skup stabala?
 
>[AZURE.NOTE]
Provjerite je li bilješke svaki odgovor i razumijevanje rationale iza odgovor. [Preduvjeti za određivanje incidenta odgovor](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) će ići preko mogućnosti dostupne. Tako da imate odgovoriti na ta pitanja ćete odabrati koju mogućnost najbolje odgovara vašem poslovanju mora.

## <a name="next-steps"></a>Daljnji koraci
[Određivanje preduvjeti višestruka provjera autentičnosti](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)
