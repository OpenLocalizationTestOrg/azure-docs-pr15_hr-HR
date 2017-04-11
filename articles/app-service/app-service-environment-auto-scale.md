<properties
    pageTitle="Autoscaling i okruženja aplikacije servisa za | Microsoft Azure"
    description="Autoscaling i aplikacije servisa za okruženje"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"
/>

# <a name="autoscaling-and-app-service-environment"></a>Autoscaling i aplikacije servisa za okruženje

Azure okruženja aplikacije servisa za podržavaju *autoscaling*. Možete automatsko skaliranje pojedinačnih radnih grupe na temelju metriku ili raspored.

![Automatsko skaliranje mogućnosti za tempiranja skup.][intro]

Autoscaling optimizira vaše Upotreba resursa tako da automatski i aplikacije servisa za okruženje sustava Prilagodi proračunom ili učitavanje profila.

## <a name="configure-worker-pool-autoscale"></a>Konfigurirajte automatsko skaliranje radnih grupa aplikacija

Funkcija automatsko skaliranje možete pristupiti skup tempiranja na kartici **Postavke** .

![Kartica postavke skup tempiranja.][settings-scale]

Iz nje, sučelje mora biti prilično poznatih nakon je isti doživljaj da vidite kada skaliranje tarifu aplikacije servisa. 

![Ručna promjena veličine postavke.][scale-manual]

Možete konfigurirati i profil programa automatsko skaliranje.

![Automatsko skaliranje postavke.][scale-profile]

Automatsko skaliranje Profili su korisne možete postaviti ograničenja na vaš skaliranje. Na taj način možete imati dosljedan performanse sučelje tako da postavite skaliranje vrijednost donja granica (1) i predvidljivi spend kapica postavljanjem gornja granica (2).

![Postavke mjerila u profilu.][scale-profile2]

Nakon definiranja profil možete dodati pravila za automatsko skaliranje skaliranja prema gore ili dolje broj instanci u tempiranja unutar granica definira profil. Pravila za automatsko skaliranje temelje se na određene parametre.

![Promjena veličine pravilo.][scale-rule]

 Skup tempiranja ni sučelja metriku mogu se definirati pravila za automatsko skaliranje. Ove metriku su iste metriku možete nadzirati plohu grafikona resursa ili postaviti upozorenja za.

## <a name="autoscale-example"></a>Primjer automatsko skaliranje

Automatsko skaliranje od okruženju aplikacije servisa možete najbolje se slaganje prolaska kroz scenarij.

U ovom se članku objašnjava potrebne razmatranja prilikom postavljanja automatsko skaliranje. U članku vodit će vas kroz interakcije koje se isporučuju u Reproduciraj kada ste faktor u autoscaling okruženja aplikacije servisa koji se nalaze u okruženju aplikacije servisa.

### <a name="scenario-introduction"></a>Uvod u scenariju

Luka je sustava za tvrtku koja sadrži migrirati dio radnih opterećenja koja on upravlja okruženju aplikacije servisa.

Okruženje za aplikacije servisa za je konfiguriran za ručno skaliranje na sljedeći način:

* **Prednje završava:** 3
* **Skup tempiranja 1**: 10
* **Skup tempiranja 2**: 5
* **Skup tempiranja 3**: 5

Skup tempiranja 1 koristi se za radnih opterećenja proizvodnje, tempiranja skup 2 i tempiranja skup 3 koriste se za kvalitete (značajke pitanja i odgovora) i radnih opterećenja razvoj.

Tarife za aplikacije servisa za značajku pitanja i odgovora i razvojni konfigurirani tako da ručno mjerilo. Proizvodne aplikacije servisa za planiranje postavljen na automatsko skaliranje baviti varijacije u opterećenja i promet.

Luka je vrlo poznat s aplikacijom. On zna nastoji za učitavanje jesu li između 9:00 Prijepodne i 6:00 PM jer je aplikacija (LOB) redak tvrtke koje koriste zaposlenici dok su u sustavu office. Korištenje izostavlja nakon toga kad korisnici gotovi za taj dan. Izvan nastoji, postoji i dalje neke opterećenja Budući da korisnici mogu se daljinski pristupati aplikaciju pomoću svojim mobilnim uređajima ili kućni PC-ja. Proizvodne aplikacije servisa za planiranje već je konfiguriran za automatsko skaliranje na temelju procesora sa sljedećim pravilima:

