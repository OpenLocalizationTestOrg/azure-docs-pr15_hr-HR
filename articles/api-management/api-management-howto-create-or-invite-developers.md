<properties 
    pageTitle="Kako upravljati korisničkim računima u upravljanju API Azure | Microsoft Azure" 
    description="Saznajte kako stvarati i pozivanje korisnika u upravljanju API Azure" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Kako upravljati korisničkim računima u upravljanju API Azure

U odjeljku Upravljanje API-JA, razvojni inženjeri mogu korisnicima API-ji koji izlažu značajkom upravljanja API-JA. Ovaj vodič prikazuje kako stvoriti i pozovite razvojnim inženjerima korištenja API-ji i proizvodi koje unesete dostupni na njih vašoj instanci upravljanje API-JA. Informacije o upravljanju korisničkim računima programski potražite u dokumentaciji [entitet korisnika](https://msdn.microsoft.com/library/azure/dn776330.aspx) u referenci [REST API upravljanje](https://msdn.microsoft.com/library/azure/dn776326.aspx) .

## <a name="create-developer"> </a>Stvaranje nove razvojni inženjer

Da biste stvorili novi programiranje, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA. Tako ćete doći do portala za upravljanje API programa publisher. Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

![Portal za Publisher][api-management-management-console]

Kliknite **korisnici** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim **Dodaj korisnika**.

![Stvaranje za razvojne inženjere][api-management-create-developer]

Unesite **e-pošte**, **lozinku**i **naziv** za novu programiranje, a zatim kliknite **Spremi**.

![Stvaranje za razvojne inženjere][api-management-add-new-user]

Prema zadanim postavkama računa za razvojne inženjere novostvorenu **aktivnih**, a pridružene grupi **razvojni inženjeri** .

![Novi za razvojne inženjere][api-management-new-developer]

Za razvojne inženjere račune koji su u stanju **aktivni** se može koristiti da biste pristupili svim API-ji za koje imaju pretplate. Želite pridružiti novostvorenu programiranje dodatne grupe, potražite [u][]članku združivanje s razvojnim inženjerima.

## <a name="invite-developer"> </a>Pozivanje razvojni inženjer

Da biste pozvali razvojni inženjer, na izborniku **Upravljanje API -JA** na lijevoj strani kliknite **korisnici** , a zatim **Pozivanje korisnika**.

![Pozivanje za razvojne inženjere][api-management-invite-developer]

Unesite adresu za razvojne inženjere ime i e-pošte pa kliknite **Pozovi**.

![Pozivanje za razvojne inženjere][api-management-invite-developer-window]

Prikazuje se poruka potvrde, ali nedavno pozvani programiranje ne prikazuje na popisu do kada je prihvate. 

![Pozivanje potvrdu][api-management-invite-developer-confirmation]

Kada je pozvan razvojni inženjer programer se šalje poruku e-pošte. U ovom e-pošte generira pomoću predloška i moguće prilagoditi. Dodatne informacije potražite u članku [Konfiguriranje predlošci e-pošte][].

Kada poziv bude prihvaćen, račun postaje aktivna.

## <a name="block-developer"></a> Deaktiviraj ili Ponovna aktivacija računa za razvojne inženjere

Prema zadanim postavkama računa ili ponovno konfiguriranih pozvanih programiranje su **aktivne**. Da biste deaktivirali račun za razvojne inženjere, kliknite **Blok**. Da biste ponovno aktivirali blokirane za razvojne inženjere račun, kliknite **Aktiviraj**. Blokirani programiranje računa možete pristupiti portala za razvojne inženjere ili poziva sve API-ji. Da biste izbrisali korisnički račun, kliknite **Izbriši**.

![Blok za razvojne inženjere][api-management-new-developer]

## <a name="reset-a-user-password"></a>Ponovno postavljanje lozinke za korisnika

Da biste ponovno postavili lozinku za korisnički račun, kliknite naziv računa.

![Ponovno postavljanje lozinke][api-management-view-developer]

Kliknite **ponovno postaviti lozinku** da biste poslali vezu na korisnika da biste ponovno postavili svoju lozinku.

![Ponovno postavljanje lozinke][api-management-reset-password]

Programski radili s korisničkim računima, potražite u članku [entitet korisnika](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentacije referencu [REST API upravljanje](https://msdn.microsoft.com/library/azure/dn776326.aspx) . Da biste ponovno postavljanje lozinke korisničkog računa na određenu vrijednost, možete koristiti operacija [Ažuriranje korisnika](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) i navedite željenu lozinku.

## <a name="pending-verification"></a>Provjera na čekanju

![Provjera na čekanju][api-management-pending-verification]

## <a name="next-steps"> </a>Sljedeće korake

Nakon stvaranja računa za razvojne inženjere možete pridružiti ulogama i pretplata na proizvodi i API-ji. Dodatne informacije potražite [u][]članku Stvaranje i korištenje grupe.


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Kako stvoriti i koristiti grupe]: api-management-howto-create-groups.md
[Kako se pridružiti grupe za razvojne inženjere]: api-management-howto-create-groups.md#associate-group-developer

[Početak rada s upravljanjem API Azure]: api-management-get-started.md
[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance
[Konfiguriranje predložaka e-pošte]: api-management-howto-configure-notifications.md#email-templates