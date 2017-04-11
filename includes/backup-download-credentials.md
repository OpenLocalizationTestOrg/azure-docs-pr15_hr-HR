## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Pomoću sigurnog vjerodajnica za provjeru autentičnosti sa servisom Azure sigurnosnog kopiranja

Potrebno je moguće provjeriti autentičnost kod sigurnosno kopiranje zbirke ključeva prije nego što se može sigurnosno kopiranje podataka za Azure lokalnog poslužitelja (Windows klijenta ili Windows Server ili upravitelj zaštite podataka poslužitelja). Provjera autentičnosti je postići pomoću "sigurnog vjerodajnice". Koncept sigurnog vjerodajnice je slična pojam "postavke objavljivanja" datoteku koja se koristi u Azure PowerShell.

### <a name="what-is-the-vault-credential-file"></a>Što je datoteka vjerodajnica sigurnog?

Datoteka vjerodajnice sigurnog je certifikat koji je generirao portal za svaki sigurnosno kopiranje zbirke ključeva. Na portalu zatim prenosi javni ključ da biste u Access kontrola servisa (ACS). Privatni ključ certifikata postane dostupna korisniku u sklopu tijeka rada koji je zadan kao ulaz u tijeku rada za registraciju računala. To potvrđuje računala da biste poslali sigurnosne kopije podataka identificirani sigurnog u servisu Azure sigurnosne kopije.

Samo tijekom tijeka rada za registraciju koristi se vjerodajnica zbirke ključeva. To je odgovornosti korisnika da biste bili sigurni vjerodajnice datoteke sigurnog ne ugrožena. Ako se nalaze u ruke bilo koji rogue korisnik, vjerodajnice datoteke sigurnog mogu se registrirati druga računala protiv iste zbirke ključeva. Međutim, kao sigurnosne kopije podataka šifriran pomoću pristupni izraz koji pripada kupcu, postojeće sigurnosne kopije podataka ne može biti ugrožena. Da biste smanjili ovaj problem, sigurnog vjerodajnice su postavljene isteći u 48hrs. Možete preuzeti vjerodajnice zbirke ključeva za sigurnosno kopiranje zbirke ključeva više puta –, ali samo najnovije sigurnog vjerodajnica datoteka je primjenjivo tijekom tijeka rada za registraciju.

### <a name="download-the-vault-credential-file"></a>Preuzimanje datoteke vjerodajnica zbirke ključeva

Putem sigurnog kanala na portalu Azure preuzimanja datoteke sigurnog vjerodajnica. Servisa Azure sigurnosne kopije je ostalog privatnog ključa certifikat i privatni ključ je ista i na portalu ili servisima sustava. Poduzmite sljedeće korake da biste preuzeli datoteke vjerodajnica sigurnog na lokalno računalo.

1.  Prijava na [Portal za upravljanje](https://manage.windowsazure.com/)
2.  Kliknite na **Servisima za oporavak** u lijevom navigacijskom oknu, a zatim odaberite sigurnosno kopiranje zbirke ključeva koji ste stvorili. Kliknite ikonu oblaka da biste dobili prikaz brzi Start sigurnosno kopiranje zbirke ključeva.

    ![Brzi pregled](./media/backup-download-credentials/quickview.png)

3.  Na stranici za brzo pokretanje kliknite **Preuzimanje sigurnog vjerodajnice**. Na portalu generira datoteke sigurnog vjerodajnica koje postane dostupna za preuzimanje.

    ![Preuzimanje](./media/backup-download-credentials/downloadvc.png)

4.  Na portalu generirat će sigurnog vjerodajnica pomoću kombinacije naziv sigurnog i trenutni datum. Kliknite **Spremi** da biste preuzeli sigurnog vjerodajnica za lokalni račun preuzimanja mape ili odaberite Spremi kao na izborniku Spremi da biste odredili mjesto za vjerodajnice zbirke ključeva.

### <a name="note"></a>Napomena
- Provjerite jesu li vjerodajnice sigurnog spremljene na mjestu na kojem se može pristupiti s računala. Ako je pohranjen u datoteci zajedničko korištenje/SMB, provjerite dozvole za pristup.
- Datoteka vjerodajnice sigurnog koristi se samo tijekom tijeka rada za registraciju.
- Datoteka vjerodajnice sigurnog istječe nakon 48hrs i može se preuzeti s portala sustava.
- Pogledajte sigurnosne kopije Azure [Najčešća pitanja vezana uz](../articles/backup/backup-azure-backup-faq.md) pitanja na tijeka rada.
