<properties 
    pageTitle="Integriranje sustava lokalnih identiteta sa Azure Active Directory."
    description="Ovo je Azure AD Connect koji opisuje što je to i zašto je želite koristiti."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Višestruka provjera autentičnosti sastavnih u prilagođene aplikacije (SDK)

> [AZURE.IMPORTANT]  Ako želite da biste preuzeli SDK-a, morat ćete stvoriti davatelja usluge provjere autentičnosti višestruku Azure čak i ako imate Azure MFA, AAD Premium ili EMS licence.  Ako stvorite davatelja usluge provjere autentičnosti višestruku Azure u tu svrhu, a već imate licence, je potreban je stvorili davatelja s modelom **Po korisničkih** i povezati davatelj direktorij koji sadrži Azure MFA, Azure AD Premium ili EMS licence.  To će osigurati da se naplaćuju osim ako imate više jedinstvenih korisnika pomoću SDK od broj licenci koje ste vlasnik.

Na Azure višestruke provjere autentičnosti Software Development Kit (SDK) omogućuje vam stvaranje telefonski poziv i tekst poruke provjere valjanosti izravno u prijavu ili transakcije procesa aplikacija u klijentu za Azure AD.

Višestruke provjere autentičnosti SDK dostupna je za C#, Visual Basic (.NET), Java, Perl, PHP i Ruby. SDK sadrži Tanki omot oko višestruke provjere autentičnosti. Obuhvaća sve što trebate svoj kod, uključujući komentirane izvorne datoteke kod, primjer datoteke i detaljne datoteke ReadMe. Svaki SDK sadrži i certifikat i privatni ključ za šifriranje transakcije koja je jedinstvena davatelja višestruke provjere autentičnosti. Pod uvjetom da imate davatelja, možete preuzeti SDK u proizvoljan broj jezika i oblici god želite.

Struktura API-ji u SDK višestruke provjere autentičnosti je prilično jednostavan. Napravite jednu funkciju poziva API-JA s parametrima mogućnost multi-factor, kao što su način provjere valjanosti i korisničkih podataka, kao što je telefonski broj telefona ili broj PIN-a za provjeru valjanosti. API-ji prevesti funkcija poziv u zahtjeva za uslugu web oblaku Azure multi-factor servis za provjeru autentičnosti. Sve pozive mora sadržavati reference na privatne certifikat koji je uključen u svakoj SDK.

Jer API-ji ima pristup korisnicima registriran u Azure Active Directory, morate unijeti informacije o korisniku, kao što su telefonski brojevi i šifre PIN-a, datoteke ili u bazi podataka. Osim toga, API-ji ne nude značajke upravljanja registraciju ili korisnika, tako da potrebne za izradu ti procesi u aplikaciji.






## <a name="download-the-azure-multi-factor-authentication-sdk"></a>Preuzimanje Azure višestruke provjere autentičnosti SDK

Preuzimanje višestruku SDK Azure potreban je [Davatelj usluge provjere autentičnosti za Azure višestruke provjere](multi-factor-authentication-get-started-auth-provider.md).  To zahtijeva puno Azure pretplatu, čak i ako su vlasnik Azure MFA, Azure AD Premium ili paket Enterprise mobilnost licenci.  Da biste preuzeli SDK-a, morate prijeći Portal za upravljanje multi-factor upravljanjem davatelja provjere autentičnosti višestruku izravno ili tako da kliknete vezu **"Otvorite portal"** na stranici Postavke servisa MFA.


### <a name="to-download-the-azure-multi-factor-authentication-sdk-from-the-azure-portal"></a>Da biste preuzeli SDK Azure višestruke provjere autentičnosti na portalu za Azure


1. Prijavite se na Portal za Azure kao Administrator.
2. Na lijevoj strani odaberite servisa Active Directory.
3. Na stranici servisa Active Directory, pri vrhu kliknite **Davatelji provjere autentičnosti višestruke provjere**
4. Pri dnu kliknite **Upravljanje**
5. Otvorit će se nova stranica.  Na lijevoj strani, pri dnu kliknite SDK.
<center>![Preuzimanje](./media/multi-factor-authentication-sdk/download.png)</center>
6. Odaberite jezik koji želite i kliknite u jednoj pridružene preuzimanje veze.
7. Spremite preuzimanje.



### <a name="to-download-the-azure-multi-factor-authentication-sdk-via-the-service-settings"></a>Da biste preuzeli Azure višestruke provjere autentičnosti SDK putem postavke servisa


1. Prijavite se na Portal za Azure kao Administrator.
2. Na lijevoj strani odaberite servisa Active Directory.
3. Dvaput pritisnite instanci programa Azure AD.
4. Pri vrhu kliknite **Konfiguriraj**
5. U odjeljku višestruka provjera autentičnosti odaberite **Postavke servisa za upravljanje**
![preuzimanje](./media/multi-factor-authentication-sdk/download2.png)
6. Na stranici Postavke servisa pri dnu zaslona kliknite **Idi na portal**.
![Preuzimanje](./media/multi-factor-authentication-sdk/download3a.png)
7. Otvorit će se nova stranica.  Na lijevoj strani, pri dnu kliknite SDK.
8. Odaberite jezik koji želite i kliknite u jednoj pridružene preuzimanje veze.
9. Spremite preuzimanje.

