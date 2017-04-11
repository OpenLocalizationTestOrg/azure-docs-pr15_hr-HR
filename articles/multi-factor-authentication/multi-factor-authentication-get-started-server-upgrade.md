<properties 
    pageTitle="Agent za PhoneFactor nadogradnje na poslužitelj za Azure višestruka provjera autentičnosti"
    description="U ovom dokumentu opisuje za početak rada s poslužiteljem MFA Azure i Nadogradnja starijih phonefactor agent."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="upgrading-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Agent za PhoneFactor nadogradnje na poslužitelj za Azure višestruka provjera autentičnosti

Nadogradnja s v5.x PhoneFactor Agent ili stariji na poslužitelj za Azure višestruke provjere autentičnosti zahtijeva PhoneFactor Agent i matičnom komponente za deinstalaciju prije nego višestruke provjere autentičnosti i njegove matičnom komponente moguće je instalirati.

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Da biste nadogradili PhoneFactor Agent poslužitelj za Azure višestruke provjere autentičnosti
<ol>
<li>Najprije sigurnosno kopirajte PhoneFactor podatkovne datoteke. Zadano mjesto za instalaciju je C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Ako je instaliran portalu korisnika:</li>
<ol>
<li>Dođite do mape u instaliranje i sigurnosno kopiranje web.config datoteke. Zadano mjesto za instalaciju je C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Ako ste dodali prilagođene teme portalu, stvorite sigurnosnu kopiju prilagođene mape ispod C:\inetpub\wwwroot\PhoneFactor\App_Themes direktorija.</li>


<li>Deinstalirajte korisnički Portal putem PhoneFactor Agent (dostupno samo ako je instaliran na istom poslužitelju kao PhoneFactor Agent) ili putem Windows programi i značajke.</li></ol>




<li>Ako je instalirana web-servisa za mobilne aplikacije:
<ol>
<li>Otvorite mapu instaliranje i sigurnosno kopiranje web.config datoteke. Zadano mjesto za instalaciju je C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Deinstalacija mobilne aplikacije web-usluge putem Windows programi i značajke.</li></ol>

<li>Ako je instaliran servis SDK Web, deinstalirajte ga putem PhoneFactor Agent ili putem Windows programi i značajke.

<li>Deinstalacija Agent PhoneFactor kroz Windows programi i značajke.

<li>Instalirajte višestruka provjera autentičnosti poslužitelja. Imajte na umu da se put instalacije je izdvojiti iz registra s prethodne instalacije PhoneFactor Agent pa ga instalirajte na istom mjestu (npr. C:\Program Files\PhoneFactor). Nove instalacije će imati različite zadane instalacije put (npr. C:\Program Files\Multi dvofaktorska analiza varijance provjeru autentičnosti poslužitelja). Tijekom instalacije, trebali biste moguća podatkovnu datoteku ostavili prethodne PhoneFactor Agent da bi korisnika i postavke trebali biste i dalje se nakon instalacije nova višestruku poslužiteljima za provjeru autentičnosti.

<li>Ako se to od vas zatraži, aktivirati na višestruke provjere autentičnosti i provjerite je li što vam je dodijeljen grupi točan replikacije.

<li>Ako servis SDK Web prethodno instalirali, instalirajte novi Web servisa SDK putem korisničkog sučelja višestruke provjere autentičnosti poslužitelja. Imajte na umu da zadani naziv virtualnog direktorija sada "MultiFactorAuthWebServiceSdk" umjesto "PhoneFactorWebServiceSdk". Ako želite koristiti prethodni naziv, morate promijeniti naziv virtualnog direktorija tijekom instalacije. U suprotnom, ako dopustite Instaliraj da biste koristili zadani naziv, morat ćete promijeniti URL u sve aplikacije koje se odnose na servis SDK Web kao što su korisnički Portal i mobilne aplikacije web-servisa pokažite na odgovarajuće mjesto.

