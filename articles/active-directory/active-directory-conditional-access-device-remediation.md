<properties
    pageTitle="Otklanjanje problema s pristupom Azure Active Directory | Microsoft Azure"
    description="Dodatne korake koje možete poduzeti da biste riješili probleme programa access s resursima vaše tvrtke ili ustanove."
    services="active-directory"
    keywords="uređaj pristupom utemeljeno na uvjetno, Registracija uređaja omogućiti Registracija uređaja, Registracija uređaja i MDM"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Otklanjanje poteškoća u vezi Azure Active Directory programa access

Pokušate li pristupiti sustavu SharePoint Online intranet vaše tvrtke ili ustanove, a dobijete poruku o pogrešci "pristup odbijen". što ti radiš?

U ovom se članku objašnjava postupak olakšava koje olakšavaju rješavanje problema vezanih uz pristup s resursima vaše tvrtke ili ustanove.

Pomoć pri otklanjanju Azure Active Directory (Azure AD) pristupiti problemi, prijeđite na odjeljak u članku koji prekriva svoju platformu uređaj:

-   Uređaj u sustavu Windows
-   iOS uređaj (Provjera natrag uskoro za pomoć za uređaje Iphone i uređaje Ipad.)
-   Android uređaj (ih provjeravajte uskoro za pomoć za Android telefonima i tabletima.)

## <a name="access-from-a-windows-device"></a>Pristup putem uređaja Windows

Ako vaš uređaj izvodi jedan od sljedećih platforme, potražite u sljedećim odjeljcima poruku o pogrešci koja se prikazuje pri pokušaju pristupiti aplikacije ili servisa:

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012.
- Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Uređaj nije registriran

Ako vaš uređaj nije registriran Azure AD i aplikacije zaštićen s pravilima utemeljen na uređaju, možda će se prikazati stranice na kojoj se prikazuje jedna od sljedećih poruka o pogrešci:

!["Koje ne mogu li na tom mjestu" poruke za uređaje registriran] (./media/active-directory-conditional-access-device-remediation/01.png "Scenarij")

Ako vaš uređaj nije domene-pridruženo sa servisom Active Directory u tvrtki ili ustanovi, učinite sljedeće:

1.  Provjerite je li da se prijavite u sustav Windows pomoću poslovnog računa (vaš račun servisa Active Directory).
2.  Povezivanje s korporacijskom mrežom putem virtualne privatne mreže (VPN-a) ni DirectAccess.
3.  Kada ste povezani, pritisnite tipku s logotipom sustava Windows + L ključ da biste zaključali Windows sesiju.
4.  Unesite vjerodajnice račun da biste otključali Windows sesiju.
5.  Pričekajte nekoliko minuta pa pokušajte ponovno da biste pristupili aplikaciju ili servis.
6.  Ako vidite na istoj stranici, kliknite vezu **više detalja** , a zatim obratite se administratoru e-poštu s detaljima.

Ako vaš uređaj nije domene pridruženo i pokreće Windows 10, imate dvije mogućnosti:

- Pokretanje Azure AD spoj
- Dodavanje računa tvrtke ili obrazovne ustanove za Windows

Informacije o čemu se razlikuju ove mogućnosti potražite u članku [uređaja pomoću sustava Windows 10 na svom radnom mjestu](active-directory-azureadjoin-windows10-devices.md).

Da biste pokrenuli Azure AD uključivanje, učinite sljedeće korake za platformu pokreće se vaš uređaj. (Azure AD spoj nije dostupna na uređaje Windows phone).

**Windows 10 Obljetnica Update**

1.  Otvorite aplikaciju **Postavke** .
2.  Kliknite **računi** > **pristup tvrtke ili obrazovne ustanove**.
3.  Kliknite **Poveži**.
4.  Kliknite **Uključivanje ovaj uređaj za Azure AD**.
5.  Autentičnost vašoj tvrtki ili ustanovi, unesite višestruka provjera autentičnosti ako se to od vas zatraži, a zatim slijedite korake koje se prikazuju.
6.  Odjava, a zatim se prijavite pomoću poslovnog računa.
7.  Da biste pristupili aplikacije, pokušajte ponovno.


**Ažuriranja za Windows 10 studenom 2015.**

1.  Otvorite aplikaciju **Postavke** .
2.  Kliknite **sustav** > **o**.
3.  Kliknite **Uključivanje Azure AD**.
4.  Autentičnost vašoj tvrtki ili ustanovi, unesite višestruka provjera autentičnosti ako se to od vas zatraži, a zatim slijedite korake koje se prikazuju.
5.  Odjava, a zatim se prijavite pomoću poslovnog računa (vaš račun Azure AD).
6.  Da biste pristupili aplikacije, pokušajte ponovno.

Da biste dodali račun tvrtke ili obrazovne ustanove, učinite sljedeće:

**Windows 10 Obljetnica Update**

1.  Otvorite aplikaciju **Postavke** .
2.  Kliknite **računi** > **pristup tvrtke ili obrazovne ustanove**.
3.  Kliknite **Poveži**.
4.  Autentičnost vašoj tvrtki ili ustanovi, unesite višestruka provjera autentičnosti ako se to od vas zatraži, a zatim slijedite korake koje se prikazuju.
5.  Da biste pristupili aplikacije, pokušajte ponovno.


**Ažuriranja za Windows 10 studenom 2015.**

1.  Otvorite aplikaciju **Postavke** .
2.  Kliknite **računi** > **računa**.
3.  Kliknite **Dodavanje tvrtke ili obrazovne ustanove**.
4.  Autentičnost vašoj tvrtki ili ustanovi, unesite višestruka provjera autentičnosti ako se to od vas zatraži, a zatim slijedite korake koje se prikazuju.
5.  Da biste pristupili aplikacije, pokušajte ponovno.

Ako vaš uređaj nije domene pridruženo i pokreće Windows 8.1, da biste pokrenuli uključivanje na radnom mjestu i Primjena Microsoft Intune, učinite sljedeće:

1.  Otvorite **Postavke PC-JA**.
2.  Kliknite **mreža** > **radnom mjestu**.
3.  Kliknite **Pridruži se**.
4.  Autentičnost vašoj tvrtki ili ustanovi, unesite višestruka provjera autentičnosti ako se to od vas zatraži, a zatim slijedite korake koje se prikazuju.
5.  Kliknite **Uključi**.
6.  Da biste pristupili aplikacije, pokušajte ponovno.


### <a name="browser-is-not-supported"></a>Preglednik nije podržan

Možda će biti zabranjen pristup ako pokušavate da biste pristupili aplikaciju ili servis pomoću neke od sljedećih preglednika:

- Chrome, Firefox ili bilo kojeg preglednika koji nije Microsoft Edge ili Microsoft Internet Explorer u sustavu Windows 10 ili Windows Server 2016
- Firefox u sustavu Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 ili Windows Server 2008 R2

Prikazat će se stranica s pogreškom koji izgleda ovako:

![Poruka "Koje ne mogu li na tom mjestu" za Nepodržani preglednici] (./media/active-directory-conditional-access-device-remediation/02.png "Scenarij")

Samo olakšava tako da koristite preglednik koji podržava aplikacija za svoju platformu uređaja.

## <a name="next-steps"></a>Daljnji koraci

[Azure Active Directory uvjetno pristup](active-directory-conditional-access.md)
