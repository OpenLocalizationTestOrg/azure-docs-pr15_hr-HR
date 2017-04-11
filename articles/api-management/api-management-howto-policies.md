<properties 
    pageTitle="Pravila u odjeljku Upravljanje Azure API | Microsoft Azure" 
    description="Saznajte kako možete stvarati, uređivati i konfiguriranje pravilnika o u odjeljku Upravljanje API-JA." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


#<a name="policies-in-azure-api-management"></a>Pravila u odjeljku Upravljanje Azure API-JA

U odjeljku Azure API upravljanje, pravila su naprednih mogućnosti sustava koje omogućuju publisher da biste promijenili ponašanje API kroz konfiguraciju. Pravila su skup izraza koji se izvode sekvencijalno na zahtjev ili odgovor API. Popularne naredbe uključiti Pretvaranje oblika iz XML-a JSON i nazovite stopa ograničavanje da biste ograničili količinu dolazne pozive iz razvojni inženjer. Mnogo više pravila dostupnih iz okvir.

Potražite u članku [Referenca za pravila][] za cijeli popis izraze pravila i njihove postavke.

Pravila primjenjuju se pristupnika koje se nalazi između korisničke API-JA i upravljani API-JA. Pristupnik prima sve zahtjeve i obično prosljeđuje ih nepromijenjen temeljni API-JA. No pravilo možete primijeniti promjene zahtjev za ulazni i izlazni odgovora.

Pravilnik o izrazima može se koristiti kao vrijednosti atributa ili tekstne vrijednosti u bilo kojem od pravilnike za upravljanje API osim ako pravilo određuje u suprotnom. Neka pravila kao što su pravila [tijek kontrolu][] i [Postavljanje varijable][] temelje se na pravilnik o izrazima. Dodatne informacije potražite u članku [Napredne pravilnike][] i [pravila izrazima][].

## <a name="scopes"> </a>Kako konfigurirati pravila
Pravila se može konfigurirati globalno ili u dosegu [proizvoda][], [API-JA][] ili [operacija][]. Da biste konfigurirali pravilo, pomaknite se do uređivaču pravila na portalu za publisher.

![Izbornik pravila][policies-menu]

Uređivač pravila sastoji se od tri glavna dijela: opseg pravila (gore), definiciju pravila kojima pravila uređivanja (lijevo) i naredbe popisa (desno):

![Uređivanje pravila][policies-editor]

Da biste započeli s konfiguracijom pravilnika, morate odabrati opseg primjene pravila. Proizvod je odabran u zaslona u nastavku **Starter** . Imajte na umu da kvadratne simbol uz naziv pravila označava da su pravila već primijenjene na ovoj razini.

![Opseg][policies-scope]

Budući da se pravilo već primijenjen, konfiguraciju prikazuju se u prikazu definicija.

![Konfiguriranje][policies-configure]

Pravila prikazuje se samo za čitanje isprva. Da biste mogli uređivati definiciju kliknite akciju **Konfiguriranje pravilnika** .

![Uređivanje][policies-edit]

Definicija pravila je jednostavan XML dokument koji opisuje nizu ulazni i izlazni izvješća. XML se može uređivati izravno u prozoru. Popis naredbi navedeni su udesno i naredbe koje se primjenjuju na trenutnom opsegu su omogućeno i istaknuta; kao što je prikazano izjava **Stopa poziva ograničenje** u snimku zaslona koja se nalazi iznad.

Klikom na omogućeno izjava dodat će odgovarajuće XML na mjestu gdje se nalazi pokazivač u prikazu definicija. 

>[AZURE.NOTE] Ako pravilo koje želite dodati nije omogućena, provjerite radite li u odgovarajuće opseg za to pravilo. Svaki izjave pravila namijenjen je za korištenje u određenim opsega i sekcije pravila. Da biste pregledali pravila sekcije i opsega pravilo, potvrdite okvir u odjeljku **Korištenje** tog pravila u odjeljku [Referenca pravila][].

