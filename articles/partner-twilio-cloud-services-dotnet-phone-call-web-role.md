<properties 
    pageTitle="Upute za upućivanje telefonskog poziva s Twilio (.NET) | Microsoft Azure" 
    description="Saznajte kako nazvati i slanje SMS poruke sa servisom Twilio API na Azure. Primjere koda pisane .NET." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Upute za upućivanje telefonskog poziva pomoću Twilio u ulogu web-na Azure

Ovaj vodič pokazuje kako koristiti Twilio za upućivanje poziva s web-stranice smješten u Azure. Rezultat aplikacije traži od korisnika nazvati vrijednosti, kao što je prikazano u sljedećim snimku zaslona.

![Obrazac za Azure poziv pomoću Twilio i platforme ASP.NET][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Preduvjeti

Morat ćete biste pomoću koda u ovoj temi, učinite sljedeće:

1. Nabava Twilio računa i provjera autentičnosti tokena. Za početak rada s Twilio, prijavite se na [https://www.twilio.com/try-twilio][try_twilio]. Analiza cijene pri [http://www.twilio.com/pricing][twilio_pricing]. Informacije o API nudi Twilio potražite u članku [http://www.twilio.com/voice/api][twilio_api].
2. Dodavanje biblioteke Twilio .NET vaša uloga web. Potražite u odjeljku "Dodavanje biblioteka Twilio uloga projekta web" u nastavku ovog članka.

Morate biti upoznati sa stvaranjem weba uloga na Azure.

## <a name="howtocreateform"></a>Kako: Stvaranje web-obrasca za upućivanje poziva

<a id="use_nuget"></a>Da biste dodali biblioteke Twilio projekta web uloga:

1.  Otvorite rješenje u Visual Studio.
2.  Desnom tipkom miša kliknite **reference**.
3.  Kliknite **Upravljanje NuGet paketa**.
4.  Kliknite **na mreži**.
5.  U okvir za pretraživanje online upišite *twilio*.
6.  Kliknite **Instaliraj** na paket Twilio.

Sljedeći kod prikazuje kako stvoriti web-obrasca za dohvaćanje korisničkih podataka za upućivanje poziva. U ovom primjeru stvara se uloga web ASP.NET koja se naziva **TwilioCloud** .

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>Kako: Stvaranje kod da biste poziv
Sljedeći kod, koja se naziva i kada korisnik dovrši obrazac, stvara poruku poziva i generira poziv. U ovom primjeru kod pokrenuti u rukovatelj događajima onclick gumba na obrascu. (Koristi Twilio računa i provjera autentičnosti token umjesto vrijednosti rezervirano mjesto dodijeljene **accountSID** i **authToken** u kodu niže).

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

Postala je poziv, a prikazuju se krajnju točku Twilio, API verzija i status poziv. Sljedeće snimka zaslona prikazuje Izlaz iz uzorka pokretanja.

![Odgovor Azure poziv pomoću Twilio i platforme ASP.NET][twilio_dotnet_basic_form_output]

Dodatne informacije o TwiML nalazi se na [http://www.twilio.com/docs/api/twiml][twiml]. Dodatne informacije o &lt;izgovorite&gt; i druge glagoli Twilio nalazi se na [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Daljnji koraci
Da bi se prikazala osnovne funkcije pomoću Twilio u ulogu web ASP.NET na Azure nije naveden kod. Prije no što implementirate Azure u proizvodnje, preporučujemo vam da biste dodali više pogreškama ili druge značajke. Ako, na primjer:

* Umjesto korištenja web-obrasca, može koristiti spremište blobova platforme Azure ili instancu baze podataka SQL Azure za pohranu telefonske brojeve i tekst poziva. Informacije o korištenju blob-ova u Azure potražite u članku [upute za korištenje usluga spremišta blobova platforme Azure u .NET][howto_blob_storage_dotnet]. Informacije o korištenju baze podataka SQL potražite u članku [upute za korištenje baze podataka SQL Azure u aplikacijama za .NET][howto_sql_azure_dotnet].
* Možete koristiti RoleEnvironment.getConfigurationSettings za dohvaćanje ID računa Twilio i provjeru autentičnosti tokena iz postavki konfiguriranje implementaciju sustava, umjesto kodiranje vrijednosti u obrazac. Informacije o klase RoleEnvironment potražite u članku [Prostora za naziv Microsoft.WindowsAzure.ServiceRuntime][azure_runtime_ref_dotnet].
* Pročitajte upute o sigurnosti Twilio pri [https://www.twilio.com/docs/security][twilio_docs_security].
* Dodatne informacije o Twilio pri [https://www.twilio.com/docs][twilio_docs].

##<a name="seealso"></a>Vidi također
* [Kako koristiti Twilio za glas i mogućnosti SMS iz Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
