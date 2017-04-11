<properties
    pageTitle="Azure AD Connect sinkronizacije: atributi sinkroniziraju sa servisom Azure Active Directory | Microsoft Azure"
    description="Popis atributa koji se sinkroniziraju sa servisom Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect sinkronizacije: atributi sinkroniziraju sa servisom Azure Active Directory
Ova tema sadrži popis atributa koji se sinkroniziraju Azure AD Connect sinkronizaciju.  
Atributi grupirane prema povezane Azure AD aplikacije.

## <a name="attributes-to-synchronize"></a>Atributi za sinkronizaciju
Uobičajena pitanja je *što je popis minimalne atributa za sinkronizaciju*. Zadane i preporučeni način je održati zadanih atributa cijelog GAL (globalni popis adresa) koje se mogu konstruirana u oblaku i se sve značajke radnih opterećenja sustava Office 365. U nekim slučajevima, postoje neka atributom tvrtke ili ustanove ne sinkronizirani s oblakom jer ti atributi sadrže osjetljive ili PII podataka (osobne podatke), kao u ovom primjeru:  
![neispravni atributa](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

U ovom slučaju, započnite s popis atributa u ovoj temi i prepoznavanje te atribute koje će sadržavati osjetljive ili PII podataka i ne može sinkronizirati. Poništite odabir te atribute tijekom instalacije pomoću [aplikacije za Azure AD atribut filtriranja i](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering).

>[AZURE.WARNING] Kada poništenje odabira atribute, trebali biste Budite oprezni i samo poništite odabir te atribute uistinu nije moguće sinkronizirati. Poništavanjem okvira druge atribute može imati negativan učinak na značajke.

## <a name="office-365-proplus"></a>Office 365 ProPlus

| Naziv atributa| Korisnik| Komentar |
| --- | :-: | --- |
| accountEnabled| X| Definira ako je omogućeno račun.|
| CN| X|  |
| riješiti problem| X|  |
| objectSID| X| mehanički svojstvo. Identifikator korisnika AD koristi da bi se zadržao sinkronizaciju između Azure AD i AD.|
| pwdLastSet| X| mehanički svojstvo. Koristi se kada za onemogućavanje već Izdana tokeni informacije. Koristi sinkronizaciju lozinke i vanjski pristup.|
| sourceAnchor| X| mehanički svojstvo. Immutable identifikator da biste zadržali odnos između ZBRAJA i Azure AD.|
| usageLocation| X| mehanički svojstvo. Država korisnika. Koristi se za dodjelu licenci.|
| userPrincipalName| X| UPN je ID za prijavu za korisnika. Najčešće isto kao [pošte] vrijednost.|

## <a name="exchange-online"></a>Exchange Online

| Naziv atributa| Korisnik| Kontakt| Grupa| Komentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Definira ako je omogućeno račun.|
| Pomoćnik za| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| snimka zaslona poruke o| X| X|  |  |
| tvrtke| X| X|  |  |
| Pozivni| X| X|  |  |
| odjelu| X| X|  |  |
| Opis| X| X| X|  |
| riješiti problem| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| DOM telefon| X| X|  |  |
| Info| X| X| X| Taj atribut trenutno nije potrošena za grupe.|
| Inicijali| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| Upravitelj| X| X|  |  |
| člana|  |  | X|  |
| mobilni| X| X|  |  |
| msDS HABSeniorityIndex| X| X| X|  |
| msDS PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| Taj atribut trenutno ne koriste Exchange Online. |
| msExchExtensionCustomAttribute2| X| X| X| Taj atribut trenutno ne koriste Exchange Online. |
| msExchExtensionCustomAttribute3| X| X| X| Taj atribut trenutno ne koriste Exchange Online. |
| msExchExtensionCustomAttribute4| X| X| X| Taj atribut trenutno ne koriste Exchange Online. |
| msExchExtensionCustomAttribute5| X| X| X| Taj atribut trenutno ne koriste Exchange Online. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| msOrg IsOrganizational|  |  | X|  |
| objectSID| X|  | X| mehanički svojstvo. Identifikator korisnika AD koristi da bi se zadržao sinkronizaciju između Azure AD i AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| Dojavljivač| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Poštanski broj| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | mehanički svojstvo. Koristi se kada za onemogućavanje već Izdana tokeni informacije. Koristi sinkronizaciju lozinke i vanjski pristup.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Izvedene iz groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mehanički svojstvo. Immutable identifikator da biste zadržali odnos između ZBRAJA i Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Naslov| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | mehanički svojstvo. Država korisnika. Koristi se za dodjelu licenci.|
| userCertificate| X| X|  |  |
| userPrincipalName| X|  |  | UPN je ID za prijavu za korisnika. Najčešće isto kao [pošte] vrijednost.|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint Online

| Naziv atributa| Korisnik| Kontakt| Grupa| Komentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Definira ako je omogućeno račun.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| snimka zaslona poruke o| X| X|  |  |
| tvrtke| X| X|  |  |
| Pozivni| X| X|  |  |
| odjelu| X| X|  |  |
| Opis| X| X| X|  |
| riješiti problem| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| hideDLMembership|  |  | X|  |
| DOM telefon| X| X|  |  |
| Info| X| X| X|  |
| Inicijali| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| pošta| X| X| X|  |
| mailnickname| X| X| X|  |
| managedBy|  |  | X|  |
| Upravitelj| X| X|  |  |
| člana|  |  | X|  |
| middleName| X| X|  |  |
| mobilni| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| mehanički svojstvo. Identifikator korisnika AD koristi da bi se zadržao sinkronizaciju između Azure AD i AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| Dojavljivač| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Poštanski broj| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mehanički svojstvo. Koristi se kada za onemogućavanje već Izdana tokeni informacije. Koristi sinkronizaciju lozinke i vanjski pristup.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Izvedene iz groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mehanički svojstvo. Immutable identifikator da biste zadržali odnos između ZBRAJA i Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Naslov| X| X|  |  |
| unauthOrig| X| X| X|  |
| URL-a| X| X|  |  |
| usageLocation| X|  |  | mehanički svojstvo. Država korisnika. Koristi se za dodjelu licenci.|
| userPrincipalName| X|  |  | UPN je ID za prijavu za korisnika. Najčešće isto kao [pošte] vrijednost.|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Lync Online

| Naziv atributa| Korisnik| Kontakt| Grupa| Komentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Definira ako je omogućeno račun.|
| c| X| X|  |  |
| CN| X|  | X|  |
| snimka zaslona poruke o| X| X|  |  |
| tvrtke| X| X|  |  |
| odjelu| X| X|  |  |
| Opis| X| X| X|  |
| riješiti problem| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| givenName| X| X|  |  |
| DOM telefon| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| pošta| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| Upravitelj| X| X|  |  |
| člana|  |  | X|  |
| mobilni| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| msRTCSIP ApplicationOptions| X|  |  |  |
| msRTCSIP DeploymentLocator| X| X|  |  |
| msRTCSIP retka| X| X|  |  |
| msRTCSIP OptionFlags| X| X|  |  |
| msRTCSIP OwnerUrn| X|  |  |  |
| msRTCSIP PrimaryUserAddress| X| X|  |  |
| msRTCSIP-UserEnabled| X| X|  |  |
| objectSID| X|  | X| mehanički svojstvo. Identifikator korisnika AD koristi da bi se zadržao sinkronizaciju između Azure AD i AD.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Poštanski broj| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mehanički svojstvo. Koristi se kada za onemogućavanje već Izdana tokeni informacije. Koristi sinkronizaciju lozinke i vanjski pristup.|
| securityEnabled|  |  | X| Izvedene iz groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mehanički svojstvo. Immutable identifikator da biste zadržali odnos između ZBRAJA i Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Naslov| X| X|  |  |
| usageLocation| X|  |  | mehanički svojstvo. Država korisnika. Koristi se za dodjelu licenci.|
| userPrincipalName| X|  |  | UPN je ID za prijavu za korisnika. Najčešće isto kao [pošte] vrijednost.|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS

| Naziv atributa| Korisnik| Kontakt| Grupa| Komentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Definira ako je omogućeno račun.|
| CN| X|  | X| Uobičajeni ime ili pseudonim. Najčešće prefiks vrijednost [pošte].|
| riješiti problem| X| X| X| Niz koji predstavlja naziv često prikazana kao neslužbeni naziv (ime prezime).|
| pošta| X| X| X| adresa e-pošte s puno.|
| člana|  |  | X|  |
| objectSID| X|  | X| mehanički svojstvo. Identifikator korisnika AD koristi da bi se zadržao sinkronizaciju između Azure AD i AD.|
| proxyAddresses| X| X| X| mehanički svojstvo. Koristi Azure AD. Sadrži sve sekundarne adrese za korisnika.|
| pwdLastSet| X|  |  | mehanički svojstvo. Koristi se kada za onemogućavanje već Izdana tokeni informacije.|
| securityEnabled|  |  | X| Izvedeno iz prefiksa groupType.|
| sourceAnchor| X| X| X| mehanički svojstvo. Immutable identifikator da biste zadržali odnos između ZBRAJA i Azure AD.|
| usageLocation| X|  |  | mehanički svojstvo. Država korisnika. Koristi se za dodjelu licenci.|
| userPrincipalName| X|  |  | U ovom UPN je ID za prijavu za korisnika. Najčešće isto kao [pošte] vrijednost.|

## <a name="intune"></a>Intune

| Naziv atributa| Korisnik| Kontakt| Grupa| Komentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Definira ako je omogućeno račun.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Opis| X| X| X|  |
| riješiti problem| X| X| X|  |
| pošta| X| X| X|  |
| mailnickname| X| X| X|  |
| člana|  |  | X|  |
| objectSID| X|  | X| mehanički svojstvo. Identifikator korisnika AD koristi da bi se zadržao sinkronizaciju između Azure AD i AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mehanički svojstvo. Koristi se kada za onemogućavanje već Izdana tokeni informacije. Koristi sinkronizaciju lozinke i vanjski pristup.|
| securityEnabled|  |  | X| Izvedene iz groupType|
| sourceAnchor| X| X| X| mehanički svojstvo. Immutable identifikator da biste zadržali odnos između ZBRAJA i Azure AD.|
| usageLocation| X|  |  | mehanički svojstvo. Država korisnika. Koristi se za dodjelu licenci.|
| userPrincipalName| X|  |  | UPN je ID za prijavu za korisnika. Najčešće isto kao [pošte] vrijednost.|

## <a name="dynamics-crm"></a>Dynamics CRM

| Naziv atributa| Korisnik| Kontakt| Grupa| Komentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Definira ako je omogućeno račun.|
| c| X| X|  |  |
| CN| X|  | X|  |
| snimka zaslona poruke o| X| X|  |  |
| tvrtke| X| X|  |  |
| Pozivni| X| X|  |  |
| Opis| X| X| X|  |
| riješiti problem| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| Upravitelj| X| X|  |  |
| člana|  |  | X|  |
| mobilni| X| X|  |  |
| objectSID| X|  | X| mehanički svojstvo. Identifikator korisnika AD koristi da bi se zadržao sinkronizaciju između Azure AD i AD.|
| physicalDeliveryOfficeName| X| X|  |  |
| Poštanski broj| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | mehanički svojstvo. Koristi se kada za onemogućavanje već Izdana tokeni informacije. Koristi sinkronizaciju lozinke i vanjski pristup.|
| securityEnabled|  |  | X| Izvedene iz groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mehanički svojstvo. Immutable identifikator da biste zadržali odnos između ZBRAJA i Azure AD.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| Naslov| X| X|  |  |
| usageLocation| X|  |  | mehanički svojstvo. Država korisnika. Koristi se za dodjelu licenci.|
| userPrincipalName| X|  |  | UPN je ID za prijavu za korisnika. Najčešće isto kao [pošte] vrijednost.|

## <a name="3rd-party-applications"></a>3 proizvođača aplikacije
Ova grupa je skup atributi koji se koriste kao minimalnog atribute potrebne za generički radno opterećenje ili aplikacije. Može se koristiti za radno opterećenje nije naveden na druga sekcija ili drugih proizvođača aplikacije. Koristi se izričito za sljedeće:

- Yammer (samo korisnik potrošnje)
- [Poslovni-to-Business (B2B) unakrsno organizacijski suradnju scenarija hibridnog nudi resurse kao što je SharePoint](http://go.microsoft.com/fwlink/?LinkId=747036)

Skup atributi koje je moguće koristiti ako direktorija Azure AD se koristi u sustavu Office 365, Dynamics ili Intune je ovu grupu. Ima malom core atribute.

| Naziv atributa| Korisnik| Kontakt| Grupa| Komentar |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Definira ako je omogućeno račun.|
| CN| X|  | X|  |
| riješiti problem| X| X| X|  |
| givenName| X| X|  |  |
| pošta| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| člana|  |  | X|  |
| objectSID| X|  |  | mehanički svojstvo. Identifikator korisnika AD koristi da bi se zadržao sinkronizaciju između Azure AD i AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | mehanički svojstvo. Koristi se kada za onemogućavanje već Izdana tokeni informacije. Koristi sinkronizaciju lozinke i vanjski pristup.|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| mehanički svojstvo. Immutable identifikator da biste zadržali odnos između ZBRAJA i Azure AD.|
| usageLocation| X|  |  | mehanički svojstvo. Država korisnika. Koristi se za dodjelu licenci.|
| userPrincipalName| X|  |  | UPN je ID za prijavu za korisnika. Najčešće isto kao [pošte] vrijednost.|

## <a name="windows-10"></a>Windows 10
Windows 10 domene pridruženo computer(device) sinkronizira neke atribute Azure AD. Dodatne informacije o scenarijima potražite u članku [Povezivanje domene pridruženo uređajima Azure AD za Windows 10 iskustvo](active-directory-azureadjoin-devices-group-policy.md). Ti atributi uvijek sinkronizirajte i Windows 10 prikazuju se kao aplikaciju možete poništiti. Windows 10 domene pridruženo računalu otkrije pojavljuju userCertificate atribut popunjeno.

| Naziv atributa| Uređaj| Komentar |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| Vrijednost koji nisu za domenu pridruženo računala. |
| riješiti problem | X| |
| MS-DS-CreatorSID | X| Naziva se i registeredOwnerReference.|
| objectGUID | X| Naziva se i deviceID.|
| objectSID | X| Naziva se i onPremisesSecurityIdentifier.|
| Operacijski_sustav | X| Naziva se i deviceOSType.|
| operatingSystemVersion | X| Naziva se i deviceOSVersion.|
| userCertificate | X| |

Ti atributi za **korisnika** nisu u ostale aplikacije koje ste odabrali.  

| Naziv atributa| Korisnik| Komentar |
| --- | :-: | --- |
| domainFQDN| X| Naziva se i dnsDomainName. Ako je, primjerice, contoso.com.|
| domainNetBios| X| Naziva se i netBiosName. Na primjer, CONTOSO.|

## <a name="exchange-hybrid-writeback"></a>Upisima hibridnog sustava Exchange
Ti atributi zapisuju se ponovno iz Azure AD lokalnog imeničkog servisa Active Directory kad odaberete da biste omogućili **hibridna implementacija sustava Exchange**. Ovisno o verziji sustava Exchange, možda će se sinkronizirati manje atribute.

| Naziv atributa| Korisnik| Kontakt| Grupa| Komentar |
| --- | :-: | :-: | :-: | --- |
| msDS ExternalDirectoryObjectID| X|  |  | Izvedeno iz prefiksa cloudAnchor u Azure AD. Taj atribut je novo u sustavu Exchange 2016.|
| msExchArchiveStatus| X|  |  | Mrežna arhiva: Omogućuje klijentima za arhiviranje pošta.|
| msExchBlockedSendersHash| X|  |  | Filtriranje: Zapisuje lokalnog filtriranja i online sigurnih i blokiranih pošiljatelja podataka klijenata.|
| msExchSafeRecipientsHash| X|  |  | Filtriranje: Zapisuje lokalnog filtriranja i online sigurnih i blokiranih pošiljatelja podataka klijenata.|
| msExchSafeSendersHash| X|  |  | Filtriranje: Zapisuje lokalnog filtriranja i online sigurnih i blokiranih pošiljatelja podataka klijenata.|
| msExchUCVoiceMailSettings| X|  |  | Omogućivanje sjedinjenog (UM) – Online Govorna pošta: Microsoft Lync Server koristi Integracija da biste naznačili Lync Server lokalni korisnik ima govorne pošte u internetske servise.|
| msExchUserHoldPolicies| X|  |  | Čekanje sudskih: Omogućuje servise u oblaku da biste utvrdili koji korisnici su u odjeljku otkrivanje sudskih držite.|
| proxyAddresses| X| X| X| Samo x500 umeće se adresa sa sustava Exchange Online.|

## <a name="device-writeback"></a>Uređaj upisima
Objekata uređaja stvaraju se u servisu Active Directory. Ti objekti može biti uređaje koji se spajaju Azure AD ili domene pridruženo računala za Windows 10.

| Naziv atributa| Uređaj| Komentar |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| riješiti problem | X| |
| dn | X| |
| msDS CloudAnchor | X| |
| msDS DeviceID | X| |
| msDS DeviceObjectVersion | X| |
| msDS DeviceOSType | X| |
| msDS DeviceOSVersion | X| |
| msDS DevicePhysicalIDs | X| |
| msDS KeyCredentialLink | X| Samo s shemom AD za Windows Server 2016 |
| msDS IsCompliant | X| |
| msDS IsEnabled | X| |
| msDS IsManaged | X| |
| msDS RegisteredOwner | X| |


## <a name="notes"></a>Bilješke

- Kada koristite zamjenski ID, atribut userPrincipalName lokalnog se sinkronizira s onPremisesUserPrincipalName za Azure AD atribut. Atribut zamjenski ID-a za poštu primjer se sinkronizira s atribut userPrincipalName Azure AD.
- Na popisu iznad vrsta objekta **korisnika** odnosi i na Vrsta objekta **iNetOrgPerson**.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o konfiguraciji [Azure AD Connect sinkronizirati](active-directory-aadconnectsync-whatis.md) .

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
