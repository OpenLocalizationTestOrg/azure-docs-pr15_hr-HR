<properties
   pageTitle="Upravljanje identiteta u Azure | Microsoft Azure"
   description="U članku se objašnjava i uspoređuje različite metode dostupne za upravljanje identiteta u sustavima hibridnog koje se protežu na – lokalno/oblaka granicu s Azure."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>Upravljanje identiteta u Azure

U većini tvrtki sustavima koji se temelji na sustavu Windows radi olakšavanja identiteta servise upravljanja aplikacija će koristiti Active Directory (AD). AD funkcionira dobro lokalnog okruženja, ali kada proširivanje mrežne infrastrukture u oblak su neke važne odluke da biste tiče kako upravljati identitet. Trebali biste proširiti domene lokalnog ugraditi VMs u oblaku? Potrebno stvoriti nove domene u oblaku, a ako je tako, kako? Trebali biste provesti vlastiti skup stabala u oblaku ili trebali biste učiniti pomoću programa Azure Active Directory (AAD)?

U ovom se članku opisuju neke uobičajene mogućnosti sastanka izazove posed po scenarij, a omogućuje određivanje koji rješenje najbolje odgovara vašim potrebama svojim potrebama.

## <a name="using-azure-active-directory"></a>Pomoću servisa Azure Active Directory

AAD možete koristiti za stvaranje domenom sustava AD u oblak i povezati ga s lokalnog servisa Active Directory domene. AAD omogućuje vam da biste konfigurirali jedinstvenu prijavu (SSO) za korisnike pokretanje aplikacija na kojoj se pristupa putem oblaka.

[! [0]][0]

AAD je jasan način možete implementirati sigurnosne domene u oblaku. Koristi se po mnoge aplikacije Microsoft, kao što je Microsoft Office 365. 

Prednosti korištenja AAD:

- Nema potrebe da biste zadržali infrastruktura za Active Directory u oblaku. AAD potpuno upravlja i održava Microsoft.

- AAD nudi iste podatke identitet koji je dostupan lokalno.

- Provjera autentičnosti se može dogoditi u Azure, smanjite potrebe za vanjske aplikacije i korisnika da se obratite lokalne domene.

Upućuje na umu prilikom korištenja AAD:

- Identitet services ograničeni su na korisnike i grupe. Postoji nema mogućnost za provjeru autentičnosti račune servisa i računalo.

- Morate konfigurirati povezivanje s lokalne domene da biste zadržali direktorija AAD sinkronizirati. 

- Vi ste odgovorni za objavljivanje aplikacije koje korisnici mogu pristupati u oblaku putem AAD.

Detaljne informacije potražite u članku [Implementacijom Azure Active Directory][implementing-aad].

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>Korištenje servisa Active Directory u oblak se pridružite na lokalni skupa stabala

Možete hostirati AD Directory Services (AD DS) lokalni, ali u scenariju hibridnog gdje se nalaze elementi aplikacije u Azure može biti učinkovitiji za replikaciju ta je funkcija i spremištu servisa Active Directory u oblak. Takvog smanjiti Latencija uzrokovanih slanja provjeru autentičnosti i zahtjevi za lokalni autorizacije iz oblaka natrag u AD DS radi lokalnog. 

[! [1]][1]

Taj se način zahtijeva Stvaranje vlastite domene u oblak i pridruživanje skupa stabala lokalnog. Stvaranje VMs za hostiranje servisa AD DS.

Prednosti korištenja zasebna domena u oblaku:

- Omogućuje provjeru autentičnosti korisnika, servisa i računalo račune lokalne i u oblaku.

- Omogućuje pristup iste podatke identitet koji je dostupan lokalno.

- Nema potrebe da biste upravljali zasebnom skupa stabala AD; domene u oblak pripadati skupa stabala lokalnog.

- Možete primijeniti pravilnik grupe definira lokalni objekt pravilnika GRUPE objekata na domenu u oblaku.

Okolnosti pri korištenju zasebna domena u oblaku:

- Morate stvarati i upravljati vlastitim poslužitelja za AD DS i domena u oblaku.

- Možda postoje neka sinkronizacije latencije između poslužitelja domene u oblak i poslužiteljima koji se izvode na lokalni.

Informacije o tome kako konfigurirati tu arhitekturu potražite u članku [Proširivanje Active Directory Directory Services (ZBRAJA) za Azure][extending-adds].

