<properties
    pageTitle="Azure AD Connect sinkronizacije: najbolje prakse za promjenu zadanu konfiguraciju | Microsoft Azure"
    description="Nudi najbolje prakse za promjenu zadanu konfiguraciju Azure AD Connect sinkronizacije."
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
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect sinkronizacije: najbolje prakse za promjenu zadanu konfiguraciju
Je svrha u ovoj se temi opisuje podržane i nepodržane promjene Azure AD Connect sinkronizaciju.

Konfiguriranje stvorio Azure AD Connect radi "kakav jest" za većinu okruženja koji se sinkroniziraju lokalnog imeničkog servisa Active Directory sa Azure AD. No u nekim slučajevima je potrebno da biste primijenili neke promjene konfiguracije na način koji zadovoljava određene treba li vam ili preduvjeta.

## <a name="changes-to-the-service-account"></a>Promjene na račun servisa
Sinkronizacija Azure AD Connect se izvodi u odjeljku servis za račun stvorenu u čarobnjaku za instalaciju. Ovaj račun servisa sadrži ključeve za šifriranje s bazom podataka koristi sinkronizaciju. Stvara se pomoću lozinke dugački 127 znakova i lozinka je postavljeno na ne istječu.

- Nije **nepodržane** da biste promijenili ili ponovno postavite lozinku za račun servisa. Time destroys ključeva za šifriranje i servis nije uspio pristup bazi podataka te se ne može pokrenuti.

## <a name="changes-to-the-scheduler"></a>Promjene u raspored
Počevši od izdanja iz međuverzije 1.1 (veljača 2016) možete konfigurirati [rasporeda](active-directory-aadconnectsync-feature-scheduler.md) da bi različite sinkronizaciju ciklusa od zadane 30 minuta.

## <a name="changes-to-synchronization-rules"></a>Promjene pravila za sinkronizaciju
Čarobnjak za instalaciju omogućuje konfiguraciju koja bi trebao radi Najčešći scenariji. U slučaju da trebate napraviti promjene konfiguracije, morate slijediti ta pravila da biste ostavili podržani konfiguracije.

- Možete [promijeniti atribut tokova](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) ako Izravni atribut tokova zadani nisu prikladna za tvrtku ili ustanovu.
- Ako želite [ne tijek atribut](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) i uklonite sve postojeće atribut vrijednosti u Azure AD, morate stvoriti pravilo za taj scenarij.
- [Onemogućivanje sinkronizacije pravilo neželjene](#disable-an-unwanted-sync-rule) umjesto brisanja. Pravilo izbrisana je ponovno stvoriti tijekom nadogradnje.
- Da biste [promijenili pravilo – okvir](#change-an-out-of-box-rule), napravite njezinu kopiju izvornog pravila i Onemogući pravilo – okvir. Uređivač pravila za sinkronizaciju Traži i pomaže vam.
- Izvoz pravila prilagođenih sinkronizacije u uređivaču pravila za sinkronizaciju. Uređivač nudi skriptu PowerShell možete koristiti da biste jednostavno ponovno ih stvorite u scenariju Izrada oporavak.

>[AZURE.WARNING] Pravila za sinkronizaciju – okvir imaju na otisak prsta. Ako ta pravila izvršite promjene, na otisak prsta je više koji se podudaraju. Možda ćete imati problema u budućnosti kada pokušate primijeniti novo izdanje Azure AD Connect. Samo promijeniti način na koji je opisan u ovom članku.

### <a name="disable-an-unwanted-sync-rule"></a>Onemogućivanje neželjene pravilo za sinkronizaciju
Brisanje pravilo – okvir Sinkroniziraj. To je ponovno stvoriti tijekom sljedećeg nadogradnje.

U nekim slučajevima, čarobnjak za instalaciju je sastavio konfiguraciju koja ne funkcioniraju za topologiji. Ako, na primjer, ako skup stabala topologija račun resursa, ali ste prošireni shemom u skup stabala računa sa shemom sustava Exchange, pravila za Exchange se stvaraju za skup stabala računa i skupa stabala resursa. U ovom slučaju morate onemogućiti sinkronizaciju pravila za Exchange.

![Pravilo onemogućene sinkronizacije](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Na gornjoj slici, čarobnjak za instalaciju je pronašao shemom starog sustava Exchange 2003 u skup stabala računa. Ovaj kućni broj sheme dodan prije skupa stabala resursa uvedena u okruženju Fabrikam korisnika. Da bi se sinkroniziraju bez atributa iz stare implementaciji sustava Exchange, pravila za sinkronizaciju mora biti onemogućena kao što je prikazano.

### <a name="change-an-out-of-box-rule"></a>Promjena pravilo – okvir
Ako vam je potrebna da biste promijenili pravilo – okvir, trebali biste napravite njezinu kopiju pravila – okvir i onemogućiti izvornog pravila. Zatim unesite željene promjene kloniranu pravilo. Uređivač pravila za sinkronizaciju pomaže vam pomoću tih koraka. Kada otvorite pravilo – okvir predstavljanja u ovom dijaloškom okviru:  
![Upozorenje iz okvira pravilo](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Odaberite **da** biste stvorili kopiju pravila. Otvara se zatim kloniranu pravilo.  
![Kloniranu pravila](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Na kloniranu pravilo, izvršite željene promjene opseg, uključite i transformacije.

## <a name="next-steps"></a>Daljnji koraci

**Pregled tema**

- [Azure AD Connect sinkronizacije: razumijevanje i Prilagodba sinkronizacije](active-directory-aadconnectsync-whatis.md)
- [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
