<properties
    pageTitle="Prilagodba mapiranja atributa | Microsoft Azure"
    description="Saznajte što mapiranja atributa SaaS aplikacije u Azure Active Directory kako možete ih izmijeniti da biste riješili poslovne potrebe."
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


# <a name="customizing-attribute-mappings"></a>Prilagodba mapiranja atributa


Microsoft Azure AD pruža podršku za korisnika dodjele resursa za aplikacije drugih proizvođača SaaS kao što su Salesforce, servisa Google Apps i drugim korisnicima. Imate li korisnik dodjele resursa za drugog proizvođača aplikacije SaaS omogućeno Portal za upravljanje Azure kontrole atribut vrijednosti u obrascu konfiguracije pod nazivom "mapiranje atributa".

Postoji unaprijed konfiguriranom skup atribut mapiranja između Azure AD korisnika i svaku aplikaciju SaaS korisnik objekata. Neke se aplikacije upravljanje druge vrste objekata, kao što su grupe ili kontakata. <br> 
Možete prilagoditi mapiranja atributa zadani prema poslovne potrebe. To znači da, možete promijeniti ili izbrisati postojeće mapiranja atributa ili stvaranje novih mapiranja atributa.

Na portalu za Azure AD tu značajku možete pristupiti tako da kliknete atribute na alatnoj traci SaaS aplikacije.

> [AZURE.NOTE] **Atributi** veze dostupna je samo ako ste korisnik dodjeljivanje omogućen za aplikaciju SaaS. 


![Salesforce][1] 


Kada kliknete atribute na alatnoj traci na popisu trenutni mapiranja podešena za aplikaciju SaaS.

Sljedeća slika prikazuje primjer za to:



![Salesforce][2]  


U gornjem primjeru možete vidjeti atribut **ime** upravljanih objekta u Salesforce je popunjen **givenName** vrijednost povezane Azure AD objekta.

Ako ili Prilagodba mapiranja atributa ili ako želite vratiti prilagođene postavke natrag na zadanu konfiguraciju, to možete učiniti tako da kliknete povezane gumb na alatnoj traci pri dnu aplikacije.


![Salesforce][3]  


Postoje mapiranja atributa koji su potrebni za SaaS aplikacije da biste pravilno funkcionirati. U tablici atribute mapiranja povezanog atributa imaju **da** kao vrijednost atributa **obavezno** . Ako je mapiranje atributa je potrebno, možete je izbrisati. U ovom slučaju **Brisanje** značajka nije dostupna.

Da biste izmijenili postojeće mapiranje atributa, samo odaberite mapiranje, a zatim **Uređivanje**. Tako ćete prikazati dijaloški okvir stranicu koja omogućuje vam da biste izmijenili mapiranje odabrane atributa.


![Uređivanje mapiranja atributa][4]  



## <a name="understanding-attribute-mapping-types"></a>Objašnjenje atribut mapiranje vrsta


Sa mapiranjima atributa kontrolu načina na koji se atributi unose u nekog drugog proizvođača aplikacije SaaS. Postoje četiri različite mapiranje vrste podržava:

- **Izravni** – ciljni atribut je popunjen vrijednost atribut povezani objekt u Azure AD.


- **Konstanta** – ciljni atribut je popunjen određeni niz koji ste naveli.


- **Izraz** – ciljni atribut popunjeno je rezultat izraza skripte nalik na temelju. Dodatne informacije potražite u članku [Pisanje izraza za mapiranja atributa u servisu Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).


- **Ništa** – ciljni atribut je nepromijenjenu lijevo. Međutim, ako ikad prazna ciljni atribut, ona će se popuniti zadanu vrijednost koju navedete.



Uz ove četiri vrste mapiranje osnovni atribut, prilagođeni atribut mapiranja podržava pojam dodjele **zadanu** vrijednost. Zadana vrijednost dodjeljivanjem osigurava da ciljni atribut je popunjen vrijednost ako postoji nijedna vrijednost u Azure AD niti na ciljnog objekta.

Microsoft Azure AD pruža vrlo učinkovitog implementaciju sustava postupak sinkronizacije. U okruženju inicijalizirana samo one objekte koje je obavezna ažuriranja se obrađuju tijekom sinkronizacije ciklus. Ažuriranje mapiranja atributa ima utjecaj na performanse ciklusa sinkronizacije. To je zato što ažuriranje mapiranja konfiguracije atribut zahtijeva svih objekata upravljanih da biste se reevaluated. Zbog toga je preporučena preporučenim načinom rada da biste zadržali broj uzastopna promjene da biste mapiranja atributa najmanje.


##<a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Automatiziranje korisničkog dodjele resursa/Deprovisioning SaaS aplikacije](active-directory-saas-app-provisioning.md)
- [Pisanje izraza za mapiranja atributa](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Određivanje opsega filtre za dodjelu resursa korisnika](active-directory-saas-scoping-filters.md)
- [Koristite SCIM da biste omogućili automatsko dodjeljivanje korisnika i grupa iz servisa Azure Active Directory aplikacijama](active-directory-scim-provisioning.md)
- [Račun dodjeljivanje obavijesti](active-directory-saas-account-provisioning-notifications.md)
- [Popis vodiče za integraciju SaaS aplikacije](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
