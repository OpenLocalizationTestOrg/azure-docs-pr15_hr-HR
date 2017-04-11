<properties
   pageTitle="Poveznik povijest izdanje | Microsoft Azure"
   description="Ova tema sadrži popis svih izdanja poveznika za upravljanje pravima na informacije (FIM-Forefront identiteta Upravitelj) i Microsoft identiteta Manager (MIM)"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="billmath"/>

# <a name="connector-version-release-history"></a>Povijest verzija izdanja za poveznika
Poveznici za upravljanje pravima na informacije (FIM-Forefront identiteta Upravitelj) i Microsoft identiteta Manager (MIM) često ažurira.

>[AZURE.NOTE]
U ovoj se temi je samo na FIM i MIM. Ove poveznika nisu podržane na Azure AD Connect.

U ovoj se temi popis svih verzija poveznika objavljene.

Srodne veze:

- [Preuzmite najnovije poveznika](http://go.microsoft.com/fwlink/?LinkId=717495)
- [Generički LDAP poveznik](active-directory-aadconnectsync-connector-genericldap.md) referentnu dokumentaciju
- [Generički SQL poveznik](active-directory-aadconnectsync-connector-genericsql.md) referentnu dokumentaciju
- Dokumentacija referenca [Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245)
- [Poveznik za PowerShell](active-directory-aadconnectsync-connector-powershell.md) referentnu dokumentaciju
- [Poveznik programa Lotus Domino](active-directory-aadconnectsync-connector-domino.md) referentnu dokumentaciju

## <a name="111170"></a>1.1.117.0
Objavio: 2016 ožujak

**Novi poveznika**  
Početna izdanje [Generički Connector SQL](active-directory-aadconnectsync-connector-genericsql.md).

**Nove značajke:**

- Poveznik za generički LDAP:
    - Podrška za uvoz delta s Isode.
- Poveznik za web Services:
    - Ažurirati csEntryChangeResult aktivnosti i setImportErrorCode aktivnosti da biste omogućili objekt pogreške na razini vratiti natrag modula za sinkroniziranje.
    - Ažuriranje predložaka SAP6 i SAP6User će se koristiti nove funkcije razine pogreške objekta.
- Poveznik za Lotus Domino:
    - Za izvoz, potrebno vam je jedan certifier po adresar. Sada možete koristiti istu lozinku za sve certifiers da biste olakšali upravljanje.

**FIXED problemi:**

- Poveznik za generički LDAP:
    - Za IBM Tivoli DS, neki atributi referenca nije otkriven pravilno.
    - Za otvaranje LDAP tijekom uvoza za delta razmaci na početku i kraju nizovi su odrezan.
    - Novell i NetIQ, izvoz koje premjestili neki objekt između organizacijske jedinice/spremnika i istovremeno preimenovati objekt nije uspjelo.
- Poveznik za web Services:
    - Ako web-servisa imali više end-points za istu uvez, zatim poveznik jeste li nije ispravno otkrijte te end-points.
- Poveznik za Lotus Domino:
    - Izvezite atribut ime i prezime u pošte u bazu podataka nije uspjelo.
    - Izvoz koji se dodaju i uklanjaju člana iz grupe samo izvoze dodane članova.
    - Ako je dokument bilješke nije valjan (u atribut isValid postavite na false), a zatim ne uspije poveznik.

## <a name="older-releases"></a>Stariji izdanja
Prije nego što 2016 ožujak poveznika su izdavati tijekom teme podrške.

**Generički LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, rujan 2015.
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, ožujak 2015
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, siječnju 2015.
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, rujan 2014.
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, ožujak za 2014.

**WebServices**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, rujan 2014.

**PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, rujan 2014.

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, rujan 2015.
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, ožujak 2015
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, kolovoz 2014.
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, veljača 2014.  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, listopad 2013
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, kolovoz 2013

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o konfiguraciji [Azure AD Connect sinkronizirati](active-directory-aadconnectsync-whatis.md) .

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
