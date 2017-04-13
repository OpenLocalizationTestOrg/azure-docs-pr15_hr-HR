<properties
   pageTitle="Postavljanje pod nazivom vjerodajnice | Microsoft Azure"
   description="Saznajte kako da da biste unijeli vjerodajnice te Visual Studio pomoću autentičnost zahtjevima za Azure da biste objavili aplikacije za Azure Visual Studio ili praćenje postojeće oblaku... "
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="setting-up-named-authentication-credentials"></a>Postavljanje imenovani vjerodajnice

Da biste objavili aplikacije za Azure Visual Studio ili praćenje postojeće oblaku, morate unijeti vjerodajnice za Visual Studio možete koristiti za provjeru autentičnosti zahtjevi za Azure. Nekoliko je mjesta u Visual Studio kojima se možete prijaviti možete unijeti vjerodajnice. Ako, na primjer, iz programa Explorer poslužitelja možete otvaranje izbornika prečaca za čvor **Azure** i odaberite **Poveži se s Azure**. Prilikom prijave, podatke o pretplati povezani s računom za Azure dostupna je u Visual Studio, a ne ništa više da biste trebali učiniti.

Alati za Azure podržava i stariji način prikazivanja vjerodajnice, pomoću datoteke pretplate (.publishsettings datoteka). U ovoj se temi opisuje ta metoda koje je i dalje podržava Azure SDK 2.2.

Za provjeru autentičnosti za Azure potrebni su sljedeće stavke.

- Identifikacijskog Broja za pretplatu

- Valjani certifikat X.509 v3

>[AZURE.NOTE] Duljina ključa X.509 certifikat v3 mora biti najmanje 2048 bitova. Azure će odbaciti sve certifikat koji ne zadovoljava taj uvjet ili koji nije valjan.

Visual Studio koristi identifikacijskog Broja za pretplatu, zajedno s podatke o certifikatu kao vjerodajnice. Odgovarajuće vjerodajnice su navedene u datoteci pretplate (.publishsettings datoteka) koji sadrži javni ključ za potvrdu. Datoteke pretplate može sadržavati vjerodajnice za više pretplata.

Informacije o pretplati u dijaloškom okviru **Novo/uređivanje pretplate** možete uređivati kao što je opisano u nastavku ovog članka.

Ako želite stvoriti certifikat, pogledajte upute u [stvoriti i prenijeti upravljanje certifikat za Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) , a zatim ručno prenesite certifikat za [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.NOTE] Vjerodajnice Visual Studio potrebne za upravljanje uslugama za oblak nisu istih vjerodajnica koje su potrebne za provjeru autentičnosti zahtjev za servise za Azure pohranu.

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Izmjena ili izvoza vjerodajnice za provjeru autentičnosti u Visual Studio

Možete i postaviti, izmjena ili izvoz vjerodajnice za provjeru autentičnosti u dijaloškom okviru **Nova pretplata** koja se pojavljuje ako započnete nešto od sljedećeg:

- U **Programu Explorer poslužitelja**otvaranje izbornika prečaca za čvor **Azure** , odaberite **Upravljanje pretplatama**, odaberite karticu **potvrde** i odaberite gumb **Novo** ili **Uređivanje** .

- Kad objavite Azure oblaku iz čarobnjaka za **Objavljivanje Azure aplikacije** , odaberite **Upravljanje** na popisu **Odaberite pretplatu** , a zatim odaberite karticu certifikata, a zatim gumb **Novo** ili **Uređivanje** .

Sljedeći postupak pretpostavlja otvoren dijaloški okvir **Novu pretplatu** .

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Da biste postavili vjerodajnice za provjeru autentičnosti u Visual Studio

1. U **Odaberite postojeći certifikat** za provjeru autentičnosti popisa, odaberite certifikat.

1. Odaberite gumb **Kopiraj cijeli put** . Put do certifikat (.cer datoteka) je kopirali u međuspremnik.

    >[AZURE.IMPORTANT] Da biste objavili svoje Azure aplikacije Visual Studio, morate je prenijeti ovog certifikata [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Da biste prenijeli certifikat [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885):

    1. Odaberite vezu za Azure Portal.

         Otvorit će se [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885) .

    1. Prijava na [portal za Azure klasični](http://go.microsoft.com/fwlink/?LinkID=213885), a zatim odaberite gumb za **Servise u Oblaku** .

    1. Odaberite servisa u oblaku koja vas zanima.

        Otvorit će se stranica za servis.

    1. Na kartici **certifikata** , odaberite gumb **Prenesi** .

    1. Zalijepite puni put .cer datoteka koju ste upravo stvorili, a zatim unesite lozinku koju ste naveli.
