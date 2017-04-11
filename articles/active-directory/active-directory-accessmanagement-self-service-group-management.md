<properties
    pageTitle="Postavljanje Azure Active Directory za upravljanje pristupom prošao na samostalnom servisnim aplikacijama | Microsoft Azure"
    description="Grupa samostalno Upravljanje korisnicima omogućuje stvaranje i upravljanje sigurnosnim grupama ili Office 365 grupe u servisu Azure Active Directory i omogućuje korisnicima zahtjev sigurnosne grupe ili članstva u grupi sustava Office 365"
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
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Postavljanje Azure Active Directory za upravljanje samostalno grupe

Grupa samostalno Upravljanje korisnicima omogućuje stvaranje i upravljanje sigurnosnim grupama ili grupe sustava Office 365 u Azure Active Directory (Azure AD). Korisnicima možete zatražiti i sigurnosne grupe ili članstva u grupi sustava Office 365, a zatim vlasnika grupe možete odobravanje ili odbijanje članstvo. Na taj način svakodnevno kontrolu nad članstvo u grupi možete dodijeljeno osobama razumijevanje tvrtke kontekst za taj članstvom. Grupa samostalno upravljanje značajke dostupne su samo za sigurnosnim grupama i grupama sustava Office 365, ali ne sigurnosnim grupama s podrškom za poštu ili popisima za raspodjelu.

Upravljanje samostalno grupe trenutno sastoji se od dva ključna scenarija: Upravljanje grupe i upravljanje samostalno grupe uz prijenos ovlasti.

- **Upravljanje grupe uz prijenos ovlasti** 
   primjer je administrator koji je upravljanje pristupom SaaS aplikacije koja koristi tvrtke. Upravljanje tih prava pristupa postaje naporan, tako da se ovaj administrator pita vlasnik tvrtke da biste stvorili novu grupu. Administrator dodjeljuje pristup za aplikaciju u novu grupu, a sve osobe koje se već pristup aplikaciji dodaje u grupu. Vlasnik tvrtke pa možete dodati više korisnika, a ti korisnici automatski dodijeljeni resursi aplikaciji. Vlasnik tvrtke ne mora proći administrator za upravljanje pristupom za korisnike. Ako administrator daje iste dozvole Upravitelj u drugoj poslovnoj grupe, a zatim toj osobi možete upravljati pristupom za svoje korisnike. Vlasnik tvrtke ni Upravitelj možete prikazati ili upravljanje promjene koje su drugi korisnici. Administrator i dalje možete vidjeti sve korisnike koji imaju pristup aplikacije i prava pristupa bloka prema potrebi.

- **Upravljanje samostalno grupe** 
   primjer scenarij je dva korisnika i SharePoint Online web-mjestima koja su postavljena neovisno. Žele da biste omogućili tuđe timovima pristup web-mjesta. Da biste to postigli, mogu stvoriti jedne grupe u Azure AD i u sustavu SharePoint Online za svaku od njih odabire tu grupu za pristup web-mjesta. Kada netko želi programa access, oni se zatražite od ploča programa Access, a nakon odobrenje mogu pristupiti oba web-mjesta sustava SharePoint Online automatski. Jedan od njih kasnije odluči da sve osobe pristupa web-mjesta i dobiti pristup određenu aplikaciju SaaS. Administrator programa SaaS možete dodati prava pristupa za aplikaciju za web-mjesta sustava SharePoint Online. Od tada nadalje sve zahtjeve za koje se odobrene omogućuje pristup dva web-mjesta sustava SharePoint Online i ovu aplikaciju SaaS.

## <a name="making-a-group-available-for-end-user-self-service"></a>Dostupnost grupe za krajnjeg korisnika samostalno

1. [Azure klasični portal](https://manage.windowsazure.com)otvorite Azure AD direktorija.

2. Na kartici **Konfiguriraj** postavite **ovlašteni grupa Upravljanje** na omogućeno.

3. **Korisnici mogu stvarati sigurnosne grupe** ili **korisnici mogu stvarati grupama sustava Office** postavite na omogućeno.

Kada **korisnici mogu stvarati sigurnosnih grupa** je omogućen, svi korisnici u direktoriju dopušteno stvaranje nove sigurnosne grupe i dodali članove grupe. Ove nove grupe bi prikazuju se na ploči pristupa za druge korisnike. Ako postavka pravilnika u grupi to dopušta, drugi korisnici mogu stvarati zahtjeve za pridruživanje te grupe. Ako **korisnici mogu stvarati sigurnosne grupe** je onemogućen, korisnici ne može stvarati grupe i ne možete promijeniti postojeće grupe za koje su vlasnika. Međutim, mogu i dalje upravljanje članstvima te grupe i odobravanje zahtjeva za od drugih korisnika da biste se uključili njihov grupe.

Da biste postigli više preciznije kontrola pristupa putem upravljanje samostalno grupe za korisnike možete koristiti i **korisnici tko može koristiti samostalne za sigurnosne grupe** . Kada je omogućen **korisnici mogu stvarati grupe** , svi korisnici u direktoriju dopušteno stvaranje nove grupe i dodali članove grupe. Postavljanjem i **korisnici tko može koristiti samostalne za sigurnosne grupe** s nekim, ste restricting upravljanje samo ograničeni grupu korisnika grupe. Kada taj parametar postavljen na neke, morate Dodavanje korisnika u grupu SSGMSecurityGroupsUsers prije stvaranje nove grupe i dodali članove. Postavljanjem **korisnici tko može koristiti samostalne za sigurnosne grupe** na sve omogućiti sve korisnike u direktoriju za stvaranje nove grupe.

Da biste odredili prilagođeni naziv za grupu čiji članovi mogu koristiti samostalne možete koristiti i okvir **grupe koje možete koristiti samostalne za sigurnosne grupe** .

## <a name="additional-information"></a>Dodatne informacije

Ti članci sadrže dodatne informacije na Azure Active Directory.

* [Upravljanje pristupom resursima s grupama Azure Active Directory](active-directory-manage-groups.md)

* [Cmdleti za Azure Active Directory za konfiguriranje postavki grupe](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)

* [Što je Azure Active Directory?](active-directory-whatis.md)

* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
