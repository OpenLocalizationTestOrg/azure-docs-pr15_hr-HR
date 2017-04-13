# <a name="pull-request-comment-automation"></a>Povlačenje Automatizacija zahtjev komentara

Koristimo Automatizacija komentar u komentarima istaknuti zahtjev za suradnicima i autora da biste dodijelili natpisa koji utječu na zahtjev za istaknuti pregled postupka.

| Komentar | Funkcija | Dostupnost|
| -------- |-------------|-------------|
|#potpis | Kada autor članka unese komentar **#sign isključivanje** u strujanje komentar, natpis **jeste li spremni za spajanje** je dodijeljen. | Javne i privatne|
|#potpis | Ako suradnika koji nije navedenih autor pokušava odjaviti na zahtjev za javne istaknuti pomoću komentara **#sign isključivanje** poruke zapisuje u istaknuti zahtjev koji označava oznake koje se mogu dodijeliti samo autor. | Javno |
|#isključivanje čekanje | Ako upišete **#hold isključivanje** u komentaru istaknuti zahtjev, uklanja oznaku **jeste li spremni za spajanje** – u slučaju da ste promijenili mišljenje ili pogriješite. U privatnu repo to dodjeljuje oznaku **učinite, a ne i pisma** . | Javne i privatne |
| #Imajte na Zatvori | Autori u strujanje komentar zahtjeva za povlačite da biste ga zatvorili Ako odlučite da ne spojene promjene možete upisati komentar **please #-završna** . | Javno |

##<a name="troubleshooting-sign-offs-in-the-public-repo"></a>Znak Poma u javnim repo za otklanjanje poteškoća

Znak javno repo isključivanje Automatizacija je omogućuje autora da biste se odjaviti. Možda će biti potrebno neke obrada ručno iznimke:

- **Članak autori**: da biste koristili komentar Automatizacija javno spremište, stvarni GitHub račun mora u potpunosti odgovarati račun GitHub naveden u članku metapodataka. Velika i mala slova vašeg računa zapise. Ako su blokirane iz potpisivanja zbog tog problema, slanje pošte azdocprs pseudonim.

- **Drugi zaposlenici**: Ako ste zaposlenik tko je potpisivanja ime autora i blokiraju na Automatizacija, obratite se azdocprs s vezom zahtjev povlačite. Određivanje tko ste--PMs na istom timu za proizvod, suradnika na tim za pisanje i pisanje tima upravitelji smatraju pouzdanih izvora.



## <a name="related"></a>Odnosi

- [Povlačenje bontonu zahtjev i najbolje prakse za Microsoft suradnika](contributor-guide-pull-request-etiquette.md)

- [Kvaliteta kriterija za istaknuti zahtjev za pregled](contributor-guide-pr-criteria.md)
