<properties
    pageTitle="Azure AD Connect sinkronizaciju servisa značajke i konfiguraciji | Microsoft Azure"
    description="U članku se opisuje značajke strani servisa Azure AD Connect servisa za sinkronizaciju."
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
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect značajke servisa sinkronizacije

Značajka sinkronizacije Azure AD Connect sastoji se od dviju komponenata:

- Komponenta za lokalni pod nazivom **Azure AD Connect sinkronizaciju**nazivaju se i **modula za sinkroniziranje**.
- Servis residing u poznat i kao **Azure AD Connect sinkronizaciju servisa** Azure AD

U ovoj se temi objašnjava kako funkcionira sljedeće značajke **servisa Azure AD Connect sinkronizaciju** i kako ih pomoću komponente Windows PowerShell možete konfigurirati.

Ove postavke konfigurirani tako [Azure Active Directory Module za Windows PowerShell](http://aka.ms/aadposh). Preuzmite i instalirajte ga zasebno Azure AD Connect. Cmdleti za zabilježeni u ovoj se temi uvedene u [2016 ožujak pustite (Sastavi 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Ako nemate cmdleta navedenih u nastavku ili neće proizvesti isti rezultat, zatim provjerite pokrenete najnoviju verziju.

Da biste vidjeli konfiguraciju u direktoriju Azure AD, pokrenite `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures rezultat](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Mnoge od tih postavki samo može promijeniti Azure AD Connect.

Sljedeće postavke mogu konfigurirati `Set-MsolDirSyncFeature`:

DirSyncFeature | Komentar
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Omogućuje atribut u karanteni kada je duplikat drugog objekta umjesto neuspješnih cijeli objekt tijekom izvoza.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Omogućuje objekata da biste se uključili u userPrincipalName osim primarnu SMTP adresu.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Omogućuje modul sinkronizaciju da biste ažurirali atribut userPrincipalName upravlja/licencirani korisnici (nije omogućen vanjski pristup).

Nakon što ste omogućili značajku, ona ne može se onemogućiti ponovno.

>[AZURE.NOTE] Iz kolovoz 24, 2016 značajku *duplikat atributa otpornost* po zadanom je omogućena za nove Azure AD direktorija. Ova značajka će poslednjeg i omogućena na Imenici stvoreni prije tog datuma. Dobit ćete obavijest putem e-pošte kada direktorija uskoro će se ova funkcija nije omogućena.

Sljedeće postavke konfigurirani tako Azure AD Connect i ne mogu mijenjati `Set-MsolDirSyncFeature`:

DirSyncFeature | Komentar
--- | ---
DeviceWriteback | [Azure AD Connect: Omogućivanje uređaj upisima](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure AD Connect sinkronizacije: direktorij proširenja](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Implementacijom sinkronizaciju lozinke za sinkronizaciju Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Pregled: Grupe upisima](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Trenutno nije podržano.

## <a name="duplicate-attribute-resiliency"></a>Dupliciranje otpornost atributa
Umjesto neuspjeh Dodjela resursa za objekte s dupliciranih UPNs / proxyAddresses, atribut dupliciranu "karanteni je" i privremene vrijednost je dodijeljena. Prilikom rješavanja sukoba privremene UPN se automatski mijenja vrijednost proper. Taj problem može biti omogućen za UPN-a i proxyAddress zasebno. Dodatne pojedinosti potražite u članku [sinkronizacije identiteta i otpornost duplicirane atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName meki podudaranje
Kada je omogućen tu značajku, mekih podudaranje omogućena za UPN osim [primarnu SMTP adresu](https://support.microsoft.com/kb/2641663)koji uvijek je omogućeno. Mekih podudaranje koristi se za odgovara postojećim korisnicima oblak u Azure AD lokalne korisnike.

Ako je potrebno podudaranje lokalnog računa AD pomoću postojećih računa stvorene u oblak i ne koristite Exchange Online, a zatim Ova značajka je korisna. U ovom slučaju obično ne morate razlog da biste postavili atribut SMTP u oblak.

Značajka je na zadano novostvoreni Azure AD direktorija. Možete vidjeti ako ta značajka je omogućena za ponovnim pokretanjem:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Ako ta značajka nije omogućena za Azure AD direktorija, možete ga omogućiti tako da pokrenete:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>Sinkroniziranje userPrincipalName ažuriranja
Prethodno, ažuriranja atribut UserPrincipalName putem servisa za sinkronizaciju s lokalnim blokiran, osim ako su zadovoljena oba sljedeća uvjeta:

- Korisnik se upravlja (nije omogućen vanjski pristup).
- Korisnik nije dodijeljena licenca.

Dodatne informacije potražite u članku [Korisnička imena u sustavu Office 365, Azure, ili Intune ne podudaranje lokalnog UPN-a ili ID za prijavu zamjenski](https://support.microsoft.com/kb/2523192).

Omogućivanje Ova značajka omogućuje modul sinkronizaciju da biste ažurirali na userPrincipalName kada je promijenjena lokalnog i koristite sinkronizaciju lozinke. Ako koristite vanjski pristup, ta značajka nije podržana.

Značajka je na zadano novostvoreni Azure AD direktorija. Možete vidjeti ako ta značajka je omogućena za ponovnim pokretanjem:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Ako ta značajka nije omogućena za Azure AD direktorija, možete ga omogućiti tako da pokrenete:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Nakon uključivanja te značajke postojećih vrijednosti userPrincipalName ostat će-je. Na sljedeću promjenu u userPrincipalName atribut lokalnog programa normalni delta sinkronizacije na korisnike ažurirat će se UPN-a.  

## <a name="see-also"></a>Vidi također

- [Azure AD Connect sinkronizacije](active-directory-aadconnectsync-whatis.md)
- [Integriranje vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
