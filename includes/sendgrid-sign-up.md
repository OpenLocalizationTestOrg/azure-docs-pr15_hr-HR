Azure korisnici mogu otključati 25,000 besplatne poruke e-pošte svakog mjeseca. Ove 25,000 besplatni mjesečni poruke e-pošte dobit ćete pristup dodatno izvješćivanje i analize i [sve API-ji][] (Web, SMTP, događaj, rastavljanju i više). Informacije o SendGrid dodatne usluge potražite u članku [Značajke SendGrid][] stranice.

### <a name="to-sign-up-for-a-sendgrid-account"></a>Da biste se registrirali za SendGrid račun

1. Prijavite se na [Portal za upravljanje Azure][].

2. U donjem oknu portal za upravljanje kliknite **Novo**.

    ![Naredba-traka – novi][command-bar-new]

3. Kliknite **Marketplace**.

    ![sendgrid spremišta][sendgrid-store]

4. U dijaloškom okviru **Odabir aplikacija i servisa** odaberite **SendGrid** i kliknite strelicu desno.

5. U dijaloškom okviru **Personalizacija aplikacija i servisa** odaberite željenu tarifu **SendGrid** za prijavu.

6. Unesite naziv za identifikaciju **SendGrid** uslugu u postavkama Azure ili koristite zadanu vrijednost **SendGridEmailDelivery.Simplified.SMTPWebAPI**. Nazivi mora biti između 1 i 100 znakova i sadržavati samo alfanumeričke znakove, crtice, točke i podvlake. Naziv mora biti jedinstvena u svoj popis ste pretplaćeni stavki iz trgovine Azure.

    ![pohrana, zaslon i 2][store-screen-2]

7. Odaberite vrijednosti za područje; na primjer, Zapad SAD-a.

8. Kliknite strelicu desno.

9. Na kartici **Pregled kupnju** pregledajte plana i informacije o cijenama i pregledajte pravni uvjete. Ako se slažete uvjete, kliknite kvačicu. Nakon što kliknete kvačicu, vaš račun SendGrid će početi [SendGrid postupak za dodjelu resursa].

    ![pohrana, zaslon i 3][store-screen-3]

10. Nakon potvrde kupnju bit ćete preusmjereni na nadzornoj ploči dodaci i prikazat će se poruka **SendGrid kupnje**.

    ![sendgrid kupnju-porukama][sendgrid-purchasing-message]

    Vaš račun SendGrid odmah dodjeli i prikazat će se poruka **uspješno kupili SendGrid dodatak**. Sada stvoriti račun i vjerodajnice. Spremni ste za slanje poruke e-pošte na tom mjestu. 

    Da biste izmijenili tarifa pretplate ili potražite u članku SendGrid obratite postavke, kliknite naziv SendGrid servisa da biste otvorili na nadzornoj ploči SendGrid trgovine. 

    ![sendgrid – dodavanje-na-nadzorne ploče][sendgrid-add-on-dashboard]

    Da biste poslali poruku e-pošte pomoću SendGrid, morate unijeti vjerodajnice za račun (korisničko ime i lozinka).

### <a name="to-find-your-sendgrid-credentials"></a>Da biste pronašli SendGrid vjerodajnice ###

1. Kliknite **vezu informacije**.

    ![sendgrid veze-informacije – gumb][sendgrid-connection-info-button]

2. U dijaloškom okviru *informacije veze* , kopirajte **lozinku** i korisničko ime da biste koristili u nastavku ovog praktičnog vodiča.

    ![sendgrid, veze i informacije][sendgrid-connection-info]

    Da biste postavili deliverability postavke e-pošte, kliknite gumb **Upravljanje** . To bit ćete preusmjereni na vaše SendGrid upravljačku ploču. 

    ![sendgrid upravljačke ploče][sendgrid-control-panel]

    Dodatne informacije o Uvod u SendGrid, potražite u članku [Početak rada SendGrid][].

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/sendgrid_BAR_NEW.PNG
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid_offerings_store.png
[store-screen-2]: ./media/sendgrid-sign-up/sendgrid_store_scrn2.png
[store-screen-3]: ./media/sendgrid-sign-up/sendgrid_store_scrn3.png
[sendgrid-purchasing-message]: ./media/sendgrid-sign-up/sendgrid_purchasing_message.png
[sendgrid-add-on-dashboard]: ./media/sendgrid-sign-up/sendgrid_add-on_dashboard.png
[sendgrid-connection-info]: ./media/sendgrid-sign-up/sendgrid_connection_info.png
[sendgrid-connection-info-button]: ./media/sendgrid-sign-up/sendgrid_connection_info_button.png
[sendgrid-control-panel]: ./media/sendgrid-sign-up/sendgrid_control_panel.png

<!--Links-->

[Značajke SendGrid]: http://sendgrid.com/features
[Portal za upravljanje Azure]: https://manage.windowsazure.com
[SendGrid početak rada]: http://sendgrid.com/docs
[SendGrid postupak za dodjelu resursa]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[sve API-ji]: https://sendgrid.com/docs/API_Reference/index.html