Potpuni popis izjave pravila i njihove postavke dostupnih u odjeljku [Referenca pravila][].

Na primjer, da biste dodali novi izjava da biste ograničili zahtjevi za navedeni IP adrese, postavite pokazivač ispred sadržaj u `inbound` XML elemenata i kliknite naredbu **Ograniči pozivatelja IP-ovi** .

![Ograničenja pravila][policies-restrict]

Ovo će dodati u isječak XML da biste na `inbound` element koji sadrži smjernice o tome kako konfigurirati naredbu.

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

Da biste ograničili dolazni zahtjevi i prihvatiti samo one preuzete iz IP adresu 1.2.3.4 izmjena XML na sljedeći način:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![Spremi][policies-save]

Kada dovršite konfiguriranje naredbe za pravilo, kliknite **Spremi** , a promjene će se propagirati pristupnika za upravljanje API odmah.

##<a name="sections"> </a>Konfiguracija razumijevanje pravila

Pravilo je niz naredbi koje se izvršiti da bi zahtjeva i odgovora. Konfiguracija podijeljen pravilno `inbound`, `backend`, `outbound`, i `on-error` sekcije kao što je prikazano u sljedećim konfiguracije.

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

Ako je došlo do pogreške tijekom obrade zahtjeva, sve preostale korake u na `inbound`, `backend`, ili `outbound` preskaču sekcije i izvođenja prelazi izvješća u na `on-error` sekciju. Potvrđivanjem izraze pravila u na `on-error` sekciju možete pregledati pogreške pomoću na `context.LastError` svojstvo, pregledati i prilagoditi pomoću pogreške odgovora na `set-body` pravila, i konfiguriranje što se događa ako dođe do pogreške. Postoje šifre pogrešaka za ugrađene korake i pogreške koje se mogu pojaviti tijekom obrade izjave pravila. Dodatne informacije potražite u članku [Rukovanje pogreškama u pravila upravljanja API -JA](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Budući da možete navesti pravila na različitim razinama (globalnu, proizvoda, API-ja i operacije) konfiguraciju nudi omogućuje vam da biste odredili redoslijed kojim izjave pravila definition izvršavanje vezana uz nadređenog pravila. 

Pravilima dosega vrednuju se sljedećim redoslijedom.

1. Globalni opseg
2. Opseg proizvoda
3. Opseg API-JA
4. Operacija dosega

Naredbe u njima se procjenjuju prema položaj na `base` elementa, ako postoje.

Ako, na primjer, ako imate pravilo na razinu globalne i pravila konfigurirana za API, pa svaki put kada se taj određeni API će se koristiti oba pravila će se primijeniti. Upravljanje API omogućuje deterministic redoslijed izraze kombinirane pravila putem osnovni element. 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

U definiciji pravila primjer gore navedena u `cross-domain` izjava želite izvršiti prije veći pravilnicima za koje želite u uključivanje, iza u `find-and-replace` pravila.

Ako se ista pravila dvaput u izjavu pravila, primjenjuje se najčešće nedavno provjeriti u odnosu pravila. To možete koristiti da biste nadjačali pravilnike koji su definirani u noviji dosegu. Da biste vidjeli pravila u trenutnom opsegu u uređivaču pravila, kliknite **ponovni izračun učinkovitih pravila za odabrani opseg**.

Imajte na umu da globalno pravilo ima nema nadređene pravila i koristite na `<base>` element u njoj ne utječe. 

## <a name="next-steps"></a>Daljnji koraci

Pogledajte slijedi videozapis na pravilnik o izrazima.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Referenca za pravila]: api-management-policy-reference.md
[Proizvoda]: api-management-howto-add-products.md
[API-JA]: api-management-howto-add-products.md#add-apis 
[Postupak]: api-management-howto-add-operations.md

[Napredni pravila]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Kontrola toka]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Postavljanje varijable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Pravilnik o izrazima]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
