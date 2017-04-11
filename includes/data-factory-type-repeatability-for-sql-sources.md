## <a name="repeatability-during-copy"></a>Repeatability tijekom Kopiraj

Kada kopiranje podataka za SQL/SQL Server Azure iz drugih podataka pohranjuje nešto treba repeatability Imajte na umu da biste izbjegli neočekivane ishoda. 

Kada kopirate podatke Azure SQL/SQL Server bazu podataka, Kopiraj aktivnosti će po zadanom DODAVANJEM zadani skup podataka u tablici primatelj. Ako, na primjer, kada kopirate podatke iz izvora (podaci vrijednosti odvojenih zarezom) CSV datoteka koja sadrži dva zapisa Azure SQL/SQL Server bazu podataka, to je izgleda tablice:
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

Pretpostavimo da pronaći pogrešaka u izvorišnoj datoteci i ažurirati količina dolje Tube od 2 za 4 u izvorišnoj datoteci. Ako ponovno pokrenite isječak podataka za to razdoblje, pronaći ćete dva nova zapisa dodan Azure SQL-SQL Server bazu podataka. U dolje podrazumijeva ništa od stupaca u tablici imate u ograničenja za primarni ključ.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Da biste izbjegli ovo, morat ćete navesti semantiku UPSERT tako da neke korištenje na ispod 2 mehanizme naveden u nastavku.

> [AZURE.NOTE] Isječak može ponovno pokrenuti automatski na tvorničke Azure podataka po pravila Ponovi naveden.

### <a name="mechanism-1"></a>Mehanizam 1

Omogućuje korištenje **sqlWriterCleanupScript** svojstva za prvo akciju čišćenje prilikom pokretanja isječak. 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

Skripta za čišćenje želite izvršiti prvi tijekom kopiju za dani isječak koji želite izbrisati podatke iz tablice SQL odgovara suprotnu. Aktivnost naknadno Umetanje podataka u tablici u SQL. 

Ako je isječak sada ponovno pokrenuti, a zatim koje ćete pronaći količina ažurira kao želji.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Pretpostavimo da paušalni Washer zapis je uklonjena iz izvorne csv. Ponovnim pokretanjem isječak dat će sljedeći rezultat: 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

Ništa novi morali napraviti. Kopiraj aktivnosti pokrenuli čišćenje skriptu da biste izbrisali odgovarajuće podatke za tog isječka. A zatim pročitajte unos iz csv (koji se zatim sadrži samo 1 zapisa) i umetnuta u tablici. 

### <a name="mechanism-2"></a>Mehanizam 2
> [AZURE.IMPORTANT] sliceIdentifierColumnName nije podržana za Azure SQL Data Warehouse trenutno. 

Drugi mehanizam da biste postigli repeatability je tako da vas namjenski stupaca (**sliceIdentifierColumnName**) u ciljnu tablicu. Ovaj stupac će poslužiti tvorničke Azure podataka da biste bili sigurni izvorišne i odredišne ostanu sinkronizirane. Taj se način radi kada postoji fleksibilnosti pri promjeni ili koji definira shemu odredišna tablica sustava SQL. 

Ovaj stupac želite koristiti tvorničke Azure podataka u svrhu repeatability i u postupku Azure podataka tvorničke će ne promijenite shemu u tablicu. Način korištenja takvog:

1.  Definirajte stupac vrste binarni (32) u odredišnoj tablici SQL. Treba li bez ograničenja u tom stupcu. Pogledajmo naziv ovaj stupac kao "ColumnForADFuseOnly" u ovom primjeru.
2.  Koristiti u aktivnosti Kopiraj na sljedeći način:

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure tvorničke podataka će popunite ovaj stupac po njegov potrebno da biste bili sigurni izvorišne i odredišne ostanu sinkronizirane. Vrijednosti u ovom stupcu ne treba koristiti izvan taj kontekst korisnika. 

Slično mehanizam 1, Kopiraj aktivnosti će automatski čišćenja podataka za dani isječak iz odredišne tablice SQL, a zatim pokrenite aktivnosti Kopiraj obično da biste umetnuli podatke iz izvora odredište za suprotnu. 
