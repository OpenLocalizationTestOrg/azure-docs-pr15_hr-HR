<properties
    pageTitle="Azure AD Connect sinkronizacije: objašnjenje deklarativno izraza dodjeljivanje | Microsoft Azure"
    description="U članku se objašnjava deklarativno izraza za dodjelu resursa."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect sinkronizacije: objašnjenje deklarativno izraza dodjele resursa
Sinkronizacija Azure AD Connect sastavlja na deklarativno dodjele resursa najprije uvedene u Upravitelj identiteta Forefront 2010. Omogućuje implementirati dovršeno identiteta Integracija poslovne logike bez potrebe za pisanje kompilirani kod.

Ključni dio deklarativno dodjele resursa je jezik izraz koji se koristi u atribut tokova. Jezik koji se koristi je podskup Microsoft®® Visual Basic for Applications (VBA). Koristi ovaj jezik u sustavu Microsoft Office i korisnici s sučelje VBScript i prepoznat će ga. Deklarativno dodjeljivanje jezik izraz samo korištenje funkcija i nije strukturirane jezik. Nema metode ili naredbe. Umjesto toga ugnijezditi funkcija eksplicitnih program toka.

Dodatne informacije potražite u članku [Dobro došli u Visual Basic for referenca jezik aplikacije za Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

Svakako upisane atribute. Funkcija prihvaća samo atributa pravilnu vrstu. Ujedno velika i mala slova. Funkcija nazive i nazive atributa mora imati odgovarajuću kućište ili pogreška se Expression.Error.

## <a name="language-definitions-and-identifiers"></a>Web-mjesto definicije jezika i u okvir za identifikatore

- Funkcija imati naziv slijedi argumente u uglatim zagradama: naziva funkcije (argument 1, argument N).
- Atributi su označena uglatim zagradama: [attributeName]
- Parametri prepoznaju se po znak postotka: ParameterName %
- Konstante niza se staviti u navodnicima:, na primjer, "Contoso" (Napomena: morate koristiti ravne navodnike "", a ne pametnim navodnicima "")
- Numeričke vrijednosti su izražen bez navodnika i očekuje da bude broja decimalnih mjesta. Heksadecimalnu vrijednost vrijednosti su mjestu s & H. Na primjer, 98052 & HFF
- Booleove vrijednosti su izražen s konstantama: True, False.
- Ugrađeni konstante i literala izražen s samo njegovo ime: NULL, riječ o CRLF, IgnoreThisFlow

### <a name="functions"></a>Funkcija
Da biste omogućili mogućnost za pretvaranje vrijednosti atributa deklarativno dodjeljivanje koristi mnoge funkcije. Ove funkcije može se ugnijezditi tako rezultata iz jedne funkcije se prenosi u drugu funkciju.

`Function1(Function2(Function3()))`

Popis svih funkcije pronaći ćete u odjeljku [Referenca funkcije](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parametri
Parametar je definiran poveznik ili administrator pomoću komponente PowerShell. Parametri obično sadrže vrijednosti koje se razlikuju od sustava do sustava, primjerice naziv domene korisnika nalazi se u. Parametara može se koristiti u atribut tokova.

Poveznik za Active Directory sljedećih parametara namijenjeno ulazna pravila sinkronizacije:

| Naziv parametra | Komentar |
| --- | --- |
| Domain.Netbios | Oblikovanje NetBIOS domene trenutno koja se uvozi, primjerice FABRIKAMSALES |
| Domain.FQDN | Oblikovanje FQDN domene trenutno koja se uvozi, primjerice sales.fabrikam.com |
| Domain.LDAP | Oblikovanje LDAP domene trenutno koja se uvozi, primjerice Kontroler = Prodaja, Kontroler = fabrikam, Kontroler = com |
| Forest.Netbios | NetBIOS oblik naziva skupa stabala trenutno koja se uvozi, primjerice FABRIKAMCORP |
| Forest.FQDN | FQDN oblik naziva skupa stabala trenutno koja se uvozi, primjerice fabrikam.com |
| Forest.LDAP | LDAP Oblik naziva skupa stabala trenutno koja se uvozi, primjerice Kontroler = fabrikam, Kontroler = com |

Sustav nudi sljedeće parametar koji se koristi da biste dobili identifikator poveznika koji se trenutno izvode:  
`Connector.ID`

Slijedi primjer koja popunjava domenu atribut metaverse s netbios naziv domene u kojoj se korisnik nalazi:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operatori
Možete koristiti sljedeće operatore:

- **Usporedba**: <, < = <>, =, > > =
- **Matematika**: +, -, \*, -
- **Niz**: & (concatenate)
- **Logička**: & & (i) || (ili)
- **Procjena redoslijed**:)

Operatori vrednuju se slijeva nadesno i imaju isti procjenu prioritet. To je na \* (množitelj) nisu analizirane – prije (oduzimanje). 2\*(5 + 3) nije 2\*5 + 3. Da biste promijenili redoslijed procjenu kada slijeva udesno procjenu redoslijed nije odgovarajuće se koriste u uglatim zagradama ().

## <a name="multi-valued-attributes"></a>Atributi s više vrijednosti
Funkcije možete upravljati na jednom vrijednosti i s većim brojem vrijednosti atributa. Za atribute s više vrijednosti, funkcija pristajete putem svaka vrijednost i primjenjuje istu funkciju svaka vrijednost.

Ako, na primjer:  
`Trim([proxyAddresses])`Učinite Trim svaku vrijednost atributa proxyAddress.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Za svaku vrijednost s programa @-sign, zamjena domene u sustavu @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Pronađite SIP adrese i uklonite ga s vrijednostima.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o model konfiguraciju u [Razumijevanje deklarativno dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Pogledajte kako deklarativno dodjele resursa je korištenih u novom tvorničke za [Razumijevanje Zadana konfiguracija](active-directory-aadconnectsync-understanding-default-configuration.md).
- Saznajte kako promijeniti praktično pomoću deklarativno Dodjela resursa u [kako promijeniti zadanu konfiguraciju](active-directory-aadconnectsync-change-the-configuration.md).

**Pregled tema**

- [Azure AD Connect sinkronizacije: razumijevanje i Prilagodba sinkronizacije](active-directory-aadconnectsync-whatis.md)
- [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)

**Referentne teme**

- [Azure AD Connect sinkronizacije: Referenca funkcije](active-directory-aadconnectsync-functions-reference.md)
