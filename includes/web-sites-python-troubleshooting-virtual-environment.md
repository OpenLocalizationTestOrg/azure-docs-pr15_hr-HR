Implementacijsku skriptu će preskočite stvaranja okruženja za virtualne na Azure otkrije da već postoji okruženje virtualne kompatibilne.  To možete ubrzati implementacije znatno.  Paketi koji su već instalirani će se preskočiti po točaka.

U određenim okolnostima, preporučujemo vam da biste nametnuli brisanje njegovom virtualne okruženju.  Ćete htjeti učiniti ako se odlučite uključiti okruženje virtualne kao dio sustava spremište.  Možda želite učiniti ako je morate riješiti određene paketa ili testirali promjene requirements.txt.

Da biste upravljali postojeće okruženje virtualne na Azure na nekoliko načina:

### <a name="option-1-use-ftp"></a>Mogućnost 1: Korištenje FTP

FTP klijent povezati s poslužiteljem i ćete moći izbrišite mapu env.  Imajte na umu da neki klijenti FTP (kao što je web-preglednici) može biti samo za čitanje i ne dopuštaju da biste izbrisali mape, pa ćete htjeti provjerite je li za uporabu FTP klijent za tu mogućnost.  Naziv glavnog računala FTP i korisnik prikazuju se u plohu web app za [Azure Portal](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Mogućnosti 2: Izvođenje Uključivanje i isključivanje

Evo alternativu uzima prednost fact implementacijsku skriptu će izbrisati mapu env kada ne odgovara željenoj verziji Python.  To će učinkovito izbrišite postojeće okruženje i stvorite novi.

1. Prijelaz na neku drugu verziju programa Python (putem runtime.txt ili plohu **Postavke aplikacije** na portalu za Azure)
1. brojka automatske neke promjene (Zanemari sve pogreške prilikom instalacije točaka ako ih ima)
1. Vratite se u Početna verzija Python
1. ponovno brojka automatske neke promjene

### <a name="option-3-customize-deployment-script"></a>Mogućnost 3: Prilagodba implementacijsku skriptu

Ako ste prilagodili implementacijsku skriptu, možete promijeniti kod u deploy.cmd da biste prisilno izbrišite mapu env.
