<properties
    pageTitle="Sigurnost koncentratora obavijesti"
    description="U ovoj se temi objašnjava sigurnosti za Azure obavijesti koncentratora."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="security"></a>Sigurnost

##<a name="overview"></a>Pregled

U ovoj se temi opisuju model sigurnosti koncentratora Azure obavijesti. Budući da obavijesti koncentratora servisa Bus entitet, oni implementirati isti model sigurnosti kao Bus servisa. Dodatne informacije potražite u članku [Provjera autentičnosti Bus servisa](https://msdn.microsoft.com/library/azure/dn155925.aspx) teme.

##<a name="shared-access-signature-security-sas"></a>Sigurnost potpis zajednički pristup (SAS) 

Obavijest o koncentratora implementira shemu sigurnosti na razini entitet naziva SAS (zajednički pristup potpis). Ovu shemu omogućuje razmjenu entiteti deklarirati do 12 pravila autorizacije u njihov opis koji će se dodijeliti prava na taj entitet.

Svako pravilo sadrži naziv, vrijednost ključa (dijeljena tajna) i skup prava, kao što je opisano u odjeljku "Zahtjevima za sigurnost". Prilikom stvaranja koncentratora za obavijesti se automatski stvaraju dva pravila: Preslušavanja pravima (oblikovanu aplikaciju klijenta) i sa sva prava (oblikovanu pozadinska aplikacija).

Prilikom izvršavanja Registracija upravljanje s klijentskim aplikacijama, ako se podaci koji se šalju putem obavijesti nije osjetljive (na primjer, vremenska prognoza ažuriranja), uobičajeni način da biste pristupili obavijesti koncentrator je da bi se dobilo vrijednost ključa pravila pristup samo za Preslušavanja aplikaciju klijenta i dati vrijednost ključa pravilo puni pristup pozadinska aplikacija.

Ne preporučuje se vrijednost ključa ugraditi u klijentskim aplikacijama za iz Windows trgovine. Način da biste izbjegli ugrađivanje vrijednost ključa je aplikaciju klijent dohvatiti iz aplikacije pozadinskog prilikom pokretanja.

Da biste razumjeli dopušta li tipku s pristupom Preslušavanja aplikacije klijenta za registraciju na bilo koju oznaku važno je. Ako aplikacije potrebno ograničiti registracije oznake specifične za određenu klijentima (na primjer, kada oznake predstavljaju korisničkog ID-a), vaše aplikacije pozadinskog morate izvršiti registracije. Dodatne informacije potražite u članku Upravljanje registracije. Imajte na umu da na taj način aplikaciju klijent neće imati izravan pristup koncentratora obavijesti.

##<a name="security-claims"></a>Sigurnost zahtjevima

Slično ostali entiteti, obavijesti koncentrator operacije dopuštene tri zahtjevima za sigurnost: preslušali, slanje i upravljanje.

| Zahtjeva | Opis | Operacije dopuštene |
|-------|-------------|--------------------|
| Slušanje | Stvaranje i ažuriranje, čitati i brisanje jednog registracije | Stvaranje i ažuriranje Registracija<br><br>Registracija za čitanje<br><br>Čitanje svih registracije za bolji<br><br>Brisanje Registracija |
| Pošalji | Slanje poruka u središtu obavijesti | Slanje poruke |
| Upravljanje | CRUDs na obavijesti koncentratora (uključujući ažuriranje PNS vjerodajnice i sigurnosnih ključeva) i čitanje registracije na temelju oznaka | Stvaranje/ažuriranje/pročitano/brisanje obavijesti koncentratora<br><br>Čitanje registracije prema oznaci |


Obavijest o koncentratora prihvatiti zahtjevima odobren tokeni kontrola pristupa za Microsoft Azure i potpis tokeni generirao zajedničke tipke konfiguriran izravno u središtu obavijesti.