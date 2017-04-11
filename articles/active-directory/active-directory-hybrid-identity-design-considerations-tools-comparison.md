<properties
    pageTitle="Identitet hibridnog: Integracija direktorija alata usporedbi | Microsoft Azure"
    description="To je stranica sadrži potpun tablicu koja se uspoređuje različite alate integraciju direktorija koje je moguće koristiti za integraciju direktorija."
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
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Integracija direktorija za hibridno identiteta Alati usporedbe

Tijekom godina Alati za integraciju direktorija imate mijenjati redoslijed reprodukcije i razvile.  Ovaj je dokument da biste dobili konsolidirani prikaz te alate i usporedbu značajki koje su dostupne u svakoj.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Azure AD Connect ugrađuje komponente i funkcija koje su prethodno izdavati tijekom Dirsync i AAD Sync. Ti Alati se više ne puštanja pojedinačno, a sve buduće poboljšanja će biti obuhvaćene ažuriranja za Azure AD Connect da biste uvijek znali gdje da biste dobili najnovije funkcije.
>
>Ukinuta je DirSync i Azure AD sinkronizaciju. Dodatne informacije pronaći ćete u [nastavku](active-directory-aadconnect-dirsync-deprecated.md).


Pomoću sljedećeg ključa za svaku od tablica.

● = Sada dostupna  
FR = buduće izdanje  
PP = javno pretpregled  

## <a name="on-premises-to-cloud-synchronization"></a>Lokalni na Cloud sinkronizacije

| Značajka  | Povezivanje Azure Active Directory  | Servisi za sinkronizaciju Azure Active Directory (AAD Sync) | Alat za sinkronizaciju servisa Azure Active Directory (DirSync)| Upravitelj identiteta Forefront 2010 R2 (FIM) |Microsoftov Upravitelj identiteta 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Povezivanje s jednom lokalnog skupa stabala AD | ● | ● | ● | ● |● |
| Povezivanje s više lokalnog šuma AD |●  | ● |  | ● |● |
| Povezivanje s više lokalnog sustava Exchange tvrtkama i ustanovama | ● |  |  |  | |
| Povezivanje s jednom lokalnog LDAP imenika | FR |  |  | ● |● |
| Povezivanje s više lokalnog LDAP imenika |FR  |  |  | ● |● |
| Povezivanje s lokalnim AD i lokalnog imenika LDAP |FR  |  |  | ● |● |
| Povezivanje s prilagođene sustavi (odnosno SQL Oracle, MySQL, itd.) | FR |  |  | ● |● |
| Sinkronizacija kupca definirani atributa (direktorija proširenja) | ● |  |  |  |  |
|Povezivanje s lokalnim HR (odnosno SAP, Oracle eBusiness, PeopleSoft)| FR |  |  | ● |● |
|Podržava pravila za sinkronizaciju FIM i poveznici za dodjeljivanje lokalnog sustava.|  |  |  | ● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Oblaka za lokalne sinkronizacije

| Značajka  | Povezivanje Azure Active Directory  | Servisi za sinkronizaciju servisa Azure Active Directory | Alat za sinkronizaciju servisa Azure Active Directory (DirSync) | Upravitelj identiteta Forefront 2010 R2 (FIM) |Microsoftov Upravitelj identiteta 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Upisima uređaja | ● |  | ● |  ||
| Atribut upisima (za hibridne implementacije sustava Exchange) | ● | ● | ● | ● |● |
| Upisima korisnici i grupe objekata |  ●|  | |  ||
| Upisima lozinke (iz samostalno ponovno postavljanje lozinke (SSPR) i promjena lozinke) |  ● | ● |  |  ||



## <a name="authentication-feature-support"></a>Podrška za značajku provjere autentičnosti

| Značajka  | Povezivanje Azure Active Directory | Servisi za sinkronizaciju servisa Azure Active Directory | Alat za sinkronizaciju servisa Azure Active Directory (DirSync) | Upravitelj identiteta Forefront 2010 R2 (FIM) |Microsoftov Upravitelj identiteta 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Sinkronizaciju lozinke za jednu lokalnog skupa stabala AD | ● | ● | ● |  ||
| Sinkronizaciju lozinke za više lokalnog šuma AD |  ●| ● |  |  ||
| Jedinstvenu prijavu s vanjskim pristupom | ● | ● | ● | ● |● |
| Upisima lozinke (iz web-mjesto SSPR i u okvir za lozinku promjena) |●  | ● |  |  ||



## <a name="set-up-and-installation"></a>Instalacija i postavljanje međuverzije

| Značajka  | Povezivanje Azure Active Directory  | Servisi za sinkronizaciju servisa Azure Active Directory | Alat za sinkronizaciju servisa Azure Active Directory (DirSync) | Microsoftov Upravitelj identiteta 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Podržava instalaciju na kontroleru domene | ● | ● | ● |  |
| Podržava instalaciju pomoću SQL Express | ● | ● | ● |  |
| Jednostavno nadogradnju s DirSync |● |  |  |  |
| Lokalizacija UX administrator za jezike Windows Server | ● | ● | ● |  |
|Lokalizacija krajnji korisnik UX jezika za Windows Server| |  |  |● |
| Podrška za Windows Server 2008 i Windows Server 2008 R2 | ● sinkronizacije, ne za vanjski pristup| ● | ●  | ● |
| Podrška za Windows Server 2012 i Windows Server 2012 R2 | ● | ● | ● | ● |

## <a name="filtering-and-configuration"></a>Filtriranje i konfiguracija

Značajka  | Povezivanje Azure Active Directory | Servisi za sinkronizaciju servisa Azure Active Directory | Alat za sinkronizaciju servisa Azure Active Directory (DirSync) | Upravitelj identiteta Forefront 2010 R2 (FIM)| Microsoftov Upravitelj identiteta 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Filtriranje na domene i Organizacijska jedinica | ● | ● | ● | ●  | ●
Filtriranje vrijednosti atributa objekata | ● | ● | ● | ●| ●
Dopusti minimalnim atributa moguće sinkronizirati (MinSync) | ● | ● |  ||
Dopusti različite servisne predložaka moguće primijeniti za tijekove atributa |●  | ● |  ||
Dopusti uklanjanjem slijedi iz AD za Azure AD atributa | ● | ● |  |  |
Dopusti napredno prilagođavanje za tijekove atributa | ● | ● |  | ●  | ●

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
