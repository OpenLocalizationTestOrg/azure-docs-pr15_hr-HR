## <a name="repeatability-during-copy"></a>Repeatability tijekom Kopiraj

Kada kopirate podatke od i do relacijski trgovine, morate repeatability Imajte na umu da biste izbjegli neočekivane ishoda. 

Isječak možete se ponovno pokrenite automatski na tvorničke Azure podataka po pravila Ponovi naveden. Preporučujemo da postavite pravila Ponovi radi zaštite od tranzitne pogreške. Dakle repeatability je važno razmjer da biste voditi brigu o tijekom pomicanja podataka. 

**Kao izvor:**

> [AZURE.NOTE] Sljedeća primjera su za Azure SQL, ali se primjenjuju na bilo kojem spremišta podataka koje podržava pravokutni skupova podataka. Možda ćete morati prilagoditi **Vrsta** izvora i svojstvo **upita** (na primjer: upita umjesto sqlReaderQuery) podatke u pohranu.   

Obično prilikom čitanja iz relacijske trgovina, činite koju želite pročitati samo podatke koji odgovaraju suprotnu. Možete učiniti tako da bi se pomoću WindowStart i WindowEnd varijable dostupne u tvorničke Azure podataka. Saznajte više o varijabli i funkcija u Azure ovdje tvorničke podataka u članku [Zakazivanje sastanka te izvršavanja](../articles/data-factory/data-factory-scheduling-and-execution.md) . Primjer: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

Ovaj upit čita podatke iz 'MojaTablica", koja se nalazi u rasponu trajanje isječak. Ponovno pokrenite ovaj isječka i uvijek želite li takvo ponašanje. 

U drugim slučajevima možda želite da biste pročitali cijelu tablicu (pretpostavimo da za jednu sesiju premještanje samo) i mogu definirati na sqlReaderQuery na sljedeći način:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
