# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Koraci za praćenje kada povući ili promijenite naziv članka tehnička ACOM

Ovaj je vodič namijenjen SMEs koji su navedeni kao autor članka mora biti povučena iz odjeljka tehničku dokumentaciju azure.microsoft.com. Koraci primjenjuju se i ako je preimenovati datoteku.

Ako ste član naš Azure zajednice i mislite da članak mora biti povučena iz bilo kojeg razloga, napišite komentar u komentar strujanje Disqus članka da biste omogućili autor znati nešto nije u redu s članak.

Autori SME morate slijediti nekoliko koraka obavljanje povući sadržaj tako da korisnici web-mjesta ne morate loš iskustvo prilikom ne možemo povući sadržaj s web-mjesta. U članku brisanje ili promjena naziva mora biti preostaje koji će se dogoditi!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Korak 1: U članku postavljena na bez-indeksa/ne-prati i ponovno objaviti ga (preporučeno)

Prvo što biste trebali učiniti je ponovno objaviti u članku kao ne-indeksa/ne-prati nekoliko tjedana prije no što je zapravo izbrišete. To se smatra najbolja praksa "prije rada" za povlačenje sadržaja. U članku Time se uklanja iz kazala modula za pretraživanje da bi korisnici nećete pronaći u članku u odjeljku pretraživanje. [Interna wiki detalje potražite u članku.](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Korak 2: Upravljanje ulaznog veze (obavezno)

Određuje ima li sve veze koje nisu Microsoft ulaznog sadržaja. Često blogovi, Forumi i ostali sadržaj na webu upućuje na članke. Često, možete raditi s bloga vlasnici da biste promijenili ove veze, a možete ukloniti ili ažurirati veze iz forum objave. Alata web analytics mogu sadržavati postoje li sve veze velikog prometa ulaznog morat ćete upravljati na taj način.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Korak 3: Uklanjanje svih crosslinks u članku tehničke spremišta sadržaja (obavezno)

Je za preusmjeravanja za voditi brigu o crosslinks iz druge članke. Ažuriranje ili uklanjanje poziva na više u članku brisanje ili preimenovanje, uključujući u člancima vlasništvu drugim osobama.

1. Provjerite radite u programa ažuran lokalne grani – pokretanje `git pull upstream master` (ili odgovarajuće varijacije na tu naredbu.

2.  Skeniranje mapu azure-sadržaj-cijena/članke i mapu azure-sadržaj-cijena/obuhvaća za bilo koju članci te sadrži tu vezu da biste u članku koje želite povući i uklanjanje na crosslinks ili zamijenite odgovarajuće nove crosslinks. Možete koristiti pretraživanje i zamjena pomoćnih programa da biste pronašli na crosslinks ako nemate instaliran. Ako ih ne pomoću komponente Windows PowerShell besplatno! Evo kako možete pronaći na crosslinks pomoću komponente PowerShell:

 na. Pokrenite Windows PowerShell.

 b. Kada se zatraži PowerShell pretvorite u mapu azure sadržaj pr\articles:

 `cd azure-content-pr\articles`

 c. Upišite sljedeću naredbu, koji će se popis svih datoteka koje sadrže referencu u članku brišete:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Ako biste radije da biste poslali na popisu naziva datoteke u tekstnu datoteku (u ovom slučaju, imenovani psoutput.txt), možete učiniti sljedeće:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Dodavanje i primijenite svoje promjene, automatske ih na o ogranku i stvorili zahtjev za povlačite da biste premjestili promjene iz vaše o ogranku osnovne granu glavni spremište.

## <a name="step-4-update-the-fwlink-tool-required"></a>Korak 4: Ažuriranje alata za veza (obavezno)

Provjerite alat veza za sve veze koje možda pokažite na članak. Pokažite sve veze na zamjenski sadržaj; Ako se ne nalaze na drugo ime koje je vlasnik veze, joj se pridružiti. Ako vlasnici neće ažurirati vezu, datoteka karata s MSCOM kako bi se veza promjena. Dodatne informacije o - [Interna wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>Korak 5: Uklanjanje svih crosslinks u članku s drugih stranica na azure.microsoft.com i stvaranje preusmjeravanje za stranicu otpisa ako odgovarajuću (obavezno)

Morate raditi s osoba koja se održava i ažurira odredišna stranica dokumentacija servisa za taj dio. Ako ne znate tko je ta osoba, obratite se partneru sadržaja tima. Osoba koja se održava i ažurira odredišna stranica za dokument mora učiniti dvije stvari:

1. U Visual Studio skenirajte **cijeli** ACOM web rješenja za unakrsne reference datoteku povući. Uklanjanje unakrsne reference ili zamijenite ažurirane unakrsne reference. Morat ćete ukloniti HTML veze, kao i nizovi povezanih resursa za HTML veze. Dodatne informacije o - potražite u članku [Interna wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Ako postoji zamjena članka, stvorite je preusmjeravanje. Dodatne informacije o - potražite u članku [Interna wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx).

3. Potvrdite promjene u spremištu.

## <a name="step-6-retire-the-article"></a>Korak 6: Povući u članku

Kada dovršite prethodnih koraka i te promjene su uživo, možete izbrisati u članku u spremištu. 

**Važne:** Kada izbrišete datoteke, morate koristiti u `git add --all` naredbe.

## <a name="step-7-remove-links-from-msdn-required"></a>Korak 7: Uklanjanje veza s MSDN (obavezno)

Pregled sadržaja značajke pitanja i odgovora alat za prekinutih veza na temu otpisa ili preimenovan i uklanjanje/popravak veze u svim MSDN teme utječe na.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>8 korak: Uklanjanje predmemorirani stranica iz tražilice (samo ako je apsolutno potrebno)

Učinite ovo samo ako sadržaja mora biti uklonjen brzo zbog problema s pravnim ili loši klijenta. Po najbolja iskustva iz Google brisanja stranice običan prioritet treba samo obraditi, procesi modul prirodnim pretraživanja. Idite na te web-stranice da biste uklonili predmemorirani web-stranice iz tražilice:

[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Suradnici vodič veze

- [Članak](./../README.md)
- [Indeks članaka upute](./contributor-guide-index.md)