## <a name="contents-of-the-azure-multi-factor-authentication-sdk"></a>Sadržaj Azure višestruke provjere autentičnosti SDK
Unutar SDK neku od sljedećih stavki:

- **Datoteka pročitajme za**. U članku se objašnjava kako koristiti višestruke provjere autentičnosti API-ji u novu ili postojeću aplikacije.
- **Izvorne datoteke** za višestruka provjera autentičnosti
- **Klijentski certifikat** koji koristite za komunikaciju sa servisom višestruka provjera autentičnosti
- **Privatni ključ** certifikata
- **Nazovite rezultate.** Popis kodova rezultat poziv. Da biste otvorili datoteku, pomoću aplikacije s oblikovanjem, kao što su WordPad teksta. Pomoću kodova za rezultat poziva da biste testirali i otklanjanje poteškoća prilikom implementacije višestruke provjere autentičnosti u aplikaciji. Nisu šifre statusa provjere autentičnosti.
- **Primjeri.** Ogledni kod za Osnovni radni implementacija višestruke provjere autentičnosti.


>[AZURE.WARNING]Na klijentskom certifikat je jedinstveni privatne koja je stvorena za vas. Zajedničko korištenje ili izgubiti tu datoteku. Nije ključ za osiguravanje sigurnost komunikacije sa servisom višestruke provjere autentičnosti.

## <a name="code-sample-standard-mode-phone-verification"></a>Uzorak koda: Standardnom načinu telefona potvrdu

Ovaj uzorak koda pokazuje kako koristiti API-ji u SDK Azure višestruke provjere autentičnosti standardnom načinu govorne poziv potvrdu da biste dodali aplikacije. Standardni način je telefonski poziv koji korisnik ne odgovori na pritiskom na tipku #.

U ovom se primjeru koristi C# .NET 2.0 višestruke provjere autentičnosti SDK u osnovni ASP.NET aplikacija s C# poslužiteljsko, ali postupak vrlo je sličan za jednostavne implementacije na drugim jezicima. Jer SDK sadrži izvorne datoteke, ne izvršne datoteke sastavljanje datoteke i uputiti na njih ili ih uključiti izravno u aplikaciji.

>[AZURE.NOTE]Kada implementacijom višestruka provjera autentičnosti pomoću dodatnih čimbenika kao sekundarnom ili tertiary potvrdu dodatku izjavi vaš primarni način provjere autentičnosti. Načini su osmišljeni tako će se koristiti kao metoda primarni provjere autentičnosti.

### <a name="code-sample-overview"></a>Pregled uzorak koda
Kod uzorka za vrlo jednostavne zadržavanja pokazni videozapis koristi telefonskog poziva s odgovorom ključa # da biste dovršili provjere autentičnosti korisnika. Taj faktor telefonski poziv naziva u višestruka provjera autentičnosti standardnom načinu rada.

Kodom na strani klijenta ne uključuje sve višestruke provjere autentičnosti specifične za elemente. Budući čimbenike dodatno unositi neovisno o primarni provjere autentičnosti, možete ih dodati bez promjene postojećih sučelje za prijavu. API-ji u SDK višestruku omogućuju vam prilagodbu korisničkog sučelja, ali ne morate ništa mijenjati uopće.

Kod poslužiteljsko dodaje standardna načina provjere autentičnosti u koraku 2. Stvara PfAuthParams objekt s parametrima koji su potrebni za potvrdu standardna načinu: korisničko ime, telefonski broj, i načinu rada, a put do klijentski certifikat (CertFilePath), koji je potreban u svaki poziv. Pokaz sve parametre u PfAuthParams potražite u članku oglednu datoteku u SDK-a.

Nakon toga kod prosljeđuje objekt PfAuthParams funkciju pf_authenticate(). Povratna vrijednost označava uspjelo ili nije u provjere autentičnosti. Izvan parametrima, callStatus i errorID, sadrže podatke za rezultat dodatne poziv. Šifre poziv rezultat su navedenih u datoteku rezultata poziv u SDK-a.

Ova minimalnog implementacija možete staviti u samo nekoliko redaka. No u kodu radnog će uključiti da više sofisticirane pogreškama, kod dodatne baze podataka i poboljšane doživljaja.

### <a name="web-client-code"></a>Kod klijenta za web

Sljedeći kod web klijent za stranicu pokazni videozapis.


    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Kod na strani poslužitelja

U sljedećem kodu poslužiteljsko višestruka provjera autentičnosti je konfiguriran i pokretanje u koraku 2. Standardnom načinu rada (MODE_STANDARD) je telefonski poziv na koji korisnik ne odgovori pritiskom na tipku #.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "9134884271";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = " Multi-Factor Authentication failed.";
                }
            }

        }
    }
