U sljedećoj su tablici opisani svi kvote za glavne, ograničenja, zadanih vrijednosti i Reguliranje u rasporedu Azure.

|Resurs|Ograničenje opis|
|---|---|
|**Veličina posla**|Posao Maksimalna veličina je 16K. Ako je STAVI ili na ZAKRPU rezultira zadatak koji je veće od ta ograničenja, vraća se 400 Neispravan zahtjev Šifra stanja.|
|**Zahtjev za URL veličina**|Maksimalna veličina zahtjev za URL-a je 2048 znakova.|
|**Veličinu zbrajanja zaglavlja**|Maksimalna zbrajanja zaglavlja veličina je 4096 znakove.|
|**Count zaglavlja**|Zaglavlje maksimalni broj je 50 zaglavlja.|
|**Tijelo veličina**|Tijelo Maksimalna veličina je 8192 znakova.|
|**Raspon ponavljanja**|Maksimalna ponavljanja raspon je 18 mjeseci.|
|**Vrijeme početka i vrijeme**|Maksimalna "vrijeme početka i vrijeme" je 18 mjeseci.|
|**Povijest zadatka**|Maksimalna odgovor tijelo pohranjene u povijesti zadatka je 2048 bajtova.|
|**FREQUENCY**|Zadani maksimalni učestalost kvote je 1 sat u zbirci besplatne posla i 1 min u zbirci standardni zadatak. Maksimalni učestalost je konfigurirati u zbirci posao manju od maksimalno. Ograničeni su sve zadatke u zbirci posao vrijednost postavljenu na zbirke posao. Ako pokušate stvoriti posao s viša frekvencija od maksimalno učestalosti zbirci zadatak zahtjeva neće uspjeti s 409 Šifra stanja sukoba.|
|**Zadaci**|Kvota maksimalni poslove zadani je 5 zadacima u zbirci besplatne posla i 50 zadataka u standardni posao zbirke. Maksimalni broj zadataka je konfigurirati na posao zbirke. Ograničeni su sve zadatke u zbirci posao vrijednost postavljenu na zbirke posao. Ako pokušate stvoriti više zadataka od kvote maksimalni zadacima, zatim zahtjev ne uspijeva uz 409 kod stanja sukoba.|
|**Zadržavanje povijest posla**|Dosadašnje iskustvo zadržava se do dva mjeseca i do zadnje 1000 izvršavanja.|
|**Zadržavanje dovršeni i pojavila posla**|Dovršeni i pojavila se zadržavaju 60 dana.|
|**Prekoračenje vremena**|Postoji statične (ne može konfigurirati) zahtjev vremensko ograničenje od 60 sekundi za HTTP akcije. Za dulje izvodi postupke, slijedite HTTP asinkronog protokoli; Ako, na primjer, vratili na 202 odmah, ali nastavili raditi u pozadini.|