![Određene postavke za aplikaciju LOB.][asp-scale]

|   **Automatsko skaliranje profila – dane u tjednu – aplikacije servisa za planiranje**     |   **Automatsko skaliranje profila – vikenda – aplikacije servisa za planiranje**     |
|   ----------------------------------------------------    |   ----------------------------------------------------    |
|   **Naziv:** WEEKDAY profila                               |   **Naziv:** Profil vikenda                               |
|   **Mjerilo po:** Raspored i performanse pravila            |   **Skaliranja po:** Raspored i performanse pravila            |
|   **Profil:** Dane u tjednu                                   |   **Profil:** Vikenda                                    |
|   **Vrsta:** Ponavljanje                                    |   **Vrsta:** Ponavljanje                                    |
|   **Odredišni raspon:** 5 do 20 instanci                     |   **Odredišni raspon:** 3 do 10 instanci                     |
|   **Dana:** Ponedjeljak, Utorak, srijeda, Četvrtak, petak  |   **Dana:** Subota, nedjelja                              |
|   **Vrijeme početka:** 9:00 Prijepodne                                 |   **Vrijeme početka:** 9:00 Prijepodne                                 |
|   **Vremenska zona:** UTC-08                                   |   **Vremenska zona:** UTC-08                                   |
|                                                           |                                                           |
|   **Pravila za automatsko skaliranje (skaliranje gore)**                           |   **Pravila za automatsko skaliranje (skaliranje gore)**                           |
|   **Resursa:** Proizvodne (aplikacije servisa za okruženja)      |   **Resursa:** Proizvodne (aplikacije servisa za okruženja)      |
|   **Metriku:** PROCESOR %                                       |   **Metriku:** PROCESOR %                                       |
|   **Operacija:** Veće od 60%                         |   **Operacija:** Veće od 80%                         |
|   **Trajanje:** pet minuta                                 |   **Trajanje:** 10 minuta                                |
|   **Vrijeme prikupljanja:** Prosjek                           |   **Vrijeme prikupljanja:** Prosjek                           |
|   **Akcija:** Povećavanje broja 2                         |   **Akcija:** Povećavanje broja 1                         |
|   **Zanimljivih dolje (u minutama):** 15                             |   **Zanimljivih dolje (u minutama):** 20                             |
|                                                           |                                                           |
  	|   **Pravila za automatsko skaliranje (skaliranje dolje)**                     |   **Pravila za automatsko skaliranje (skaliranje dolje)**                         |
|   **Resursa:** Proizvodne (aplikacije servisa za okruženja)      |   **Resursa:** Proizvodne (aplikacije servisa za okruženja)      |
|   **Metriku:** PROCESOR %                                       |   **Metriku:** PROCESOR %                                       |
|   **Operacija:** Manje od 30%                            |   **Operacija:** Manje od 20%                            |
|   **Trajanje:** 10 minuta                                |   **Trajanje:** 15 minuta                                |
|   **Vrijeme prikupljanja:** Prosjek                           |   **Vrijeme prikupljanja:** Prosjek                           |
|   **Akcija:** Smanjenje broja 1                         |   **Akcija:** Smanjenje broja 1                         |
|   **Zanimljivih dolje (u minutama):** 20                             |   **Zanimljivih dolje (u minutama):** 10                             |

### <a name="app-service-plan-inflation-rate"></a>Aplikacije servisa za planiranje inflation stopa

Aplikacije servisa za tarife koje su postavljene za automatsko skaliranje učiniti Najveća brzina po satu. Ovaj tečaj može se izračunava na temelju vrijednosti navedene u pravilo za automatsko skaliranje.

Razumijevanje i izračunavanje *aplikacije servisa za planiranje inflation stopa* važno je za automatsko skaliranje okruženja aplikacije servisa za jer nisu trenutačno skaliranje promjene tempiranja resurse.

Stopa inflation aplikacije servisa za planiranje se izračunava na sljedeći način:

![Aplikacije servisa za planiranje inflation stopa izračuna.][ASP-Inflation]

Na temelju automatsko skaliranje – pravilo skaliranje gore profila Weekday proizvodnje aplikacije servisa za planiranje:

![Stopa inflation aplikacije servisa za planiranje za radne dane koji se temelji na automatsko skaliranje – pravila za skaliranje prema gore.][Equation1]

