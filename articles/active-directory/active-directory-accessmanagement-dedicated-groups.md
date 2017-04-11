<properties
    pageTitle="Namjenski grupe u servisu Azure Active Directory | Microsoft Azure"
    description="Pregled kako namjenski grupe funkcioniraju u Azure Active Directory i kako se stvaraju."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="dedicated-groups-in-azure-active-directory"></a>Namjenski grupe u servisu Azure Active Directory

U Azure Active Directory (Azure AD), namjenski grupe značajka automatski stvara i popunjava članstva za Azure AD unaprijed definirane grupe. Članovi grupa namjenski nije moguće dodati ili ukloniti pomoću Azure klasični portal, cmdleta ljuske Windows PowerShell ili programski.

>[AZURE.NOTE] Namjenski grupe zahtijevaju da je dodijeljen licencu za Azure AD Premium
>- administrator koja upravlja pravilo na grupu
>- Svi korisnici koji označavaju pravilom biti član grupe

**Da biste omogućili namjenski grupe**

1. [Azure klasični portala](https://manage.windowsazure.com)odaberite **Servisa Active Directory**, a zatim otvorite imenik tvrtke ili ustanove.

2. Odaberite karticu **grupe** , a zatim otvorite grupu koju želite urediti.

3. Odaberite karticu **Konfiguracija** , a zatim **Omogućiti namjenski grupe** postavljeno na **da**.

Kad je parametar omogućiti namjenski grupe postavljeno na **da**, Dodatno možete omogućiti direktorija da biste automatski stvorili grupi namjenski svim korisnicima tako da postavite na **omogućiti "Svi korisnici" grupe** prijeđite na **da**. Zatim možete i urediti naziv grupe namjenski upisivanjem u na **zaslonsko ime "Svim korisnicima" grupe** polja.

Grupa za sve korisnike se može koristiti da biste dodijelili iste dozvole za sve korisnike u direktoriju. Na primjer, možete dodijeliti svim korisnicima u imenik pristup SaaS aplikacije dodjeljivanjem pristupa za grupu za sve korisnike za namjenski ove aplikacije.

Svi korisnici grupi namjenski obuhvaća sve korisnike u direktoriju, uključujući Gosti i vanjskim korisnicima. Ako vam je potrebna grupe koje su izostavljene vanjskih korisnika, a zatim koje možete izvršiti Stvorite grupu sa sustavom atribut dinamički pravilo kao što je sljedeće:

                (user.userPrincipalName -notContains "#EXT#@")

Za grupu isključuje sve Gosti, koristite pravila kao što je sljedeće:

                (user.userType -ne "Guest")

Da biste saznali više o stvaranju *naprednih* pravila (pravila koja se može sadržavati više usporedbe) za članstvo u grupama za dinamičku, potražite u članku [korištenje atributa stvaranju naprednih pravila](active-directory-accessmanagement-groups-with-advanced-rules.md).


Ti članci sadrže dodatne informacije na Azure Active Directory.

* [Upravljanje pristupom resursima s grupama Azure Active Directory](active-directory-manage-groups.md)
* [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
* [Što je Azure Active Directory?](active-directory-whatis.md)
* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
