<properties 
   pageTitle="Otklanjanje poteškoća distribuiranih StorSimple uređaja | Microsoft Azure"
   description="U članku se opisuje kako dijagnosticirajte i Otklonite pogreške koje se pojavljuju na uređaju StorSimple koji se trenutno distribuiranih i operativnih."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />

# <a name="troubleshoot-an-operational-storsimple-device"></a>Otklanjanje poteškoća s radu StorSimple uređaju sa sustavom

## <a name="overview"></a>Pregled

Ovaj članak sadrži korisne otklanjanje poteškoća upute za rješavanje problema s konfiguracijom koje možete naići kada je uređaj StorSimple distribuiranih i operativnih. Opisuje uobičajene probleme, mogući uzroci i preporučena koraka za rješavanje problema koji se mogu pojaviti kada pokrenete Microsoft Azure StorSimple. Ove se informacije odnose fizički uređaj za lokalni StorSimple i StorSimple virtualnog uređaja.

Na kraju ovog članka možete pronaći popis šifri pogrešaka koje biste mogli naići tijekom operacije Microsoft Azure StorSimple te korake možete poduzeti da biste riješili pogreške. 

## <a name="setup-wizard-process-for-operational-devices"></a>Postupak instalacije Čarobnjak za radu uređaje

Koristite čarobnjak za postavljanje ([Pozovite HcsSetupWizard][1]) da biste provjerili Konfiguracija uređaja i poduzmite akciju ispravljanja prema potrebi.

Kada pokrenete čarobnjak za postavljanje na uređaju prethodno konfiguriran i radu, tijek postupka razlikuje se. Možete promijeniti samo sljedeće stavke:

- IP adresu, masku i pristupnika
- Primarni DNS poslužitelj
- Primarni NTP poslužitelja
- Konfiguracija proxy neobavezno web

Čarobnjak za postavljanje izvođenje operacije vezane uz lozinku Registracija prikupljanje i uređaja.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Pogreške koje se pojavljuju tijekom narednih se pokreće čarobnjak za postavljanje

U sljedećoj tablici opisane pogreške naići kada pokrenete čarobnjak za postavljanje na uređaju sa sustavom radu, mogući uzroci pogrešaka i preporučene akcije da biste ih riješili. 

| ne. | Poruka o pogrešci ili uvjet | Mogući uzroci | Preporučene akcije |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | Pogreška 350032: Ovaj uređaj već deaktiviran. | Vidjet ćete ta se pogreška ako pokrenete čarobnjak za postavljanje na uređaju koji je deaktiviran. | [Kontakt Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za sljedeće korake. Neaktivne uređaju se ne možete staviti u servisu. Ponovno postavljanje tvorničke možda potreban prije uređaja možete ponovno aktivirati. |
|  2  | Pozivanje-HcsSetupWizard: ERROR_INVALID_FUNCTION (iznimka HRESULT: 0x80070001) | Ne daje poslužitelja ažuriranje DNS-a. Postavke DNS-a su globalnih postavki i primijenit će se preko svih omogućeni mrežni sučelja. | Omogući sučelje i ponovno primijenite postavke DNS-a. Budući da su te postavke globalne to može poremetiti mreža za druge omogućeno sučelja. |
|  3  | Na uređaju se čini da je online na portalu servisa StorSimple Manager, ali kada pokušate da biste dovršili postavljanje minimalne i spremanje konfiguracije, postupak neće uspjeti. | Prilikom početne instalacije web proxy nije konfiguriran, čak i ako je došlo do stvarni proxy poslužitelj na mjestu. | Pomoću [cmdleta Test HcsmConnection] [ 2] da biste pronašli pogrešku. [Obratite se podršci Microsoft](storsimple-contact-microsoft-support.md) ako ne uspijete riješiti problem. |
|  4  | Pozivanje-HcsSetupWizard: Vrijednost se nalaziti unutar očekivanog raspona. | Netočan masku daje ta se pogreška. Nekoliko je mogućih razloga: <ul><li> Maska podmreže ne postoji ili prazan.</li><li>Oblikovanje prefiks Ipv6 nije valjana.</li><li>Sučelje nije omogućen za oblak, ali pristupnik je nedostaju ili nisu ispravne.</li></ul>Imajte na umu da podataka 0 automatski oblaka omogućenim ako je konfiguriran pomoću čarobnjaka za postavljanje. | Da biste utvrdili problem, pomoću podmreže 0.0.0.0 ili 256.256.256.256, a zatim pogledajte izlaz. Unesite odgovarajuće vrijednosti za masku, pristupnika i prefiks protokola Ipv6, po potrebi. |
 
## <a name="error-codes"></a>Kodovi pogrešaka

Pogreške su navedene u numerički redoslijed.

|Broj pogreške|Pogreška teksta ili opis|Preporučena korisničku akciju|
|:---|:---|:---|
|10502|Tijekom pristupate svom računu za pohranu došlo je do pogreške.|Pričekajte nekoliko minuta, a zatim pokušajte ponovno. Ako se pogreška nastavi, imajte na Microsoft obratite se podršci za sljedeće korake.|
|40017|Sigurnosno kopiranje operacija nije uspjela tijekom glasnoću naveden u sigurnosne kopije pravila nije pronađen na uređaju.|Pokušajte ponovno stvaranje sigurnosne kopije operacija, ako se pogreška nastavi pojavljivati, obratite se Microsoft Support. za sljedeće korake.|
|40018|Sigurnosno kopiranje operacija nije uspjela tijekom niti jedan od količine naveden u sigurnosne kopije pravila nije pronađen na uređaju. |Pokušajte ponovno stvaranje sigurnosne kopije operacija, ako se pogreška nastavi pojavljivati, obratite se Microsoft Support. za sljedeće korake.|
|390061|Sustav je zauzet ili nije dostupan.|Pričekajte nekoliko minuta, a zatim pokušajte ponovno. Ako se pogreška nastavi, imajte na Microsoft obratite se podršci za sljedeće korake.|
|390143|Kod pogreške 390143 dogodila se pogreška. (Nepoznata pogreška).|Ako se pogreška nastavi pojavljivati, obratite se Microsoft Support za sljedeće korake.|

## <a name="next-steps"></a>Daljnji koraci

Ako ne uspijete riješiti problem, [obratite se Microsoftovoj podršci](storsimple-contact-microsoft-support.md) za pomoć. 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