Ako automatsko skaliranje – pravilo skaliranje gore profila vikenda proizvodnje plan aplikacije servisa za formulu bi raščlanjuje:

![Stopa inflation aplikacije servisa za plan za vikenda koji se temelji na automatsko skaliranje – pravila za skaliranje prema gore.][Equation2]

Ta vrijednost je moguće izračunati i za operacije skaliranje prema dolje.

Ovisno o automatsko skaliranje – pravilo skaliranje dolje profila Weekday proizvodnje aplikacije servisa za planiranje, to će izgledati na sljedeći način:

![Brzina inflation aplikacije servisa za planiranje za radne dane koji se temelji na automatsko skaliranje – pravila za skaliranje prema dolje.][Equation3]

Ako automatsko skaliranje – pravilo skaliranje dolje profila vikenda proizvodnje plan aplikacije servisa za formulu bi raščlanjuje:  

![Stopa inflation aplikacije servisa za plan za vikenda koji se temelji na automatsko skaliranje – pravila za skaliranje prema dolje.][Equation4]

Proizvodne aplikacije servisa za planiranje može rasti Maksimalna brzina od osam instance/sata tijekom tjedan i četiri instance sat tijekom vikenda. Možete pustite instance Maksimalna brzina četiri instance sat tijekom tjedan i šest instance sat tijekom vikenda.

Ako više aplikacije servisa za tarife koji se nalaze se u tempiranja skup, imate za izračun *zbroja stopa inflation* kao zbroj stopu inflation za sve aplikacije servisa za tarife koje se nalaze u toj tempiranja.

