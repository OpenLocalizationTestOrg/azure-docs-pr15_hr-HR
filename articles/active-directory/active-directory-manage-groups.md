<properties
    pageTitle="Upravljanje pristupom resursima s grupama Azure Active Directory | Microsoft Azure"
    description="Upute za korištenje grupa servisa Azure Active Directory za upravljanje korisničkim pristupom lokalnog i oblaka aplikacija i resursa."
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


# <a name="managing-access-to-resources-with-azure-active-directory-groups"></a>Upravljanje pristupom resursima s grupama Azure Active Directory

Azure Active Directory (Azure AD) je potpun identiteta i pristup upravljanje rješenje koje nudi robusni skup mogućnosti za upravljanje pristupom lokalnog i oblaka aplikacija i resursa uključujući Microsoft online services kao što su Office 365 i svijeta koji nisu iz programa Microsoft SaaS aplikacija. Ovaj članak sadrži pregled, ali ako želite da biste počeli koristiti grupe za Azure AD odmah, slijedite upute u [Upravljanje sigurnosnim grupama u Azure AD](active-directory-accessmanagement-manage-groups.md). Ako želite da biste saznali kako koristiti ljuske PowerShell za upravljanje grupama u servisu Azure Active directory Saznajte više u [Azure Active Directory pretpregled cmdleti za upravljanje grupe](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).


> [AZURE.NOTE] Da biste koristili Azure Active Directory, potreban vam je račun za Azure. Ako nemate račun, možete je [prijaviti se za besplatnu račun za Azure](https://azure.microsoft.com/pricing/free-trial/).


Unutar Azure AD jedna od glavnih značajki je mogućnost Upravljanje pristupom resursima. Ove resurse može biti dio imenika, kao u slučaju dozvola za upravljanje objekte kroz ulogama u imenik ili resursi koji se nalaze izvan direktorija, kao što su SaaS aplikacije, Azure services i web-mjesta sustava SharePoint ili na lokalnu instancu resursi. Korisnik može dodijeliti prava pristupa resursu na četiri načina:


1. Izravni dodjele

    Korisnicima se mogu dodijeliti izravno resursa vlasnik resursa.

2. Članstvo u grupi

    Grupe mogu dodijeliti resurs vlasnik resursa i na taj način, dodjelu članovi te grupe pristup resursu. Članstvo grupe pa upravljati njime mogu vlasnika grupe. Učinkovito, vlasnik resursa delegates dozvole da biste korisnicima dodijelili njihove resurs vlasnika grupe.

3. Na temelju pravila

    Vlasnik resursa možete koristiti pravila za express korisnike koji treba dodijeliti pristup resursu. Rezultat pravilo ovisi o atributi koji se koriste u to pravilo i njihovih vrijednosti za određene korisnike i na taj način, vlasnik resursa učinkovito delegates pravo da upravljaju pristup svojim resurs pouzdanih izvora za atribute koje se koriste u pravilo. Vlasnik resursa i dalje upravlja pravilo sam i određuje koje atributa i vrijednosti omogućuje pristup svojim resursa.

4. Vanjski za izdavanje certifikata

    Pristup resursu proizlazi iz vanjskog izvora; na primjer, grupa koji se sinkroniziraju iz pouzdanih izvora kao što su lokalnog imenika ili web-aplikacijom SaaS kao što su dana. Vlasnik resursa dodjeljuje grupu koju želite omogućiti pristup resursu i vanjski izvor upravlja članove grupe.

  ![Pregled dijagrama Upravljanje pristupom](./media/active-directory-access-management-groups/access-management-overview.png)


## <a name="watch-a-video-that-explains-access-management"></a>Pogledajte videozapis u kojem se objašnjava Upravljanje pristupom

Možete pogledajte kratki videozapis u kojem se objašnjava dodatne informacije o tome:

**Azure AD: Uvod u dinamičku članstvo u grupama**

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Kako pristupiti upravljanje u Azure Active Directory funkcioniraju?
U središtu Azure AD rješenje za upravljanje pristupom je sigurnosne grupe. Korištenje sigurnosne grupe za upravljanje pristupom resursima je dobro poznatog paradigm koja omogućuje prilagodljivo i jednostavno prihvaćeni način omogućuje pristup resursu za svrhu grupe korisnika. Vlasnik resursa (ili administrator direktorija) možete dodijeliti grupu u koju želite određene pristup desno na resurse oni vlasnik. Članovi grupe objavit ćemo zajedno s pristupom, a vlasnik resursa možete delegirati ograničenu pravo da upravljaju popis članova grupe nekome drugome, kao što su upravitelju odjela ili služba za administratora.

![Azure Active Directory pristup upravljanje dijagrama](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

Vlasnik grupe možete promijeniti tu grupu dostupna samostalno zahtjeva za. Tako, krajnji korisnik možete tražiti i pronađite grupu i provjerite zahtjev za pridruživanje, učinkovito traženje dozvolu za pristup resursa koji upravlja se putem grupe. Vlasnik grupe možete postaviti grupu tako da se zahtjevi za uključivanje automatski odobrene ili potrebno odobrenje vlasnik grupe. Kada korisnik napravi zahtjev za pridruživanje grupi, zahtjev za uključivanje prosljeđuju se vlasnici grupe. Ako neku od vlasnici Odobri zahtjev, prima obavijest korisnika i korisniku se pridružite grupi. Ako je jedan od vlasnici onemogućava zahtjev, korisnika je obavijest, ali niste član grupe.


## <a name="getting-started-with-access-management"></a>Početak rada s upravljanjem programa access
Jeste li spremni za početak rada? Što treba isprobate neke osnovne zadatke koje možete učiniti s Azure AD grupe. Pomoću ove mogućnosti omogućuju pristup specijalizirane različitim grupama osoba za različite resursa u vašoj tvrtki ili ustanovi. Popis osnovnih prvi koraci navedena su u nastavku.

* [Stvaranje pravila za jednostavne da biste konfigurirali dinamički članstva za grupu](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)

* [Korištenje grupe da biste upravljali pristup aplikacijama SaaS](active-directory-accessmanagement-group-saasapps.md)

* [Dostupnost grupe za krajnjeg korisnika samostalno](active-directory-accessmanagement-self-service-group-management.md)

* [Sinkroniziranje grupe lokalnog sustava za Azure pomoću Azure AD Connect](active-directory-aadconnect.md)

* [Upravljanje vlasnika grupe](active-directory-accessmanagement-managing-group-owners.md)


## <a name="next-steps-for-access-management"></a>Daljnji koraci za upravljanje pristupom
Sad kad ste razumjeti osnove upravljanja programa access, ovdje neke dodatne naprednih mogućnosti dostupne su u Azure Active Directory za upravljanje pristupom aplikacija i resursa.

* [Korištenje atributa za stvaranje naprednih pravila](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [Upravljanje sigurnosnim grupama u Azure AD](active-directory-accessmanagement-manage-groups.md)

* [Postavljanje namjenski grupa u Azure AD](active-directory-accessmanagement-dedicated-groups.md)

* [Pregled grafikonu API-JA za grupe](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

* [Cmdleti za Azure Active Directory za konfiguriranje postavki grupe](active-directory-accessmanagement-groups-settings-cmdlets.md)
