<properties
    pageTitle="Cmdleti za Azure Active Directory za konfiguriranje postavki grupe | Microsoft Azure"
    description="Kako upravljati postavkama za grupe pomoću cmdleta Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Cmdleti za Azure Active Directory za konfiguriranje postavki grupe

Sljedeće postavke za Sjedinjeno komuniciranje grupe mogu se konfigurirati u direktoriju:

1.  Klasifikacije: odvojenih zarezom popis klasifikacije koje korisnici mogu postaviti na grupu. Primjeri bio "Classified", "Tajna" i "Gornji tajna."

2.  Korištenje smjernice URL: URL koji korisnike upućuje na uvjete korištenja za korištenje grupa za Sjedinjeno komuniciranje onako kako su definirana vaša tvrtka ili ustanova. Ovaj URL prikazat će se u korisničkom sučelju koje će korisnici koristiti grupe.

3.  Grupiranje stvaranja omogućen: li ništa, neke ili sve korisnici će moći stvarati grupe za Sjedinjeno komuniciranje. Kada postavite na uključeno, svi korisnici mogu stvarati grupe. Kada je postavljeno na isključeno, bez korisnici mogu stvarati grupe. Kada isključiti, možete i navesti sigurnosne grupe čiji korisnicima koji i dalje dopušteno stvaranje grupe.

Ove postavke konfigurirane u postavkama i SettingsTemplate objekata. Na početku, nećete vidjeti sve objekte postavke u direktoriju. To znači da direktorija konfiguriran je prema zadanim postavkama. Da biste promijenili zadane postavke, morate stvoriti novi objekt postavke pomoću predloška za postavke. Postavke predložaka definira Microsoft.

Možete preuzeti modul koji sadrži cmdleta za te operacije s [web-mjesta Microsoft za povezivanje](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).

## <a name="create-settings-at-the-directory-level"></a>Stvaranje postavke na razini direktorija

Ove korake stvoriti postavke na razini direktorija, a primijenit će se sve grupe sustava Office u direktoriju.

1. Ako ne znate koje SettingTemplate da biste koristili, ovaj cmdlet vraća popis predložaka postavke:

    `Get-MsolAllSettingTemplate`

    ![Popis predložaka postavke](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Da biste dodali korištenje većinom URL-a, prvo morate dobiti SettingsTemplate objekt koji definira vrijednost URL smjernica za korištenje; To je predložak Group.Unified:

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Zatim stvorite novi objekt postavke na temelju tog predloška:

    `$setting = $template.CreateSettingsObject()`

4. Zatim ažurirajte vrijednost smjernica za korištenje:

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. Na kraju, primijeniti postavke:

    `New-MsolSettings –SettingsObject $setting`

    ![Dodajte URL smjernica za korištenje](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Evo postavke definirano u Group.Unified SettingsTemplate.

 **Postavka**                          | **Opis**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Vrsta: niza<li>Zadani: ""                  | Razdvojen zarezom popisa valjanih klasifikacija vrijednosti koje se mogu primijeniti u sjedinjenom komuniciranju grupe.                
 <ul><li>EnableGroupCreation<li>Vrsta: Booleova<li>Zadani: True              | Zastavica koja označava je li dopušten stvaranja grupe Unified u direktoriju.                               
 <ul><li>GroupCreationAllowedGroupId<li>Vrsta: niza<li>Zadani: ""         | GUID sigurnosne grupe koje je dopušteno stvaranje grupe za Sjedinjeno komuniciranje čak i ako EnableGroupCreation == false.
 <ul><li>UsageGuidelinesUrl<li>Vrsta: niza<li>Zadani: ""                  | Veza na smjernice za korištenje grupa.                                                                       

## <a name="read-settings-at-the-directory-level"></a>Postavke za čitanje na razini direktorija

Korake u nastavku pročitajte postavke na razini direktorija, koje se primjenjuju na svim grupama sustava Office u direktoriju.

1. Pročitajte postojeće postavke direktorija:

    `Get-MsolAllSettings`

2. Pročitajte sve postavke za određenu grupu:

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Pročitajte postavke određene directory pomoću SettingId GUID:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![Postavke ID GUID](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>Ažuriranje postavki na razini direktorija

Ove korake Ažurirajte postavke na razini direktorija, a primijenit će se sve grupe sustava Office u direktoriju.

1. Pojavljuje se postojeće postavke objekta:

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. Pojavljuje se vrijednost koju želite ažurirati:

    `$value = $Setting.GetSettingsValue()`

3. Ažuriranje vrijednosti:

    `$value["AllowToAddGuests"] = "false"`

4. Ažurirajte postavke:

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>Uklanjanje postavki na razini direktorija

Ovaj korak uklanja postavke na razini direktorija, koje se primjenjuju na svim grupama sustava Office u direktoriju.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Referenca za cmdlet sintaksu

Dodatne dokumentaciju Azure Active Directory PowerShell možete pronaći na [Cmdleti za Azure Active Directory](http://go.microsoft.com/fwlink/p/?LinkId=808260).

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>Referenca SettingsTemplate objekta (Group.Unified SettingsTemplate objekt)

- "naziv": "EnableGroupCreation", "Vrsta": "System.Boolean", "defaultValue": "true", "Opis": "Booleova zastavicu koji pokazuje Ako značajku stvaranja grupe Unified uključena."

- "naziv": "GroupCreationAllowedGroupId", "Vrsta": "System.Guid", "defaultValue": "", "Opis": "GUID sigurnosne grupe koji je whitelisted za stvaranje grupa Unified".

- "naziv": "ClassificationList", "Vrsta": "System.String", "defaultValue": "", "Opis": "Razdvojen zarezom popisa valjanih klasifikacija vrijednosti koje se mogu primijeniti grupama Unified."

- "naziv": "UsageGuidelinesUrl", "Vrsta": "System.String", "defaultValue": "", "Opis": "Veza na smjernice za korištenje grupa."

ime | Vrsta | defaultValue | Opis
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | "System.Boolean"  | "true"  | "Booleova zastavicu koji pokazuje Ako značajku stvaranja grupe Unified uključena."
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | "GUID sigurnosne grupe koji je whitelisted da biste stvorili Sjedinjeno komuniciranje grupe".
"ClassificationList"  | "System.String"  | ""  | "Razdvojen zarezom popisa valjanih klasifikacija vrijednosti koje se mogu primijeniti grupama Sjedinjenoj."
"UsageGuidelinesUrl"  | "System.String"  | ""  | "Veza na smjernice za korištenje grupa."

## <a name="next-steps"></a>Daljnji koraci

Dodatne dokumentaciju Azure Active Directory PowerShell možete pronaći na [Cmdleti za Azure Active Directory](http://go.microsoft.com/fwlink/p/?LinkId=808260).

Dodatne upute s upraviteljem programa Microsoft Jong de Robert dostupna je na [Blog Robert na grupe](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad).

* [Upravljanje pristupom resursima s grupama Azure Active Directory](active-directory-manage-groups.md)

* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
