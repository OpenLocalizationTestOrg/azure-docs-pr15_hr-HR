<properties
   pageTitle="Preduvjeti za Azure katalog podataka | Microsoft Azure"
   description="Azure katalog podataka preduvjeti – što vam je potrebno za početak rada s Azure kataloga podataka."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Preduvjeti za Azure kataloga podataka

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>Što je potrebno za početak rada s katalog podataka Azure?

Morat ćete voditi brigu o prije no što postavite **Azure katalog podataka**na nekoliko stvari. Ne brinite – neće stupiti dugo!

## <a name="azure-subscription"></a>Azure pretplate
Da biste postavili Azure katalog podataka, morate biti vlasnik ili suradnika kao Azure pretplate.

Azure pretplate lakše organizirali pristup oblaka resursima servisa kao što su Azure kataloga podataka. Oni i pomoći odrediti koliko je prijavio korištenje resursa, naplatiti i plaćenu. Pretplate može imati na različitim naplati i plaćanju postavljanje da biste imali različite pretplate i različite tarife odjela, project, regionalnog ureda i tako dalje. Svaki servis u oblaku pripada pretplatu, a morate imati pretplatu prije postavljanja Azure kataloga podataka. Dodatne informacije potražite u članku [Upravljanje korisničkim računima, pretplate i administratorske uloge](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Da biste postavili Azure katalog podataka, morate biti prijavljeni pomoću programa Azure Active Directory korisnički račun.

Azure Active Directory (Azure AD) predstavlja jednostavan način za svoju tvrtku da upravljaju identiteta i access, u oblak i lokalne. Korisnici mogu koristiti na jedan račun tvrtke ili obrazovne ustanove za jedinstvenu prijavu sve oblak i lokalne web-aplikacije. Azure katalog podataka koristi Azure AD za provjeru autentičnosti prijave. Dodatne informacije potražite u članku [što je Azure Active Directory](../active-directory/active-directory-whatis.md).

> [AZURE.NOTE] [Portal za Azure](http://portal.azure.com/) korisnicima omogućuje prijavite se pomoću osobnog Microsoftova Account ili rad na Azure Active Directory račun ili obrazovne ustanove. Da biste postavili Azure katalog podataka pomoću portala za Azure ili pomoću [portala za katalog podataka](http://www.azuredatacatalog.com) , morate biti prijavljeni pomoću računa za Azure Active Directory, ne osobnog računa.

## <a name="active-directory-policy-configuration"></a>Konfiguracija pravila za Active Directory

U nekim slučajevima korisnici mogu pojaviti situaciji gdje prijave na portal Azure katalog podataka, ali kada pokušaju prijavite se na alat Registracija izvora podataka koji se pojavi poruka o pogrešci koja ih sprječava prijave. Takvo ponašanje problem se može pojaviti kada korisnik je na mreži tvrtke ili samo prilikom povezivanja korisnika iz izvan mreže tvrtke.

Alat za registraciju izvora podataka koristi provjeru autentičnosti obrazaca za provjeru valjanosti korisničkog imena za prijavu protiv servisa Active Directory. Za uspješan prijavu provjere autentičnosti obrazaca mora biti omogućen u globalni pravila za provjeru autentičnosti administrator servisa Active Directory.

Globalni pravila za provjeru autentičnosti omogućuje metoda provjere autentičnosti koje će biti omogućene zasebno za intranet i ekstranet veze, kao što je prikazano ispod. Pogreške prilikom prijave može pojaviti ako Provjera autentičnosti obrazaca nisu omogućeni u mreži iz koje se povezuje korisnika.

 ![Active Directory globalni provjere autentičnosti pravila](./media/data-catalog-prerequisites/global-auth-policy.png)

Dodatne informacije potražite u članku [Konfiguriranje pravila za provjeru autentičnosti](https://technet.microsoft.com/library/dn486781.aspx).
