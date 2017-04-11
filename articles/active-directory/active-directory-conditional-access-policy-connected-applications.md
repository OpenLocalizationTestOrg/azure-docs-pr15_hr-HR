<properties
    pageTitle="Postavljanje pravilnika za pristup uvjetno utemeljen na uređaj za Azure Active Directory povezani aplikacije | Microsoft Azure"
    description="Postavite pristup utemeljen na uređaju uvjetnog pravila za Azure AD povezani aplikacije."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Postavljanje pravilnika za pristup uvjetno utemeljen na uređaj za Azure Active Directory povezani aplikacije


Azure Active Directory (Azure AD) uređaj pristupom utemeljeno na uvjetno pojednostavljuju zaštita resurse tvrtke ili ustanove:

- Access pokušava s nepoznatim ili neupravljani uređaja.
- Uređaji koji ne odgovaraju sigurnosnih pravilnika za tvrtke ili ustanove.

Da biste saznali uvjetno access potražite u članku [Azure Active Directory uvjetno pristup](active-directory-conditional-access.md).

Možete postaviti pristup uvjetno utemeljen na uređaju pravilnike za zaštitu te aplikacije:

- Office 365 SharePoint Online, radi zaštite vaše tvrtke ili ustanove web-mjesta i dokumenata
- Office 365 Exchange Online, radi zaštite vaše tvrtke ili ustanove e-pošte
- Softver kao aplikacijama servisa (SaaS) koji su povezani s Azure AD za provjeru autentičnosti
- Lokalni aplikacije koje su objavljene pomoću servisa Azure Proxy za AD aplikacije

Da biste postavili pravila pristup uvjetno utemeljen na uređaju, na portalu za Azure, idite na određenu aplikaciju u direktoriju.


  ![Popis aplikacija u imeniku Azure portala] (./media/active-directory-conditional-access-policy-connected-applications/01.png "Aplikacija")


Odaberite aplikaciju, a zatim kliknite karticu **Konfiguriraj** da biste postavili pravila uvjetnog pristup.  


  ![Konfiguriranje aplikacija] (./media/active-directory-conditional-access-policy-connected-applications/02.png "Uređaj koji se temelje pravila za pristup")




Da biste postavili pravila pristup uvjetno utemeljen na uređaju, u odjeljku **uređaj temelji pravila za pristup** **Omogućiti pravila za pristup**odaberite **na**.

Pristup utemeljen na uređaju uvjetnog pravila ima tri komponente:

- **Primjena**. Opseg korisnika koji se pravilo primjenjuje.

- **Pravila uređaja**. Uvjeti uređaj mora ispunjavati prije nego što se može pristupiti aplikacija.

- **Provođenje aplikacije**. Klijentske aplikacije (lokalnog nasuprot preglednika) pravilo primjenjuje.

  ![Tri komponente pravilo pristupom utemeljeno na uređaju] (./media/active-directory-conditional-access-policy-connected-applications/03.png "Uređaj koji se temelje pravila za pristup")


## <a name="select-the-users-the-policy-applies-to"></a>Odaberite pravilo primjenjuje na korisnike

U odjeljku **Primijeni na** možete odabrati opseg korisnika to pravilo primjenjuje se na.

Prilikom stvaranja opsega pravilnik pristupa za korisnike koji su vam dvije mogućnosti:

- **Svi korisnici**. Primjena pravila na sve korisnike koji se pristup aplikaciji.

- **Grupe**. Ograničiti pravila za korisnike koji su članovi određene grupe.

![Primijeni pravilo na sve korisnike ili grupe] (./media/active-directory-conditional-access-policy-connected-applications/11.png "Primjena")


 Da biste isključili korisnika iz pravila, uključite potvrdni okvir **osim** . To je korisno kada je potrebno da biste dodijelili dozvole za određenog korisnika za privremeno pristup aplikaciji. Odaberite tu mogućnost, na primjer, ako neki korisnici imaju uređajima koji su spremni za uvjetno pristup. Uređaje možda nije još registriran ili su uskoro će biti iz usklađenosti.


## <a name="select-the-conditions-that-devices-must-meet"></a>Odaberite uvjete koje mora zadovoljiti uređajima

Pomoću **Pravila uređaj** da biste postavili uvjete uređaj mora ispunjavati za pristup aplikaciji.

Pristup uvjetno utemeljen na uređaju možete postaviti za ove vrste uređaja:

- Ažuriranje Obljetnica na Windows 10, Windows 8.1 i Windows 7
- Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 i Windows Server 2008 R2
- uređaje sa sustavom iOS (iPad, iPhone)
- Uređaje sa sustavom android

Podrška za Mac uskoro dostupno.

  ![Primijeni pravilo na uređaje] (./media/active-directory-conditional-access-policy-connected-applications/04.png "Aplikacija")

 >[AZURE.NOTE] Informacije o razlikama između domene pridruženo Azure AD pridruženo uređaji i potražite u članku [uređaja pomoću sustava Windows 10 na svom radnom mjestu](active-directory-azureadjoin-windows10-devices.md).

