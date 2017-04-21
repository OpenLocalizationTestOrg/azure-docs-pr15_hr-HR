#<a name="how-to-create-a-custom-virtual-machine"></a>Kako stvoriti prilagođene virtualnog računala

*Prilagođeni* virtualnog računala odnosi se na virtualnog računala stvorite pomoću metode **Iz galerije** jer tako da možete konfigurirati dodatne mogućnosti od načina **Brzo stvaranje** . Ove mogućnosti obuhvaćaju sljedeće:

- Veći izbor za sliku koja se koristi za stvaranje virtualnog računala (VM)
- Povezivanje s VM virtualne mreže
- Dodavanje u VM postojeće oblaku
- Dodavanje u VM skupa dostupnosti

> [AZURE.IMPORTANT] Ako želite virtualnog računala da biste koristili virtualne mreže tako da možete se povezati s njim izravno naziv glavnog računala ili postavite više lokacija veze, provjerite jesu li virtualne mreže prilikom stvaranja virtualnog računala. Da biste se pridružili virtualne mreže samo prilikom stvaranja virtualnog računala moguće je konfigurirati virtualnog računala. Dodatne informacije o virtualne mreže potražite u članku [Azure virtualne mreže pregled](http://go.microsoft.com/fwlink/p/?LinkID=294063).

1. Prijavite se na [portal za Azure](http://manage.windowsazure.com).

2. Na naredbenoj traci kliknite **Novo**.

3. Kliknite **izračunati**, kliknite **virtualnog računala**, a zatim **Iz galerije**.

4. Odaberite sliku koju želite koristiti, a zatim kliknite strelicu da biste nastavili.

5. Ako više verzija slike dostupne u **Datum izdavanja verziju**, odaberite verziju koju želite koristiti.

6. U **Naziv virtualnog računala**, upišite naziv koji želite koristiti za virtualnog računala.

7. **Razina** i **veličine** koristite da biste odabrali odgovarajuću veličinu za virtualnog računala. Veličina odaberete utječe na najveći konfiguraciji virtualnog računala, kao i cijene. Konfiguriranje pojedinosti potražite u članku [virtualnog računala i veličine servisa oblaka za Azure](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. U **Novo korisničko ime**, upišite naziv za administratora račun koji želite koristiti za upravljanje poslužitelja.

9. **Novu lozinku**upišite jaku lozinku za račun administratora. U okvir **Potvrda lozinke**ponovno upišite istu lozinku.

10. Kliknite strelicu da biste nastavili.

11. U **Oblaku**, učinite nešto od sljedećeg:

    - Ako je ovo virtualnog računala prvog ili samo na servis u oblaku, odaberite **Stvori novi servis u Oblaku**. Nakon toga u **Oblak usluge DNS naziv**upišite naziv koji koristi između 3 i 24 mala slova i brojeva. Ovaj naziv postaje dio URI koji se koristi za kontakt virtualnog računala pomoću servisa u oblaku.
    - Ovaj virtualni stroj dodaje se na servis u oblaku, odaberite ga na popisu.

    > [AZURE.NOTE] Dodatne informacije o stavljanje virtualnim strojevima u istom servis u oblaku, potražite [u](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/)članku povezivanje virtualnih računala u oblaku.

12. U odjeljku **Regija/afinitet grupe/virtualne mreže**odaberite regija, afinitet grupe ili virtualne mreže koju želite koristiti za virtualnog računala. Dodatne informacije o grupama afinitet potražite u članku [o afinitet grupe za virtualne mreže](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

13. **Račun za pohranu**, odaberite postojeći račun za pohranu za datoteku VHD ili pomoću računa za automatski generirani prostora za pohranu. Automatski se stvara samo jedan račun za pohranu po regijama. Sve ostale virtualnim strojevima koji ste stvorili pomoću ove postavke nalaze se na račun za pohranu. Ograničeni ste 20 račune za pohranu.

14. Ako želite da se virtualnog računala pripadati skupa dostupnost u **Postavljanje dostupnosti**, odaberite **Postavljanje dostupnosti Stvori** ili ga dodati u postojeći skup dostupnost.

    **Napomena**: virtualnim strojevima u skupu dostupnost uvode se različite kvara domene. Smještanje više virtualnim računalima sustava dostupnosti skupa pomaže provjerite je li vaša aplikacija dostupne tijekom mrežnih pogrešaka, na lokalnom disku hardverske pogreške i sve planiranog nedostupnost.

15.  U odjeljku **krajnje točke**, pregledajte novi krajnje točke koji će se stvoriti da dopušta veze virtualnog računala, kao što su putem udaljene radne površine ili klijent sigurne ljuske (SSH). Također možete dodajte krajnje točke sada ili kasnije stvoriti. Upute o stvaranju ih kasnije, potražite [u](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md)članku postavljanje točke za virtualnog računala.

16.  U odjeljku **VM Agent**odlučiti želite li instalirati VM Agent. Ovaj agent okruženje za instaliranje proširenja koje omogućuju interakciju s virtualnog računala. Detalje potražite u članku [Upravljanje proširenja](http://go.microsoft.com/FWLink/p/?LinkID=390493).

17. Kliknite strelicu da biste stvorili virtualnog računala.

    ![Stvaranje prilagođene virtualnog računala uspješno](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Daljnji koraci##
Nakon stvaranja virtualnog računala ga pokreće automatski. Kada na portalu prikazuje status kao tekući, možete se prijaviti na virtualnog računala. Upute potražite u nekom od sljedećih članaka:

- [Kako se prijaviti virtualnog računala koja se izvodi Linux](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [Kako se prijaviti u virtualnog računala sa sustavom Windows Server](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