## <a name="using-active-directory-with-a-separate-forest"></a>Pomoću servisa Active Directory u zasebnom skupa stabala

Tvrtka ili ustanova koja se pokreće Active Directory (AD) lokalnog možda skupa stabala je comprising mnogo različitih domena. Domene možete koristiti za davanje odvajanja područja funkcionalnosti mora biti zadržane odvojeni, vjerojatno sigurnosnih razloga, ali možete omogućiti zajedničko korištenje informacija između domene uspostavljanjem pouzdani odnosi.

[! [2]][2]

Tvrtkom ili ustanovom koja koristi zasebnom domene možete iskoristiti Azure po relocating jedan ili više od tih domena u zasebnom skupa stabala u oblaku. Umjesto toga tvrtkom ili ustanovom možda želite zadržati sve resurse oblaka logično razlikuju od onih za te čuvanju lokalnog i pohranili podatke o oblaka resursa u vlastite direktoriju kao dio skupa stabala je i održava u oblaku.

Prednosti korištenja zasebnom skupa stabala u oblaku:

- Možete implementirati lokalnih identiteta i razdvajanje samo za Azure identiteta.

- Nema potrebe za replikaciju u lokalnim skupa stabala AD za Azure, smanjite efekata latenciju mreže.

Pitanja vezana uz:

- Provjere autentičnosti za lokalnih identiteta u oblaku izvodi dodatni mrežni *preskakanja* da biste na lokalnim poslužiteljima AD.

- Potrebno je implementirati vlastite AD DS poslužitelji i skup stabala u oblak i uspostaviti odgovarajuće pouzdanost odnose između šuma.

Dokument [Stvaranje na Active Directory Directory Services (ZBRAJA) skupa stabala resursa u Azure] [ adds-forest-in-azure] u članku se opisuje kako implementirati takvog detaljnije.

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>Korištenje servisa Active Directory Federation Services (ADFS) s Azure

ADFS jer se može pokrenuti lokalni, ali u scenariju hibridnog gdje se nalaze aplikacije u Azure može biti učinkovitije implementaciju ta je funkcija u oblaku, kao što je prikazano u nastavku.

[! [3]][3]

U ovom arhitektura je posebno korisno za:

- Rješenja koja koriste pridruženim autorizacije da biste otkrili web-aplikacija za partnera tvrtke ili ustanove.

- Sustavima koji podržavaju pristup iz web-preglednika koji se izvodi izvan tvrtke ili ustanove vatrozid.

- Sustavi koji omogućuju korisnicima pristup web-aplikacijama povezivanjem s ovlašteni vanjskih uređaja kao što su udaljenim računalima, bilježnice i drugim mobilnim uređajima. 

Prednosti korištenja ADFS s Azure:

- Omogućuje korištenje zahtjevima umu aplikacije.

- Pruža mogućnost Vjeruj vanjski partneri za provjeru autentičnosti.

- Pruža kompatibilnosti s velikim skup protokola za provjeru autentičnosti.

Zahtjevi za ADFS pomoću Azure:

- Zahtijeva implementirati vlastite ZBRAJA, ADFS i ADFS Web aplikacije Proxy poslužitelji u oblaku.

- U ovom arhitektura može biti složen da biste konfigurirali.

Detaljne informacije pročitajte [Implementacijom direktorija vanjski pristup ADFS (Active Services) u Azure][adfs-in-azure].

## <a name="next-steps"></a>Daljnji koraci

U nastavku resursima objašnjavaju kako implementirati arhitekturi opisane u ovom članku.

- [Implementacijom Azure Active Directory][implementing-aad]
- [Proširivanje usluge Active Directory (ZBRAJA) za Azure][extending-adds]
- [Stvaranje skupa stabala resursa u Active Directory Directory Services (ZBRAJA) u Azure][adds-forest-in-azure]
- [Implementacijom Active Directory Federation Services (ADFS) u Azure][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "Oblak pomoću servisa Azure Active Directory arhitektura za identitet"
[1]: ./media/guidance-identity/figure2.png "Sigurne hibridnog arhitektura mreže s servisom Active Directory"
[2]: ./media/guidance-identity/figure3.png "Sigurne hibridnog arhitektura mreže s zasebne AD domene i šuma"
[3]: ./media/guidance-identity/figure4.png "Sigurne hibridnog mreže arhitektura uz ADFS"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md