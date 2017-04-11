<properties
    pageTitle="Najčešća pitanja o stanju za Azure AD Connect"
    description="U ovim najčešćim Pitanjima navedeni odgovori na pitanja o stanju povezivanje Azure AD. U ovim najčešćim Pitanjima pokriva pitanja o korištenju servisa, uključujući naplatu model, mogućnosti, ograničenja i podrška."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Najčešća pitanja o stanju sustava za Azure AD Connect

U ovim najčešćim Pitanjima navedeni odgovori na pitanja o stanju povezati Azure AD. U ovim najčešćim Pitanjima pokriva pitanja o korištenju servisa, uključujući naplatu model, mogućnosti, ograničenja i podrška.

## <a name="general-questions"></a>Općenita pitanja



**P: mogu upravljati više Azure AD imenika. Kako se prebaciti sa Azure Active Directory Premium?**

Možete se prebacivati između različitih klijenata za Azure AD tako da odaberete trenutno traje korisničko ime u gornjem desnom kutu, a zatim odaberete odgovarajući račun. Ako račun nije ovdje navedeno, odaberite Odjava, a zatim pomoću vjerodajnica globalni administrator imenika Azure Active Directory Premium omogućen za prijavu.

## <a name="installation-questions"></a>Instalacija pitanja



**P: kakav je učinak na instaliranja Azure Agent AD povežite zdravlje na pojedinačne poslužiteljima?**

Utjecaj instalacije ADFS povežite zdravlje agente komponente Microsoft Azure AD označite Web aplikacije Proxy poslužitelje, Azure AD Connect (sycn) poslužitelji kontrolera domena je minimalna vezana uz CPU propusnost mreže potrošnju memorije i prostora za pohranu.

Brojevi u nastavku su usklađivanja.

- Potrošnju procesora: ~ 1% povećanjem
- Utrošak memorije: do 10% od ukupne sistemske memorije

>[AZURE.NOTE] U slučaju agent koja se može komunicirati Azure, agenta pohranjeni podaci lokalno, do definirano maksimalno ograničenje. Agenta prebrisati "predmemoriranih" podataka na temelju "najmanje nedavno servisiran".

- Lokalni međuspremnik prostora za pohranu za Azure AD povežite zdravlje: ~ 20 MB
- Za poslužitelje servisa AD FS, preporučuje se Dodjela resursa prostora na disku 1024 MB (1 GB) za AD FS nadzora kanal za Azure AD povežite zdravlje obraditi sve podatke o nadzoru prije nego što je prebrisat.

**P: će imati ponovno Moji poslužitelji tijekom instalacije Azure agenata AD povežite zdravlje?**

ne. Instalacija sustava agenata ćete morati ponovno pokrenuti na poslužitelj. Instalacija neke pripremni korake, međutim, možda ćete morati ponovno pokretanje poslužitelja.

Ako, na primjer, na Windows Server 2008 R2 instalacije .net 4.5 Framework zahtijeva ponovno pokretanje poslužitelja.


**P: ne Azure AD povezati stanje servisa rješavati prolazni http proxy?**

Da.  Za na početak operacije, možete konfigurirati Health Agent za prosljeđivanje zahtjeva za izlazni http pomoću HTTP Proxy. Dodatne informacije potražite u članku [Konfiguriranje Azure AD povezivanje stanja agenata da biste koristili HTTP Proxy.](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
Ako morate konfigurirati proxy tijekom registracije Agent, možda ćete morati prije toga izmijenite postavke proxyja Internet Explorer.
1. Otvaranje preglednika Internet Explorer -> Postavke -> internetske mogućnosti -> veza -> Postavke LAN-a.
2. Odaberite koristite Proxy poslužitelj za svoj LAN.
3. Ako imate drugi proxy priključke za HTTP i HTTPS/sigurne, odaberite Dodatno.

**P: Azure AD povezivanje stanje servisa podržava osnovnu provjeru autentičnosti pri povezivanju s Http proxy poslužitelji?**

ne. Mehanizam za određivanje proizvoljne korisničkog imena i lozinke za osnovnu provjeru autentičnosti trenutno nisu podržani.


**P: koju verziju sustava AD DS podržava povezivanje stanja Azure AD za AD DS?**

Nadzor od AD DS nije podržana dok instalirana na sljedeće verzije s operacijskim Sustavom:

- Windows Server 2008 R2
- Windows Server 2012.
- Windows Server 2012 R2

## <a name="operations-questions"></a>Operacije pitanja



**P: morate omogućiti nadzor na web-mjesto Moje Proxy poslužitelji AD FS aplikacija ili u okvir za Moje web-aplikacije Proxy poslužiteljima?**

Ne, nadzor ne moraju biti omogućene na poslužiteljima Proxy aplikacije na Web (WAP). Omogućiti na poslužitelje servisa AD FS.


**P: kako Azure AD povezivanje upozorenja o stanju učinite se riješiti?**

Azure AD povezivanje upozorenja o stanju se riješiti uspjeh uvjetu. Azure AD povezivanje agenata stanja otkrivanje i stvaranje izvješća uvjeta za uspjeh sa servisom na temelju periodički. Nekoliko upozorenja na potiskivanje je utemeljen na vrijeme. Drugim riječima, ako istom pogreškom ne opaženih unutar 72 sata iz upozorenja generacije, upozorenje automatski riješen.




**P: koje priključaka u vatrozidu moram da biste otvorili za Azure AD povezati stanje agenta za rad?**

Morate koristiti TCP/UDP priključci 443 i 5671 otvorena za Azure AD povezivanje stanje agenta da biste mogli komunicirati s krajnje točke servisa Azure AD stanja.


**P: zašto vidite dva poslužitelja s istim nazivom na Azure AD povezivanje stanja portalu?**

Kad agent uklonili s poslužitelja, poslužitelj ne automatski uklanjaju iz Azure AD povezivanje portalu.  Ako ručno ukloniti agent s poslužitelja ili ukloniti poslužitelja sam, morate ručno izbrisati stavku poslužitelja s portala za Azure AD povezivanje stanja. Dodatne informacije potražite na [Brisanje poslužitelja ili servis instanci.](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

Ako reimaged poslužitelju ili stvoriti novi poslužitelj s istom pojedinosti (kao što je naziv računala), a niste uklonite već registrirane poslužitelj s portala za Azure AD povežite zdravlje, instalirati agenta na novom poslužitelju, vidjet ćete dvije stavke s istim nazivom.  
U ovom slučaju je potrebno izbrisati stavku pripadaju starije poslužitelja. Podaci za ovaj poslužitelj moraju biti zastarjele.

## <a name="related-links"></a>Srodne veze

* [Azure AD Connect stanja](active-directory-aadconnect-health.md)
* [Stanje Agent za instalaciju za Azure AD Connect](active-directory-aadconnect-health-agent-install.md)
* [Stanje postupci za Azure AD Connect](active-directory-aadconnect-health-operations.md)
* [Korištenje Azure AD povezati stanja AD fs](active-directory-aadconnect-health-adfs.md)
* [Korištenje stanja povezati Azure AD za sinkronizaciju](active-directory-aadconnect-health-sync.md)
* [Stanje pomoću Azure AD povezati s AD DS](active-directory-aadconnect-health-adds.md)
* [Povijest verzija stanja za Azure AD Connect](active-directory-aadconnect-health-version-history.md)
