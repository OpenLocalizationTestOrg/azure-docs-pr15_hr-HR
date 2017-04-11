Azure virtualnog računala podržava prilaganja broj diskova podataka. Postigli optimalne performanse, želite ograničiti broj iznimno koriste diskova priložiti virtualnog računala da biste izbjegli moguće ograničavanje. Ako svih disketa ne se iznimno se koristi u isto vrijeme, računa za pohranu podržavaju veći broj diskova.

- **Za račune standardne prostora za pohranu:** Račun za standardne prostora za pohranu ima maksimalni zahtjev za Ukupno rata 20 000 IOPS. Ukupna IOPS preko svih vaše diskova virtualnog računala u standardni prostora za pohranu računa smije to ograničenje.

    Otprilike možete izračunati broj iznimno koriste diskova račun za pohranu Jedna standardna stopa ograničenje zahtjev na temelju podržava. Primjer osnovne VM sloju, maksimalni broj iznimno koriste diskova je o 66 (20 000/300 IOPS po disk), kao i za standardne VM sloju, je otprilike 40 (20 000/500 IOPS po disk), kao što je prikazano u tablici u nastavku. 
 
- **Prostor za pohranu za račune za premium:** Račun za pohranu premium ima najveći ukupnu propusnost rata 50 Gbps. Ukupna propusnost preko svih vaše diskova VM smije to ograničenje.