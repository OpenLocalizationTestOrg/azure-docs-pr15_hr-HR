<properties
    pageTitle="Azure Active Directory PowerShell pretpregled cmdleti za upravljanje grupe u Azure AD | Microsoft Azure"
    description="Ova stranica sadrži primjere ljuske PowerShell za upravljanje grupe u servisu Azure Active Directory"
    keywords="Ljuske PowerShell za Azure AD, Azure Active Directory, upravljanje grupama, grupe"
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
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory pretpregled cmdleti za upravljanje grupe

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-groups-create-azure-portal.md)
- [Azure klasični portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Sljedeći dokument dat će vam Primjeri kako pomoću ljuske PowerShell za upravljanje grupe servisa Azure Active Directory (Azure AD).  Sadrži i informacije o tome kako postaviti u programu pretpregled modul Azure AD PowerShell. Najprije morate [preuzeti modul Azure AD PowerShell](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>Instaliranje modula Azure AD PowerShell

Da biste instalirali na AzureAD modul ljuske PowerShell pretpregled, koristite sljedeće naredbe:

    PS C:\Windows\system32> install-module azureadpreview

Da biste provjerili je li instaliran modul za pretpregled, koristite sljedeću naredbu:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

Sada možete početi koristiti s cmdletima u modulu. Puni opis cmdleta u modulu AzureAD pretpregled, pogledajte [online referentnu dokumentaciju](https://msdn.microsoft.com/library/azure/mt757216.aspx).

## <a name="connecting-to-the-directory"></a>Povezivanje s imenika
Prije no što počnete Upravljanje grupama pomoću cmdleta ljuske PowerShell za Azure AD pretpregled, morate se povezati sesiju ljuske PowerShell imenik želite upravljati. Da biste to učinili, koristite sljedeću naredbu:

    PS C:\Windows\system32> Connect-AzureAD -Force

Cmdlet će vas tražiti vjerodajnice koje želite koristiti da biste pristupili direktorija. U ovom primjeru ćemo pomoću karen@drumkit.onmicrosoft.com da biste pristupili Demonstracija direktorija. Cmdlet će vratiti potvrdu da bi se prikazala sesiju uspješno je povezan s direktorija:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Sada možete početi koristiti pretpregled cmdleta AzureAD Upravljanje grupama u direktoriju.

## <a name="retrieving-groups"></a>Dohvaćanje grupe
Dohvaćanje postojeće grupe iz direktorija možete pomoću cmdleta Get-AzureADGroups. Da biste dohvatili sve grupe u imeniku, koristite cmdlet bez parametara:

    PS C:\Windows\system32> get-azureadgroup

Cmdlet će vratiti sve grupe u imeniku povezani.

Parametar ID - objekta možete koristiti za dohvaćanje određene grupe za koje navedete ID objekta grupe:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Cmdlet sada će vratiti grupu čiji ID objekta usklađuje sljedeću vrijednost parametra koje ste unijeli:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Možete tražiti određene grupe pomoću-parametar filtra. Taj parametar koristi se izraz filtra ODATA i vraća sve grupe koje se podudaraju s filtrom, kao u sljedećem primjeru:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Imajte na umu da cmdleta ljuske AzureAD PowerShell implementirati OData query standard, moguće je pronaći dodatne informacije [u nastavku](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Stvaranje grupa
Da biste stvorili novu grupu u direktoriju, pomoću cmdleta New-AzureADGroup. Ovaj cmdlet stvara novi sigurnosne grupe naziva "Marketing":

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Ažuriranje grupe
Da biste ažurirali postojeću grupu, koristite cmdlet skup AzureADGroup. U ovom primjeru smo mijenjate svojstvo riješiti problem grupe "Administratori Intune." Najprije ćemo naći grupe pomoću cmdleta Get-AzureADGroup i filtrirati pomoću atribut riješiti problem:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Zatim ćemo mijenjate svojstvo opis za novu vrijednost "Intune uređaj administratori":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Sada Vidimo da ako ponovno ne možemo pronaći grupi, svojstvo opis se ažuriraju u skladu s vizualnim novu vrijednost:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Brisanje grupe
Da biste izbrisali grupe iz direktorija, koristite cmdlet Ukloni AzureADGroup na sljedeći način:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Upravljanje članovima grupe
Ako trebate dodati nove članove u grupu, koristite cmdlet za dodavanje AzureADGroupMember. Ta se naredba dodaje član grupe administratora Intune koji se koristi u prethodnom primjeru:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Parametar ID - objekta ID objekta grupe u koje želimo da biste dodali člana, a - RefObjectId ID objekta želimo da biste dodali člana u grupu korisnika.

Da biste postojeće članove grupe, koristite cmdlet Get-AzureADGroupMember, kao u ovom primjeru:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Da biste uklonili člana koje ste prethodno dodali smo u grupu, koristite cmdlet Ukloni AzureADGroupMember, kao što je prikazano ovdje:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Da biste provjerili membership(s) grupe korisnika, koristite cmdlet odaberite AzureADGroupIdsUserIsMemberOf. Ovaj cmdlet uzima kao parametara ID objekta za koju želite provjeriti članstva u grupi korisnika i popis grupa za koju želite provjeriti članstva. Na popisu grupa mora biti navedeno u obrascu kompleksnog varijable vrste "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" pa ćemo najprije morate stvoriti tjednog prikaza kalendara s tom vrstom:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Nakon toga dajemo vrijednosti za groupIds da biste provjerili u atributu "GroupIds" u ovom varijable kompleksnog:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Sada ako želimo da biste provjerili članstva u grupi korisnika s 72cd4bbd-2594-40a2-935c-016f3cfeeeea ID objekta u odnosu na grupe u $g, trebali biste koristimo:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Vraćena vrijednost je popis grupa čiji je taj korisnik član. Možete primijeniti taj način da biste provjerili kontakata, grupe ili upravitelji servisa članstvo navedeni popis grupa, a zatim odaberite AzureADGroupIdsContactIsMemberOf, odaberite AzureADGroupIdsGroupIsMemberOf ili odaberite AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="managing-owners-of-groups"></a>Upravljanje vlasnika grupe
Da biste dodali vlasnika grupe, koristite cmdlet za dodavanje AzureADGroupOwner:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Parametar ID - objekta ID objekta grupe u koje želimo da biste dodali vlasnika, a - RefObjectId ID objekta želimo da biste dodali kao vlasnika grupe korisnika.

Da biste dohvatili vlasnici grupe, koristite Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Cmdlet će vratiti popis vlasnika za navedenu grupu:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Ako želite da biste uklonili vlasnika iz grupe, koristite Ukloni AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Daljnji koraci

Dodatne dokumentaciju Azure Active Directory PowerShell možete pronaći na [Cmdleti za Azure Active Directory](http://go.microsoft.com/fwlink/p/?LinkId=808260).

* [Upravljanje pristupom resursima s grupama Azure Active Directory](active-directory-manage-groups.md)

* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
