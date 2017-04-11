<properties
  pageTitle="Paket Azure IoT i Azure Active Directory | Microsoft Azure"
  description="U članku se opisuje kako Azure IoT paket koristi Azure Active Directory za upravljanje dozvolama."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/24/2016"
  ms.author="araguila"/>
  
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Dozvole za azureiotsuite.com web-mjesta

## <a name="what-happens-when-you-sign-in"></a>Što se događa prilikom prijave

Kada se prvi put prijavite na [azureiotsuite.com][lnk-azureiotsuite], mjesto određuje razine dozvola koje su ovisno o klijentu trenutno odabrane Azure Active Directory (AAD) i Azure pretplate.

1.  Web-mjesto najprije prima obavijest iz Azure drugih korisnika AAD pripadate da bi se popunjavanje popisa klijenata vidjeti pokraj prijavljenog korisničko ime. Trenutno web-mjesto možete samo dobiti korisnika tokena jednog klijenta odjednom. Zbog toga kada prijeđete na drugi klijent pomoću padajući popis u gornjem desnom kutu, web-mjesta ponovno prijavljuje u klijentu da biste dobili tokena za taj klijent.

2.  Nakon toga web-mjesto prima obavijest iz Azure koje ste povezali s odabranog klijenta. Vidjet ćete popis dostupnih pretplata prilikom stvaranja novog konfiguriranog rješenja.

3.  Na kraju, web-mjesto dohvaća sve resursa u pretplate i grupa resursa označen kao konfiguriranog rješenja i popunjava pločice na početnoj stranici.

U sljedećim odjeljcima opisuju uloge kojima kontrolu pristupa u unaprijed konfiguriranom rješenja.

## <a name="aad-roles"></a>Uloge AAD

Uloge AAD kontrolirati rješenja za dodjeljivanje unaprijed konfigurirane mogućnost i upravljanje korisnicima u unaprijed konfiguriranom rješenja.

Možete pronaći dodatne informacije o administratorskim ulogama u AAD u [Dodjela administratorskih uloga u Azure AD][lnk-aad-admin], ali u ovom članku fokus prvenstveno na **Globalni Administrator** i uloge **Korisnik član domene** kao što je koristi konfiguriranog rješenja.

**Globalni Administrator:** Može postojati više globalni administratori po klijentu AAD. Prilikom stvaranja klijent AAD koje su po zadanom globalni administrator taj klijent. Globalni administrator može Dodjela resursa za konfiguriranog rješenje i **ADMINISTRATORSKE** uloge za aplikaciju unutar svoje AAD klijentu vam je dodijeljen. Međutim, ako drugi korisnik u istom klijentu AAD stvara aplikaciju, zadana uloga globalnog administratora moguć je je **Implicitno ČITATI samo**. Globalni administratori mogu dodijeliti uloge za aplikacije koje koriste [Azure klasični portal][lnk-classic-portal].

**Korisnik član domene:** Može imati više domena korisnika/članovi po klijentu AAD. Korisnik domene možete Dodjela konfiguriranog rješenje kroz [azureiotsuite.com] [ lnk-azureiotsuite] web-mjesta. Zadane uloge imaju za aplikaciju su Dodjela je **ADMINISTRATOR**. Mogu stvoriti aplikaciju pomoću skripte build.cmd u [azure iot – alat za analizu daljinske-nadgledanje] [ lnk-rm-github-repo] ili [azure-iot-predvidljivu-održavanje] [ lnk-pm-github-repo] spremište, ali zadana uloga te imaju je **Implicitno samo za ČITANJE**, kao što su, nemate dozvolu za dodjeljivanje uloge. Ako drugi korisnik u klijentu AAD stvara aplikaciju, dodjeljuju **Implicitno samo za ČITANJE** uloga prema zadanim postavkama za tu aplikaciju. Nemate mogućnost dodijelite uloge za aplikacije; Stoga ne možete dodati korisnike ili uloge za korisnike programa čak i ako su dodijeljeni resursi.

**Korisnika goste goste:** Može postojati više goste korisnika/Gosti po klijentu AAD. Gosti AAD klijentu aktivirana ograničenim pravima. Kao rezultat Gosti ne dodijelite konfiguriranog rješenja u klijentu AAD.

Dodatne informacije potražite u sljedećim resursima:

- [Stvaranje ili uređivanje korisnika u Azure AD][lnk-create-edit-users]
- [Dodijelite uloge aplikacije u AAD][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure pretplate administratorskih uloga

Azure administratorske uloge u kontrolu mogućnost da biste preslikali Azure pretplate u klijentu za AD.

Možete saznati više o ulogama Azure zajednički Administrator, Administrator servisa i računa administratora u članku [upute za dodavanje ili promjenu Azure zajednički Administrator, Administrator servisa i računa administratora][lnk-admin-roles].

## <a name="application-roles"></a>Uloge aplikacije

Uloge aplikacije kontrolu pristupa uređaja u unaprijed konfiguriranom rješenje.

Postoje dvije definirani i jednu implicitno ulogu definirano u aplikaciji koja je stvorena prilikom Dodjela konfiguriranog rješenja.

-   **ADMINISTRATORA:** Sadrži potpunu kontrolu za dodavanje, upravljanje i uklonite uređaje

-   **Samo za ČITANJE:** Sadrži mogućnost prikaza uređaja

-   **Implicitno samo za ČITANJE:** To je ista kao samo za čitanje, ali moguć je za sve korisnike AAD klijentu. To je učinjeno pogodnost tijekom razvoj. Ta uloga možete ukloniti izmjenom [RolePermissions.cs] [ lnk-resource-cs] izvornu datoteku.

### <a name="changing-application-roles-for-a-user"></a>Promjena uloga aplikacije za korisnike

Koristite sljedeći postupak da biste korisnika u servisu Active Directory administrator konfiguriranog rješenje.

Morate biti globalni administrator AAD da biste promijenili uloge za korisnike:

1. Idite na [portal za Azure klasični][lnk-classic-portal].

2. Odaberite **servisa Active Directory**.

3. Kliknite naziv AAD klijentu (to je direktorija koje ste odabrali na azureiotsuite.com pri dodjeli rješenje).

4. Kliknite **aplikacije**.

5. Kliknite naziv aplikacije koja odgovara naziv konfiguriranog rješenja. Ako ne vidite aplikacije na popisu, prijeđite padajući **Prikaz** prema dolje do **aplikacije vlasništvu Moja tvrtka** i kliknite kvačicu.

7. Kliknite **korisnici**.

8. Odaberite korisnika kojeg želite prijeći uloge.

9. Kliknite **Dodijeli** , a zatim odaberite ulogu (kao što je **administrator**) koju želite dodijeliti korisniku, kliknite kvačicu.

## <a name="faq"></a>NAJČEŠĆA PITANJA

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-do-this"></a>Ja sam administrator servisa i želim da biste promijenili mapiranje direktorija između pretplate i određene AAD klijenta. Kako to učiniti?

1. Idite na [portal za Azure klasični][lnk-classic-portal], na popisu servisa s lijeve strane kliknite **Postavke** .

2. Odaberite pretplatu u koju želite promijeniti mapiranje direktorija.

3. Kliknite **Uređivanje direktorija**.

4. Odaberite **direktorija** koji želite koristiti na padajućem popisu. Kliknite strelicu prema naprijed.

5. Potvrdite mapiranje direktorija i utječe na dodatnih administratora. Imajte na umu da ako premještate iz drugog direktorija, dodatnih administratora iz izvorne direktorija uklanjaju.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Ja sam korisnika/član domene na klijentu AAD i koje ste stvorili konfiguriranog rješenja. Kako učinite li se dodjeljuju uloge Moje aplikacije?

Zatražite od globalnog administratora da vam dodijeli kao globalni administrator na klijentu AAD da biste dobili dozvola za dodjeljivanje uloge korisnici sami ili zatražite od globalnog administratora da vam Dodijeli ulogu. Ako želite da biste promijenili klijentu AAD konfiguriranog rješenje implementiran, pogledajte sljedeće pitanje.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Kako se prebaciti na klijentu AAD su dodijeljene Moje udaljene nadzora konfiguriranog rješenja i aplikacija?

Možete pokrenuti oblaka implementaciju s <https://github.com/Azure/azure-iot-remote-monitoring> i ponovno implementirate s novostvorenom klijentu AAD. Budući da ste po zadanom globalni administrator kada stvorite novi klijent AAD, imat ćete pristup za dodavanje korisnika i dodjela dozvola korisnicima.

1. Stvorite novi direktorij AAD [portal za upravljanje Azure][lnk-classic-portal].

2. Idite na <https://github.com/Azure/azure-iot-remote-monitoring>.

3. Pokrenite `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (na primjer, `build.cmd cloud debug myRMSolution`)

4. Kada se to od vas zatraži, postavite **tenantid** biti novostvorenom klijentu umjesto prethodne klijentu.


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Želim da biste promijenili administratora servisa ili zajednički Administrator kada prijavljeni pomoću organisational računa

Potražite u članku podrška [Promjena Administrator servisa i dodatnih administratora kada prijavljeni pomoću organisational računa][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Zašto se ova pogreška se prikazuju? "Vaš račun nema odgovarajuće dozvole za stvaranje rješenja. Provjerite kod administratora računa ili pokušajte s drugim računom."

Pogledajte na dijagramu u nastavku:

![][img-flowchart]

> [AZURE.NOTE] Ako i dalje da biste vidjeli pogreške nakon provjere koje su globalni administrator na klijentu AAD i dodatnih administratora u sklopu pretplate, Održavajte administratora računa uklonite korisnika i ponovno dodjeljivanje potrebne dozvole ovim redoslijedom: Dodajte korisnika kao globalni administrator, a zatim dodajte korisnika kao dodatnih administratora u sklopu Azure pretplate. Ako problemi nastavi pojavljivati, obratite se [Pomoć i podrška][lnk-help-support].

**Zašto se prikazuju ta se pogreška prilikom Azure pretplate?** *Azure pretplate potrebna je za stvaranje unaprijed konfigurirana rješenja. Možete stvoriti besplatnu probnu račun u samo nekoliko minuta.*

Ako ste sigurni imate pretplatu na Azure, provjerite valjanost klijentu mapiranja za vašu pretplatu i odgovarajuće klijentu provjerite je li odabrana na padajućem popisu. Ako ste provjeriti točni željeni klijent, slijedite dijagrama gore i mapiranje web-mjesto pretplata i u okvir za tog klijenta AAD za provjeru valjanosti.

## <a name="next-steps"></a>Daljnji koraci

Daljnje informacije o IoT paket, potražite u članku kako možete [prilagoditi konfiguriranog rješenje][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