![Ukupni inflation stopa izračuna za više aplikacije servisa za tarife smješten u tempiranja skup.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Pomoću aplikacije servisa za planiranje inflation stopa definirajte pravila za automatsko skaliranje radnih grupa aplikacija

Tempiranja pools koja hostiraju aplikacije servisa za tarife koje su postavljene za automatsko skaliranje moraju se dodijeliti međuspremnik kapaciteta. Međuspremnik omogućuje automatsko skaliranje operacija Povećaj i Smanji aplikacije servisa za planiranje prema potrebi. Minimalna međuspremnik bi izračunati ukupne aplikacije servisa planiranje Inflation stopa.

Jer aplikacije servisa za okruženje skaliranje operacije potrebno neko vrijeme da biste primijenili, bilo kakve promjene treba računa za daljnje promjene zahtjev koji se može dogoditi dok je u tijeku skaliranje operacija. Kako bi odgovarao ovaj Latencija, preporučujemo da koristite izračunati ukupne aplikacije servisa planiranje Inflation stopa kao najmanji broj instanci koje su dodane za svaku operaciju automatsko skaliranje.

Pomoću tih informacija luka možete odrediti sljedeće automatsko skaliranje profil i pravila:

![Na primjer LOB automatsko skaliranje pravila profila.][Worker-Pool-Scale]

|   **Automatsko skaliranje profila – dane u tjednu**                        |   **Automatsko skaliranje profila – vikenda**                |
|   ----------------------------------------------------    |   --------------------------------------------    |
|   **Naziv:** WEEKDAY profila                               |   **Naziv:** Profil vikenda                       |
|   **Mjerilo po:** Raspored i performanse pravila            |   **Mjerilo po:** Raspored i performanse pravila    |
|   **Profil:** Dane u tjednu                                   |   **Profil:** Vikenda                            |
|   **Vrsta:** Ponavljanje                                    |   **Vrsta:** Ponavljanje                            |
|   **Odredišni raspon:** 13 i 25 instanci                    |   **Odredišni raspon:** instance 6 do 15             |
|   **Dana:** Ponedjeljak, Utorak, srijeda, Četvrtak, petak  |   **Dana:** Subota, nedjelja                      |
|   **Vrijeme početka:** 7:00 Prijepodne                                 |   **Vrijeme početka:** 9:00 Prijepodne                         |
|   **Vremenska zona:** UTC-08                                   |   **Vremenska zona:** UTC-08                           |
|                                                           |                                                   |
|   **Pravila za automatsko skaliranje (skaliranje gore)**                           |   **Pravila za automatsko skaliranje (skaliranje gore)**                   |
|   **Resursa:** Skup tempiranja 1                             |   **Resursa:** Skup tempiranja 1                     |
|   **Metriku:** WorkersAvailable                            |   **Metriku:** WorkersAvailable                    |
|   **Operacija:** Manje od 8                              |   **Operacija:** Manje od 3                      |
|   **Trajanje:** 20 minuta                                |   **Trajanje:** 30 minuta                        |
|   **Vrijeme prikupljanja:** Prosjek                           |   **Vrijeme prikupljanja:** Prosjek                   |
|   **Akcija:** Povećavanje broja 8                         |   **Akcija:** Povećavanje broja 3                 |
|   **Zanimljivih dolje (u minutama):** 180                            |   **Zanimljivih dolje (u minutama):** 180                    |
|                                                           |                                                   |
|   **Pravila za automatsko skaliranje (skaliranje dolje)**                         |   **Pravila za automatsko skaliranje (skaliranje dolje)**                 |
|   **Resursa:** Skup tempiranja 1                             |   **Resursa:** Skup tempiranja 1                     |
|   **Metriku:** WorkersAvailable                            |   **Metriku:** WorkersAvailable                    |
|   **Operacija:** Veća od 8                           |   **Operacija:** Veće od 3                   |
|   **Trajanje:** 20 minuta                                |   **Trajanje:** 15 minuta                        |
|   **Vrijeme prikupljanja:** Prosjek                           |   **Vrijeme prikupljanja:** Prosjek                   |
|   **Akcija:** Smanjenje broja 2                         |   **Akcija:** Smanjenje broja za 3                 |
|   **Zanimljivih dolje (u minutama):** 120                            |   **Zanimljivih dolje (u minutama):** 120                    |

Odredišni raspon definirano u profilu izračunava se pomoću minimalne instance definirano u profilu za aplikacije servisa za planiranje + međuspremnik.

Raspon najviše bi zbroj Maksimalna raspone za sve aplikacije servisa za tarife smješten u skup radnih.

Povećava broj skale gore pravila trebaju imati postavljene najmanje 1 X aplikacije servisa Plan Inflation stopu skaliranja.

Smanjenje broja može se prilagoditi nešto između 1/2 X ili 1 X Inflation stopu aplikacije Plan usluge za skaliranje prema dolje.

### <a name="autoscale-for-front-end-pool"></a>Automatsko skaliranje za skup sučelja

Pravila za automatsko sučelja skaliranje su jednostavniji od za tempiranja grupe. Prije svega, trebali biste  
Provjerite je li trajanje mjeru i trajanju cooldown razmotrite skaliranje operacije na tarifu aplikacije servisa za nisu trenutačno.

U ovom scenariju luka zna da stopu pogreške povećava nakon sučeljima dosegne 80% procesora i postavlja pravilo za automatsko skaliranje da biste povećali instance na sljedeći način:

![Automatsko skaliranje postavke za skup sučelja.][Front-End-Scale]

|   **Automatsko skaliranje profila – završava prvi plan**              |
|   --------------------------------------------    |
|   **Naziv:** Automatsko skaliranje – završava prvi plan                |
|   **Mjerilo po:** Raspored i performanse pravila    |
|   **Profil:** Svaki dan                           |
|   **Vrsta:** Ponavljanje                            |
|   **Odredišni raspon:** 3 do 10 instanci             |
|   **Dana:** Svaki dan                              |
|   **Vrijeme početka:** 9:00 Prijepodne                         |
|   **Vremenska zona:** UTC-08                           |
|                                                   |
|   **Pravila za automatsko skaliranje (skaliranje gore)**                   |
|   **Resursa:** Skup sučelja                    |
|   **Metriku:** PROCESOR %                               |
|   **Operacija:** Veće od 60%                 |
|   **Trajanje:** 20 minuta                        |
|   **Vrijeme prikupljanja:** Prosjek                   |
|   **Akcija:** Povećavanje broja 3                 |
|   **Zanimljivih dolje (u minutama):** 120                    |
|                                                   |
|   **Pravila za automatsko skaliranje (skaliranje dolje)**                 |
|   **Resursa:** Skup tempiranja 1                     |
|   **Metriku:** PROCESOR %                               |
|   **Operacija:** Manje od 30%                    |
|   **Trajanje:** 20 minuta                        |
|   **Vrijeme prikupljanja:** Prosjek                   |
|   **Akcija:** Smanjenje broja za 3                 |
|   **Zanimljivih dolje (u minutama):** 120                    |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
