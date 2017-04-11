<properties
   pageTitle="Kako upravljati podacima resursi | Microsoft Azure"
   description="S uputama članak isticanje kako odrediti vidljivost i vlasništvo nad podacima resursi registrira u katalog podataka Azure."
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
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-manage-data-assets"></a>Kako upravljati podacima resursi

## <a name="introduction"></a>Uvod

**Katalog podataka Azure** pruža mogućnosti za otkrivanje izvora podataka, koji korisnicima omogućuje jednostavno otkrivanje i razumijevanje izvori podataka koje su im potrebne za izvođenje analize i donošenje odluka. Ove mogućnosti otkrivanja provjerite najvećih utjecaj kada sve korisnike možete pronaći i razumijevanje imalo raspon dostupnih izvora podataka. Uz to na umu je zadano ponašanje katalog podataka za sve izvore podataka registriranih budu vidljive – i vidljivim tako da – sve kataloga korisnika.

Katalog podataka ne odobravanje pristupa same podatke. Pristup podacima upravlja vlasnik izvora podataka. Katalog podataka omogućuje korisnicima da biste otkrili izvore podataka, a da biste pogledali metapodatke povezane s izvorima registriran u katalogu.

Možda postoje situacije, međutim, gdje izvore podataka treba vidljiv je samo za određene korisnike ili određene grupe. Ove scenarijima za katalog podataka korisnicima omogućuje vlasništvo imovine registriranih podataka u katalogu, a zatim kontrolu vidljivost imovine oni vlasnik.

> [AZURE.NOTE] Funkcija opisane u ovom članku dostupne su samo u standardni izdanje programa Azure kataloga podataka. Besplatni Edition ne nudi mogućnosti za vlasništvo i ograničavanje vidljivost podataka resursa.

## <a name="managing-ownership-of-data-assets"></a>Upravljanje vlasništvo nad podacima resursi
Po zadanom su unowned; imovine podataka registriranih u katalogu podataka bilo koji korisnik s dozvolom pristupa kataloga možete otkriti i dodati opaske te resursi. Korisnicima možete preuzeti vlasništvo nad imovine unowned podataka, a zatim možete ograničiti vidljivost amortizacije su vlasnik.

Kada čiji podataka resursa u katalog podataka samo korisnici odobrio vlasnici možete otkriti imovine i prikaz njegove metapodatke i samo vlasnici možete izbrisati imovine iz kataloga.

> [AZURE.NOTE] Vlasništvo u katalog podataka utječe samo na metapodaci spremljeni u katalogu. Ne confer sve dozvole na temeljnom izvoru podataka.

### <a name="taking-ownership"></a>Preuzimanje vlasništva
Korisnici mogu poduzeti vlasništvo nad podacima resursi tako da odaberete mogućnost "Preuzimanje vlasništva" na portalu kataloga podataka. Nema posebne dozvole potrebne za preuzimanje vlasništva nad sredstvo unowned podataka bilo koji korisnik može potrajati vlasništvo sredstava unowned podataka.

### <a name="adding-owners-and-co-owners"></a>Dodavanje vlasnicima i vlasnicima suradnika
Ako se podaci resursa već vlasnik, korisnici ne možete jednostavno vlasništvo – se mora biti dodan kao zajednički vlasnici postojeće vlasnik. Sve vlasnik možete dodati dodatne korisnike ili sigurnosne grupe kao zajednički vlasnici.

> [AZURE.NOTE] Je najbolji način imati barem dvije osobe kao vlasnika za sve resursa vlasništvu podataka.

### <a name="removing-owners"></a>Uklanjanje vlasnika
Baš kao i sve vlasnik resursa možete dodati suautorstvo vlasnici, sve vlasnik resursa možete ukloniti sve suradnika kao.

Ako vlasnika resursa uklanja sebi vlasnik, on možete više upravljati sredstava. Ako vlasnika resursa uklanja sebi kao vlasnika i postoje bez dodatnih vlasnici, imovine vratit će unowned stanje.

## <a name="visibility"></a>Vidljivost
Vlasnici resursa podataka možete odrediti vidljivost imovine podaci su vlasnik. Da biste ograničili vidljivost zadanu – gdje svi korisnici katalog podataka mogu otkrivati pregled resursa podataka – vlasnik resursa za uključivanje i isključivanje postavka vidljivosti sa "Svima" "Vlasnici & ove korisnicima" u svojstava imovine. Vlasnici možete dodati određene korisnike i sigurnosne grupe.

> [AZURE.NOTE] Kad god je to moguće, dozvole resursa vlasništvo i vidljivost trebaju biti dodijeljeni sigurnosne grupe, a ne i pojedinačne korisnike.

## <a name="catalog-administrators"></a>Administratori kataloga
Administratori kataloga podataka implicitno su zajednički vlasnici sva sredstva u katalogu. Vlasnici resursa iz kataloga administratorima ne možete ukloniti vidljivost i administratori mogu upravljati vlasništvo i vidljivosti za sva sredstva podataka u katalogu.

## <a name="summary"></a>Sažetak
Katalog podataka crowdsourcing modela metapodataka i podataka otkrivanje resursa korisnicima sve kataloga omogućuje sudjelovanje i ustanovili. Standardni kataloga za Edition podataka pruža mogućnosti za vlasništvo i upravljanje njima da biste ograničili vidljivost i korištenje imovine konkretne podatke.
