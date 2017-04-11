<properties
    pageTitle="Azure AD Connect: Daljnji koraci i kako upravljati Azure AD Connect | Microsoft Azure"
    description="Saznajte kako proširiti zadanu konfiguraciju i operativnih zadataka za Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Daljnji koraci i kako upravljati Azure AD Connect
Sljedeće dodatne su bile temama koje omogućuju vam prilagodbu Azure Active Directory povezati odgovaraju potrebama vaše tvrtke ili ustanove i preduvjeti.  

## <a name="add-additional-sync-administrators"></a>Dodavanje dodatnih sinkronizaciju administratora
Prema zadanim postavkama samo korisnika koji nije za instalaciju i lokalni administratori će moći upravljanje modul instaliranih sinkronizaciju. Za druge osobe da biste mogli pristupiti i upravljanje modula za sinkroniziranje pronađite grupu pod nazivom ADSyncAdmins na lokalnom poslužitelju i njihovo dodavanje u ovoj grupi.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Dodjela licence korisnicima Azure AD Premium i mobilnost za tvrtke

Sad kad korisnici imati sinkroniziran u oblak, morat ćete im dodijeliti licence tako da možete početi raditi s aplikacijama u oblaku kao što je Office 365.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Da biste dodijelili Azure AD Premium ili Enterprise mobilnost paket licence
--------------------------------------------------------------------------------
1. Prijava na Portal za Azure kao Administrator.
2. Na lijevoj strani odaberite **Servisa Active Directory**.
3. Na stranici servisa Active Directory, dvokliknite imenika korisnike kojima želite omogućiti.
4. Pri vrhu stranice direktorija, odaberite **licence**.
5. Na stranici licence odaberite Active Directory Premium ili paket mobilnost za tvrtke, a zatim **dodijeliti**.
6. U dijaloškom okviru odaberite korisnike koje želite dodijeliti licence, a zatim ikonu kvačice da biste spremili promjene.


## <a name="verifying-the-scheduled-synchronization-task"></a>Provjera zakazanu sinkronizaciju zadatka
Ako želite provjeriti stanja sinkronizacije to možete učiniti tako da na portalu za Azure.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Da biste provjerili zakazanu sinkronizaciju zadatka
--------------------------------------------------------------------------------
1. Prijava na Portal za Azure kao Administrator.
2. Na lijevoj strani odaberite **Servisa Active Directory**.
3. Na stranici servisa Active Directory, dvokliknite imenika korisnike kojima želite omogućiti.
4. Pri vrhu stranice direktorija, odaberite **Integracija direktorija**.
5. U odjeljku Integracija s lokalnim servisom active directory bilješke vrijeme zadnje sinkronizacije.

<center>![Oblak](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Pokretanje zadatka zakazanu sinkronizaciju
Ako morate pokrenuti sinkronizaciju zadatka učinite to tako da pokrenete kroz Čarobnjak za Azure AD Connect.  Morat ćete unijeti vjerodajnice za Azure AD.  U čarobnjaku odaberite zadatak **mogućnosti sinkronizacije Prilagodi** , a zatim kliknite Dalje kroz čarobnjak. Na kraju, provjerite je li potvrđen okvir **pokretanje postupka za sinkronizaciju čim početnu konfiguraciju dovrši** .

<center>![Oblak](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Dodatne informacije o sinkronizaciji Azure AD Connect: rasporeda, potražite u članku [Povezivanje raspored Azure AD](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>Dodatne zadatke koji su dostupni u Azure AD Connect
Nakon početne instalacije programa Azure AD Connect, možete uvijek pokrenuti čarobnjak ponovno s Azure AD Connect početak stranice ili radne površine prečaca.  Primijetit ćete da ponovno prolaze kroz Čarobnjak nudi nekoliko novih mogućnosti u obliku dodatne zadatke.  

Sljedeća tablica sadrži sažetak zadaci i kratak opis za svaku od njih.

![Uključivanje pravila](./media/active-directory-aadconnect-whats-next/addtasks.png)


Dodatni zadatka | Opis
------------- | ------------- |
Prikaz odabrane scenarija  |Omogućuje vam da biste vidjeli svoje trenutne Azure AD Connect rješenje.  To obuhvaća opće postavke sinkronizirane mape, postavke za sinkronizaciju, itd.
Prilagodba mogućnosti za sinkronizaciju | Omogućuje promjenu Trenutna konfiguracija uključujući dodavanje dodatnih šuma servisa Active Directory za konfiguraciju ili omogućivanjem mogućnosti sinkronizacije kao što je korisnik, grupa, uređaj ili lozinku pisanje natrag.
Omogućivanje načina rada za pripremna |  To vam omogućuje da podaci o fazi koja će se poslije sinkronizirati, ali ništa će se izvesti Azure AD ili servisa Active Directory.  Omogućuje pretpregled na sinkronizacijom prije nastaju.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
