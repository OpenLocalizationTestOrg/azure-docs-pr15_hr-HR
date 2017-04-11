U sljedećoj su tablici navedena ograničenja pridružene servis za različite razine (S1, S2, S3, F1). Informacije o trošak svaku *jedinice* u svakom sloju, potražite u članku [IoT koncentrator cijene](https://azure.microsoft.com/pricing/details/iot-hub/).

| Resurs | Standardna S1 | Standardna S2 | Standardna S3 | Besplatni F1 |
| -------- | ----------- | ----------- | ----------- | ------- |
| Poruke/dan | 400,000 | 6,000,000   | 300,000,000 | 8,000   |
| Maksimalan broj jedinica | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Ako predviđate više od 200 jedinice pomoću S1 ili S2 ili S3 koncentrator sloju, obratite se Microsoftovoj službi za podršku.

U sljedećoj su tablici navedena ograničenja koji se odnose na središte IoT resurse:

| Resurs | Ograničenje |
| -------- | ----- |
| Maksimalno plaćenu IoT koncentratora po Azure pretplati | 10 |
| Maksimalna besplatne IoT koncentratora po Azure pretplati | 1 |
| Maksimalni broj uređaja identiteta<br/>  Vraća jedan poziva | 1000 |
| IoT koncentrator poruke Maksimalna zadržavanja za uređaj u oblak poruke | sedam dana |
| Maksimalna veličina poruke uređaj u oblak | 256 KB |
| Maksimalna veličina grupe za uređaj u oblak | 256 KB |
| Maksimalni broj poruka u seriji uređaj u oblak | 500 |
| Maksimalna veličina poruke oblaka uređaja | 64 KB |
| Maksimalna TTL za oblak uređaj poruke | 2 dana |
| Maksimalna isporuke broj za oblak na uređaj <br/> poruka | 100 |
| Broj Maksimalna isporuke poruke povratnih informacija <br/> odgovor na poruku oblaka uređaja | 100 |
| Maksimalna TTL za povratne informacije poruke u <br/> odgovor na poruku oblaka uređaja | 2 dana |

> [AZURE.NOTE] Ako vam je potrebno više od 10 plaćenu IoT koncentratora Azure pretplate, obratite se Microsoftovoj službi za podršku.

Servis IoT koncentrator regulira zahtjeve kada se premaše kvote za sljedeće:

| Ograničenja | Vrijednost po koncentratora |
| -------- | ------------- |
| Operacija registra za identitet <br/> (Stvaranje, dohvatiti, popisa, ažuriranje, brisanje) <br/> pojedinačne ili masovno uvoz/izvoz | 5000/min/jedinica (za S3) <br/> 100/min/jedinica (za S1 i S2). |
| Veza na uređaju | 6000/sec/jedinica (za S3), 120/sec/jedinicu (za S2), 12/sec/jedinicu (za S1). <br/>Najmanji od 100/sec. |
| Šalje uređaj u oblak | 6000/sec/jedinica (za S3), 120/sec/jedinicu (za S2), 12/sec/jedinicu (za S1). <br/>Najmanji od 100/sec. |
| Šalje oblaka uređaja | 5000/min/jedinica (za S3), 100/min/jedinica (za S1 i S2). |
| Oblak uređaj Prima | 50000/min/jedinica (za S3), 1000/min/jedinica (za S1 i S2). |
| Postupci za prijenos datoteka | 5000 datoteke prenesite obavijesti/min/jedinice (za S3), 100 datoteka prijenos obavijesti/min/jedinica (za S1 i S2). <br/> 10000 SAS ji može biti izgleda za račun za Azure pohranu praznih ćelija.<br/> 10 SAS ji/uređaj može biti izvan praznih ćelija. |