Imate dvije mogućnosti za uređaj pravila:

- **Moraju biti usklađene sa svim uređajima**. Sve platforme uređaja pristup aplikaciji mora biti usklađen. Uređaje koji se izvode na platformama koji ne podržavaju pristup uvjetno utemeljen na uređaju se zabranjen pristup.

- **Samo odabrane uređaje mora biti usklađen**. Samo platforme za određeni uređaj mora biti usklađen. Druge platforme i drugim platformama koji mogu pristupiti s računala imaju pristup.

  ![Postavite doseg za pravila uređaja] (./media/active-directory-conditional-access-policy-connected-applications/05.png "Aplikacija")

Azure AD pridruženo uređaji su usklađen ako su označene kao **sukladan** u direktoriju Intune ili sustava za upravljanje mobilnim uređajima drugih proizvođača koji se integrira s Azure AD.

Na uređaju domene pridruženo je usklađena ako:

- To je registriran za Azure AD. Mnoge organizacije domene pridruženo uređaji Smatraj pouzdanom uređaja.
- Je označen kao **sukladan** u Azure AD tako da sustav centar Upravitelj konfiguracije 2016.

 ![Domene pridruženo uređaja koje nisu usklađene sa] (./media/active-directory-conditional-access-policy-connected-applications/06.png "Pravila uređaja")

Osobni uređaje sa sustavom Windows su usklađen ako su označene kao **sukladan** u direktoriju Intune ili sustava za upravljanje mobilnim uređajima drugih proizvođača koji se integrira s Azure AD.

Osobe koje nisu Windows uređaji su usklađen ako oni su označio kao **sukladan** u direktoriju Intune.

 >[AZURE.NOTE] Dodatne informacije o kako postaviti Azure AD za usklađenost s uređaja u sustavima različite upravljanja potražite u članku [uvjetno pristup Azure Active Directory](active-directory-conditional-access.md).


Možete odabrati jednu ili više platforme uređaja za pravila pristupom utemeljeno na uređaj. To obuhvaća Android, iOS, Windows Mobile (Windows 8.1 telefonima i tabletima) i Windows (sve druge Windows uređaja, uključujući sve uređaje Windows 10).
Pravilnik za procjenu pojavljuje se samo na odabrane platformama. Ako je uređaj koji pokušava pristup je pokrenut jedan od odabranih platforme, uređaj možete pristupiti aplikacije ako korisnik ima pristup. Procjenjuje nema pravila uređaja.

![Odaberite platforme za pravila uređaja] (./media/active-directory-conditional-access-policy-connected-applications/07.png "Pravila uređaja")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>Postavljanje procjene pravila za vrstu aplikacije

U odjeljku **Aplikacije prisilne** postaviti vrstu aplikacije pravilo će rezultirati za korisnika ili pristup putem uređaja.

Imate dvije mogućnosti za vrstu aplikacije da biste uključili:

- Preglednik i izvornim aplikacijama
- Izvorni aplikacije

![Odabir preglednika ili izvornim aplikacijama] (./media/active-directory-conditional-access-policy-connected-applications/08.png "Aplikacija")

Da biste nametnuli pravilnik o pristupu za aplikacije, odaberite **za preglednik i izvornim aplikacijama**. Nakon toga mogu biti:

- Preglednici (na primjer, Microsoft Edge u sustavu Windows 10) ili Safari u iOS.
- Aplikacijama koje koristite u Active Directory provjera autentičnosti biblioteke (ADAL) na bilo kojoj platformi ili koji se WebAccountManager (WAM) API u sustavu Windows 10.

>[AZURE.NOTE] Dodatne informacije o podrške za preglednike i druge napomene za korisnike koji pristupa na uređaju sustavom, certifikat za izdavanje certifikata zaštićeni aplikacije potražite u članku [uvjetno pristup Azure Active Directory](active-directory-conditional-access.md).

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>Zaštitu pristupa e-pošti iz aplikacije utemeljene na sustavu Exchange ActiveSync

U aplikacijama sustava Office 365 Exchange Online, Exchange ActiveSync možete koristiti da biste blokirali pristup e-pošte pošte utemeljen na sustavu Exchange ActiveSync aplikacijama.

![Mogućnosti za usklađenost sustava Exchange ActiveSync] (./media/active-directory-conditional-access-policy-connected-applications/09.png "Aplikacija")

![Obavezan usklađen uređaj da biste pristupili e-pošte] (./media/active-directory-conditional-access-policy-connected-applications/10.png "Aplikacija")


## <a name="next-steps"></a>Daljnji koraci

- [Azure Active Directory uvjetno pristup](active-directory-conditional-access.md)
