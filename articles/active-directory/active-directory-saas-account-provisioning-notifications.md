<properties
    pageTitle="Račun za dodjelu resursa obavijesti | Microsoft Azure"
    description="Saznajte kako da biste bili sigurni da su obavijesti o problema vezanih uz dodjeljivanje korisnika, a koje je potrebno obratiti pozornost omogućivanjem dodjele resursa vezanih uz obavijesti."
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


# <a name="account-provisioning-notifications"></a>Račun dodjeljivanje obavijesti

S korisnik dodjele resursa, možete automatizirati postupak upravljanja korisnicima u programima trećih strana SaaS. <br>
Dok je automatiziranog procesa, interakciju s tom se postupku s vremena na vrijeme potrebno je. <br>
Ovo je, primjerice predmet, kad lozinku za račun koji ste konfigurirali za razmjenu podataka s nekog drugog proizvođača SaaS istekla aplikacije. 

Omogućivanjem dodjele resursa vezanih uz obavijesti možete omogućiti da biste obavijesti od problema vezanih uz dodjeljivanje korisnika, a koje je potrebno obratiti pozornost.

Aktiviranje i deaktiviranje dodjele resursa vezanih uz obavijesti kao dio korisnički dodjeljivanje konfiguracija trećih strana SaaS aplikacije.

![Dodjela resursa za korisnika][1] 



Da biste aktivirali račun obavijesti za dodjelu resursa, odaberite povezane potvrdnog okvira na stranici dijaloški okvir za **potvrdu** , a zatim upišite pseudonim za e-pošte primatelja.

![Račun dodjeljivanje obavijesti][2]
 


Popis za raspodjelu možete unijeti kao primatelj. Međutim, važno je Imajte na umu da obavijesti e-pošte sadrži veze na izvješća koji su dostupni su administratori Azure AD.

Ako imate račun za dodjelu resursa obavijesti omogućena, primit ćete e-pošte o ključne probleme vezane uz dodjeljivanje korisnika. Međutim, da biste izbjegli preopterećenja programa e-pošte, samo dobit ćete jedan obavijesti e-pošte prema danu za svaku aplikaciju SaaS obavijesti e-pošte omogućen za.


##<a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Automatiziranje korisničkog dodjele resursa/Deprovisioning SaaS aplikacije](active-directory-saas-app-provisioning.md)
- [Prilagodba mapiranja atributa za dodjelu resursa korisnika](active-directory-saas-customizing-attribute-mappings.md)
- [Pisanje izraza za mapiranja atributa](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Određivanje opsega filtre za dodjelu resursa korisnika](active-directory-saas-scoping-filters.md)
- [Koristite SCIM da biste omogućili automatsko dodjeljivanje korisnika i grupa iz servisa Azure Active Directory aplikacijama](active-directory-scim-provisioning.md)
- [Popis vodiče za integraciju SaaS aplikacije](active-directory-saas-tutorial-list.md)



<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png