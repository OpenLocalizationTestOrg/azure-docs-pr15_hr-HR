<!--author=SharS last changed: 01/15/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>Da biste instalirali Update 1.2 s portala za Azure klasični

1. Na stranici servisa StorSimple odaberite svoj uređaj. Pronađite **uređaje** > **održavanja**.

2. Pri dnu stranice, kliknite **Pregled ažuriranja**. Posao stvorit će se da biste pregledali dostupna ažuriranja. Bit ćete obaviješteni kada posao je uspješno dovršena.

3. U odjeljku **Softverska ažuriranja** na istoj stranici, vidjet ćete dostupnosti novog softverska ažuriranja. Preporučujemo da pregledate napomene u prije primjene Update 1.2 na uređaju.

    ![Instalacija ažuriranja](./media/storsimple-install-update-via-portal/InstallUpdate12_11M.png)

4. Pri dnu stranice kliknite **Instaliranje ažuriranja**.

5. Zatražit će se za potvrdu. Kliknite **u redu**.

6. Prikazat će se dijaloški okvir s **Instalirati ažuriranja** . Uređaj mora ispunjavaju provjere navedena u ovom dijaloškom okviru. Prije no što ažuriranja su izvršiti ove korake. Odaberite **mogu razumjeti obavezne iznad i da biste ažurirali uređaju spreman sam**. Kliknite ikonu provjeri.

    ![Poruke o potvrdi](./media/storsimple-install-update-via-portal/InstallUpdate12_2M.png)

7. Postavljanje automatskog Provjera stara će početi. To obuhvaća:

    - **Provjerava stanje kontroler** kontrolera uređaja provjerite jesu li dobar i online.
    
    - Da biste provjerili jesu li sve komponente za hardver na uređaju StorSimple dobar **provjerava hardver komponente stanja** .
    
    - **Provjerava podataka 0** provjerite je li na uređaju omogućen podataka 0. Ako je ovo sučelje nije omogućena, morat ćete je omogućiti i ponovno pokušajte.
    
    - **Provjerava je podataka 2 i 3 podataka** da biste potvrdili da sučelje mreže podataka 2 i 3 podataka nisu omogućeni. Ako je omogućeno ta sučelja, zatim morat ćete ih onemogućiti, a zatim pokušajte ažurirati uređaj. Ova provjera se izvodi samo ako ažurirate s uređaja radi GA softver. Uređajima s operacijskim sustavom verzije 0,1, 0,2 ili 0,3 će vam se Ova provjera.
    
    - **Pristupnik provjerite** kod bilo kojeg uređaja na kojem je instalirana verzija prije Update 1. Ova provjera se izvodi na svim uređaja sa sustavom stara update 1 softver, ali ne uspijeva na uređaje koje ste pristupnik konfiguriran za mrežno sučelje osim podataka 0.
 
    Ažuriranje 1.2 će se primijeniti samo ako su sve iznad provjere prije ažuriranja uspješno dovršeno. Bit ćete obaviješteni prije ažuriranja provjerava jesu li u tijeku.
  
    ![Unaprijed provjerite obavijesti](./media/storsimple-install-update-via-portal/InstallUpdate12_3M.png)

    Slijedi primjer u kojem se prije nadogradnje Provjera uspjela. Morat ćete kontrolera uređaja provjerite jesu li dobar i online. Provjera stanja hardverske komponente također ćete. U ovom primjeru kontroler 0 i 1 kontroler komponente potrebno obratiti pozornost. Možda ćete morati ako, obratite se Microsoft Support po sebi ne rješavanje tih problema.

     ![Unaprijed Provjerite nije uspio](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)

    > [AZURE.NOTE] Nakon što primijenite Update 1.2 na uređaju StorSimple, provjere podataka 2 i 3 podataka i pristupnika potvrdite više neće biti potrebni za buduća ažuriranja. Ostale prije provjere i dalje biti potrebni.


8. Kada prije nadogradnje provjere uspješno dovršeni, stvorit će se na zadatak ažuriranja. Bit ćete obaviješteni kada zadatak ažuriranja uspješno stvorili.
 
    ![Stvaranje zadatka za ažuriranje](./media/storsimple-install-update-via-portal/InstallUpdate12_44M.png)

    Ažuriranje će se primijeniti na uređaju.
 
9. Da biste pratili tijek zadatak ažuriranja, kliknite **Prikaz zadatka**. Na stranici **Poslovi** , vidjet ćete tijek ažuriranja. 

    ![Ažuriranje napretka posla](./media/storsimple-install-update-via-portal/InstallUpdate12_5M.png)

10. Ažuriranje će potrajati nekoliko sati. U bilo kojem trenutku možete vidjeti detalje posla.

    ![Ažuriranje detalja iz posla](./media/storsimple-install-update-via-portal/InstallUpdate12_6M.png)

11. Nakon dovršetka posla, dođite do stranice za **Održavanje** i pomaknite se do odjeljka **Softverska ažuriranja**.

12. Provjerite je li uređaj je pokrenut **StorSimple 8000 niz Update 1.2 (6.3.9600.17584)**. **Zadnje ažuriranje datuma** moraju se i mijenjati.

    ![Stranice za održavanje](./media/storsimple-install-update-via-portal/InstallUpdate12_10M.png)

13. Vidjet ćete dostupnosti održavanja način ažuriranja. Ta su ažuriranja disruptive ažuriranja koja rezultiraju nedostupnost uređaja i se mogu primijeniti samo putem sučelja komponente Windows PowerShell uređaja. Slijedite upute u [Održavanje način ažuriranja](storsimple-update-device.md#install-maintenance-mode-updates-via-windows-powershell-for-storsimple) instalirati sljedeća ažuriranja putem komponente Windows PowerShell za StorSimple.

> [AZURE.NOTE] U određenim slučajevima poruka: održavanje način ažuriranja može se prikazati gore 24 sata način ažuriranja održavanja uspješno primjenjuju se na uređaju.  