<li>Ako na poslužitelj Agent PhoneFactor prethodno instaliran portalu korisnika, instalirajte novi višestruke provjere autentičnosti korisnika Portal putem korisničkog sučelja višestruke provjere autentičnosti poslužitelja. Imajte na umu da zadani naziv virtualnog direktorija sada "MultiFactorAuth" umjesto "PhoneFactor". Ako želite koristiti prethodni naziv, morate promijeniti naziv virtualnog direktorija tijekom instalacije. U suprotnom, ako dopustite Instaliraj da biste koristili zadani naziv, trebali biste kliknite ikonu korisnički Portal na poslužitelju višestruke provjere autentičnosti i ažuriranje URL portala korisnika na kartici postavke.

<li>Ako je Portal za korisnika i/ili mobilne aplikacije web-servisa već instalirano na drugi poslužitelj agenta PhoneFactor:
<ol>
<li>Idite na mjesto za instalaciju (primjerice C:\Program Files\PhoneFactor) i kopirajte odgovarajući installer(s) na poslužitelj. Postoje 32-bitni i 64-bitnu instalaciju za korisnički Portal i mobilne aplikacije web-servisa. Se još nazivaju MultiFactorAuthenticationUserPortalSetupXX.msi i MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi redom.</li>
<li>Da biste instalirali Portal korisnika na web-poslužitelju, otvorite naredbeni redak kao administrator i pokretanje u MultiFactorAuthenticationUserPortalSetupXX.msi. Imajte na umu da zadani naziv virtualnog direktorija sada "MultiFactorAuth" umjesto "PhoneFactor". Ako želite koristiti prethodni naziv, morate promijeniti naziv virtualnog direktorija tijekom instalacije. U suprotnom, ako dopustite Instaliraj da biste koristili zadani naziv, trebali biste kliknite ikonu korisnički Portal na poslužitelju višestruke provjere autentičnosti i ažuriranje URL portala korisnika na kartici postavke. Postojeći korisnici moraju biti obaviješteni o novi URL.</li>
<li>Otvorite portal korisnika instalirati mjesta (npr. C:\inetpub\wwwroot\MultiFactorAuth) i uređivanje web.config datoteke. Kopirajte vrijednosti u appSettings i applicationSettings sekcije iz izvorne datoteke web.config koje je sigurnosno prije nadogradnje u novu datoteku web.config. Ako se novi naziv virtualnog direktorija zadani je zadržane prilikom instalacije servisa SDK Web promijeniti URL-a u odjeljku applicationSettings tako da pokazuje na odgovarajuće mjesto. Ostale zadane vrijednosti su promijenjene u prethodno web.config datoteke, novu datoteku web.config Primjena te iste promjene.</li>
<li>Da biste instalirali mobilne aplikacije web-servisa na web-poslužitelju, otvorite naredbeni redak kao administrator i pokretanje u MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi. Imajte na umu da zadani naziv virtualnog direktorija sada "MultiFactorAuthMobileAppWebService" umjesto "PhoneFactorPhoneAppWebService". Ako želite koristiti prethodni naziv, morate promijeniti naziv virtualnog direktorija tijekom instalacije. Preporučujemo vam da biste odabrali kraći naziv da biste lakše za krajnje korisnike da biste unijeli u na mobilnim uređajima. U suprotnom, ako dopustite Instaliraj da biste koristili zadani naziv, trebali biste kliknite ikonu za mobilnu aplikaciju na poslužitelju višestruke provjere autentičnosti i ažuriranje URL za mobilne aplikacije web-servisa.</li>
<li>Idite na web-uslugu mobilne aplikacije instalacija mjesta (npr. C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) i uređivanje datoteke web.config. Kopirajte vrijednosti u appSettings i applicationSettings sekcije iz izvorne datoteke web.config koje je sigurnosno prije nadogradnje u novu datoteku web.config. Ako se novi naziv virtualnog direktorija zadani je zadržane prilikom instalacije servisa SDK Web promijeniti URL-a u odjeljku applicationSettings tako da pokazuje na odgovarajuće mjesto. Ostale zadane vrijednosti promijenjene u prethodno web.config datoteke, novu datoteku web.config Primjena te iste promjene.</li></ol>
