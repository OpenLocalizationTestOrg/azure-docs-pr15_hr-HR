<properties
    pageTitle="Resursi, uloge i kontrola pristupa u aplikaciji uvida"
    description="Vlasnici, suradnici i čitatelji uvida u vašoj tvrtki ili ustanovi."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/07/2016"
    ms.author="awills"/>

# <a name="resources-roles-and-access-control-in-application-insights"></a>Resursi, uloge i kontrola pristupa u aplikaciji uvida

Možete kontrolirati tko ima čitanje i ažuriranje programa access s podacima u Visual Studio [Aplikacije uvida][start], pomoću [Kontrola pristupa na temelju uloga u Microsoft Azure](../active-directory/role-based-access-control-configure.md).

> [AZURE.IMPORTANT] Korisnicima u **grupu resursa ili pretplatu na** kojem pripada vaš resursa aplikacije – ne u resursa sam dodijeliti pristup. Dodijeliti ulogu **suradnika uvida aplikacije komponente** . Na taj način uniform kontrolu pristupa web testira i upozorenja zajedno s vašeg računala resursa. [Dodatne informacije](#access).


## <a name="resources-groups-and-subscriptions"></a>Resursi, grupe i pretplata

Prvo, neke definicije:

* **Resurs** - instanca servisa Microsoft Azure. Vaše aplikacije uvida resursa prikuplja, analizira i prikazuje telemetrijskih podataka koji se šalju iz aplikacije.  Druge vrste Azure resursa obuhvaća web-aplikacije, baze podataka, i VMs.

    Da biste vidjeli sve resurse, idite na [Portal za Azure][portal], prijavite se u, a zatim kliknite Pregledaj.

    ![Odaberite Pregledaj, zatim ili sve ili filtrirati uvida aplikacije](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Grupa resursa** ] [ group] -svaki resurs pripada jedne grupe. Grupa je praktičan način upravljanja povezani resursi, osobito za kontrolu pristupa. Na primjer, u jednu grupu resursa možete umetnuti da Web App, aplikacije uvida resurs koji će praćenje aplikacija i resursa za pohranu da biste zadržali izvezene podatke.


    ![Odaberite Pregledaj, a zatim grupe resursa, a zatim odaberite grupu](./media/app-insights-resources-roles-access-control/11-group.png)

* [**Pretplata**](https://manage.windowsazure.com) – da biste koristili uvida aplikacije ili druge resurse za Azure prijave Azure pretplatu. Svaka grupa resursa pripada jedan Azure pretplatu koju odaberete pakiranju cijena i, ako je pretplate za tvrtke ili ustanove, odaberite članove i njihove dozvole za pristup.
* [**Microsoftov račun** ] [ account] -korisničko ime i lozinku koje koristite za prijavu u Microsoft Azure pretplate, XBox Live, Outlook.com i druge Microsoftove servise.


## <a name="access"></a>Upravljanje pristupom u grupu resursa

Je važno je znati da uz resurs koji ste stvorili za aplikaciju, postoje i zasebne skrivene resursi za upozorenja i testira web. Su priložene istoj [grupi resursa](#resource-group) kao aplikacije. Možda i niste stavili drugih servisa za Azure u njemu, kao što je web-mjesta ili prostora za pohranu.

![Resursa u aplikaciji uvida](./media/app-insights-resources-roles-access-control/00-resources.png)

Da biste kontrolirali pristup sljedećim resursima stoga preporučuje se da biste:

* Kontrola pristupa na razini **grupe resursa ili pretplate** .
* Korisnicima dodijeliti ulogu **suradnika uvida komponenti za aplikaciju** . Time ih da biste uredili web testira, upozorenja i resurse za uvid aplikacije, bez osiguravanja pristupa drugih servisa u grupi.

## <a name="to-provide-access-to-another-user"></a>Za pristup nekom drugom korisniku

Morate imati prava vlasnik pretplate ili grupu resursa.

Korisnik mora imati [Microsoftov račun][account], ili pristup svoje [Tvrtke ili ustanove Microsoftova računa](..\active-directory\sign-up-organization.md). Access omogućuje korisnicima i grupama korisnički definirano u Azure Active Directory.

#### <a name="navigate-to-the-resource-group"></a>Idite u grupu resursa

Dodavanje korisnika postoji.

![U vaše aplikacije plohu resursa, otvorite Essentials, otvorite grupu resursa i na njemu odaberite postavke/korisnika. Kliknite Dodaj.](./media/app-insights-resources-roles-access-control/01-add-user.png)

Ili nije prešli gore drugu razinu i dodavanje korisnika s pretplatom.

#### <a name="select-a-role"></a>Odaberite ulogu

![Odaberite ulogu za novog korisnika](./media/app-insights-resources-roles-access-control/03-role.png)

Uloga | U grupi resursa
---|---
Vlasnik | Možete promijeniti ništa, uključujući korisničkog pristupa
Suradnik | Možete uređivati ništa, uključujući svi resursi
Aplikacija komponente uvida suradnika | Možete uređivati aplikacije uvida resursa, testira web i upozorenja
Čitač | Možete prikazati, ali ne mijenja ništa

Stvaranje, brisanje i ažuriranje "Uređivanje obuhvaća:

* Resursi
* Testovi za web
* Upozorenja
* Neprekinuti izvoza

#### <a name="select-the-user"></a>Odaberite korisnika


![Upišite adresu e-pošte novog korisnika. Odaberite korisnika](./media/app-insights-resources-roles-access-control/04-user.png)

Ako korisniku želite ne nalazi u direktoriju, možete pozvati Svatko s Microsoftovim računom.
(Ako se koriste servisa kao što je Outlook.com, OneDrive, Windows Phone ili XBox Live imaju Microsoftov račun.)



## <a name="users-and-roles"></a>Korisnika i uloga

* [Kontrola pristupa servisu Azure na temelju uloga](../active-directory/role-based-access-control-configure.md)



<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md
