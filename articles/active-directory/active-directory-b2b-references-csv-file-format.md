<properties
   pageTitle="CSV oblik datoteke za pretpregled suradnju Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B podržava više tvrtke odnosa omogućivanjem poslovni partneri za selektivno pristup tvrtke aplikacija"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Azure AD B2B suradnju pretpregled: CSV oblik datoteke

Pretpregled izdanja sustava Azure AD B2B suradnje potrebna je u CSV datoteku navođenja informacija o korisniku partnera za prenošenje putem portala za Azure AD. CSV datoteka smiju sadržavati potrebne ispod natpisa i neobavezna polja po potrebi. Izmjena oglednoj CSV datoteci (ispod) bez promjene pravopis natpise u prvom retku.

>[AZURE.NOTE] Potrebne za CSV datoteku da biste uspješno raščlaniti je prvi redak natpisa (kao što su e-pošte, riješiti problem i tako dalje). Pravopis mora podudarati polja navedeno u nastavku. Natpise označavanje sadržaja ispod sebe. Neobavezna polja koja nisu potrebne, natpisima moguće je ukloniti iz CSV datoteke. Cijeli stupac može biti prazna.

## <a name="required-fields-br"></a>Obavezna polja: <br/>
**E-pošte:** Adresa e-pošte pozvani korisnik. <br/>
**Riješiti problem:** Zaslonsko ime za pozvani korisnik (obično imenom i prezimenom).<br/>


## <a name="optional-fields-br"></a>Neobavezna polja: <br/>

**InvitationText:** Prilagodba teksta pozivnicu e-pošte nakon brendiranje aplikacije i prije otkupa vezu.<br/>
**InvitedToApplications:** AppIDs tvrtke aplikacijama dodijeliti korisnicima. AppIDs su veličina u ljusci PowerShell tako da nazovete podršku`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** ObjectIDs za grupe da biste dodali korisnika. ObjectIDs su veličina u ljusci PowerShell tako da nazovete podršku`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** URL za usmjeravanje pozvani korisnik nakon primitka pozivnice. To mora biti URL specifične za tvrtke (primjerice [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Ako to polje nije obavezno nije naveden, pozvani korisnik je preusmjereni na ploču aplikacije programa Access koje možete otvoriti na odabranom tvrtke aplikacija. U aplikaciju programa Access ploča je URL obrasca `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: adresa da biste kopirali i pozivnicu e-pošte. Ako se polje CcEmailAddress koristi, ovu pozivnicu nije moguće koristiti za e-pošte provjeriti korisnika ili stvaranja klijenta.<br/>
**Jezik:** Jezik za pozivnicu e-pošte i otkup doživljaj, "kratki" (na engleskom) kao zadani kada neodređeni. Ostale 10 podržani jezik šifre:<br/>
1. zapis potencijalnog klijenta koji: njemački<br/>
2. ES: španjolski<br/>
3. FR: francuski<br/>
4. To: talijanski<br/>
5. ja: japanski<br/>
6. ko: korejski<br/>
7. pt BR: portugalski (Brazil)<br/>
8. pravi: ruski<br/>
9. ZH HANS: kineski (pojednostavnjeni)<br/>
10. ZH HANT: tradicionalni kineski<br/>

## <a name="sample-csv-file"></a>Ogledna CSV datoteka
Slijedi primjer CSV možete izmijeniti.

>[AZURE.NOTE] Kopiranje i to zalijepite u blok za pisanje i spremite s datotečnim nastavkom ".csv". Moći uređivati to u programu Excel. Će strukturirane u tablicu tako da je natpis u prvom retku.

> Dodavanje više neobavezna polja u programu Excel navodeći oznaku i popunjavanje stupac ispod nje.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Povezani članci
Pregled naš članaka na Azure AD B2B suradnje

- [Što je Azure AD B2B suradnje?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kako funkcionira](active-directory-b2b-how-it-works.md)
- [Detaljni vodič](active-directory-b2b-detailed-walkthrough.md)
- [Tokena oblik vanjskog korisnika](active-directory-b2b-references-external-user-token-format.md)
- [Promjene atributa objekt vanjskog korisnika](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Trenutni pretpregled ograničenja](active-directory-b2b-current-preview-limitations.md)
- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
