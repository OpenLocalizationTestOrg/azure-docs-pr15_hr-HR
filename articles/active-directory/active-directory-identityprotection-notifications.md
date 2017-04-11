<properties
    pageTitle="Azure Active Directory identiteta zaštita obavijesti | Microsoft Azure"
    description="Saznajte kako obavijesti podržava istrage aktivnosti."
    services="active-directory"
    keywords="Zaštita identiteta Azure active directory, otkrivanje aplikacije oblaka, Upravljanje aplikacijama, sigurnost, rizika, razina rizika, slabe, sigurnosna pravila"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory identiteta zaštita obavijesti 


Zaštita Azure AD identiteta šalje dvije vrste automatske obavijesti e-pošta olakšavaju upravljanje rizika korisnika i rizika događaja:

- Korisnik ugrožena upozorenja e-pošte

- Tjedni sažetka e-pošte

## <a name="user-compromised-alert-email"></a>Korisnik ugrožena upozorenja e-pošte

Upozorenje ugrožena e-poštu na korisnički je koji su generirani prilikom Azure AD identiteta zaštita označava račun kao što je ugrožena. E-pošte sadrži vezu korisnicima Obilježeno zastavicom za rizika izvješća na nadzornoj ploči identitet zaštitu. Preporučujemo da odmah istražiti obavijesti o ugrožena.


## <a name="weekly-digest-email"></a>Tjedni sažetka e-pošte

Tjedni e-pošte sažetka sadrži sažetak nove događaje rizika.<br>
Obuhvaća:

- Korisnici na rizik
- Sumnjive aktivnosti
- Otkriven slabe točke
- Veze na povezane izvješća u zaštitu identiteta


<br>
![Olakšava](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br> 

Možete se prebacivati slanje tjedni e-pošte sažetka.
<br><br>
![Korisnik rizika](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>
 

**Da biste otvorili dijaloški okvir za konfiguraciju povezane**:

1. Na plohu **Azure AD identiteta zaštita** kliknite **Postavke**.
<br><br>
![Korisnička rizika pravila](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
<br>

2. U odjeljku **Općenito** kliknite **obavijesti**.
<br><br>
![Korisnička rizika pravila](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
<br>




## <a name="see-also"></a>Vidi također

- [Zaštita identiteta Azure Active Directory](active-directory-identityprotection.md) 

